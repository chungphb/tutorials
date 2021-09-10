# OpenVPN

## Installing

### How to install

#### Install OpenVPN

1. Update the package lists and install `EPEL`.

   ```bash
   $ sudo yum update -y
   $ sudo yum install epel-release -y
   $ sudo yum update -y
   ```

2. Install **OpenVPN**.

   ```bash
   $ sudo yum install -y openvpn
   ```

#### Install Easy RSA

1. Download and extract the `Easy RSA` directory.

   ```bash
   $ sudo yum install -y wget
   $ wget https://github.com/OpenVPN/easy-rsa/archive/v3.0.8.tar.gz
   $ tar -xf v3.0.8.tar.gz
   ```

2. Copy  the `Easy RSA` directory to the `/etc/openvpn` directory.

   ```bash
   $ sudo mkdir /etc/openvpn/easy-rsa
   $ sudo mv /root/easy-rsa-3.0.8 /etc/openvpn/easy-rsa
   $ sudo chown [YOUR_ACCOUNT] /etc/openvpn/easy-rsa
   ```

#### Generate Keys and Certificates

1. Create a directory to store the generated keys and certificates.

   ```bash
   $ sudo mkdir /etc/openvpn/easy-rsa/keys
   ```

2. Update certificate variables.

   ```bash
   $ sudo vi /etc/openvpn/easy-rsa/vars
   export KEY_COUNTRY="YOUR COUNTRY"
   export KEY_PROVINCE="YOUR PROVINCE"
   export KEY_CITY="YOUR CITY"
   export KEY_ORG="YOUR ORGANIZATION"
   export KEY_EMAIL="YOUR.EMAIL@example.com"
   export KEY_EMAIL=YOUR.EMAIL@example.com
   export KEY_CN=YOUR.COMMON.NAME.com
   export KEY_NAME="YOUR NAME"
   export KEY_OU="YOUR ORGANIZATION UNIT"
   ```

3. Move into the `/etc/openvpn/easy-rsa` directory.

   ```bash
   $ cd /etc/openvpn/easy-rsa
   $ source ./vars
   ```

4. Remove existing keys and certificates.

   ```bash
   $ ./clean-all
   ```

5. Build the certificate authority.

   ```bash
   $ ./build-ca
   ```

6. Generate a key and certificate for the server.

   ```bash
   $ ./build-key-server server
   ```

7. Generate a Diffie-Hellman key exchange file.

   ```bash
   $ ./build-dh
   ```

8. Generate keys and certificates for clients.

   ```bash
   $ ./build-key client
   ```

9. Rename a versioned OpenSSL configuration file to a version-less name.

   ```bash
   $ cp /etc/openvpn/easy-rsa/openssl-1.0.0.cnf /etc/openvpn/easy-rsa/openssl.cnf
   ```

### Note

* The client name `client` is of your choice.

## Configuration

### How to configure

1. Copy a sample configuration file to the `/etc/openvpn` directory.

   ```bash
   $ sudo /usr/share/doc/openvpn-2.4.11/sample/sample-config-files/server.conf /etc/openvpn
   ```

2. Update/Uncomment the following fields in the `server.conf` configuration file.

   ```
   push "redirect-gateway def1 bypass-dhcp"
   push "dhcp-option DNS 8.8.8.8"
   push "dhcp-option DNS 8.8.4.4"
   user nobody
   group nobody
   topology subnet
   ;tls-auth ta.key 0
   tls-crypt myvpn.tlsauth
   ```

3. Generate the `myvpn.tlsauth` static encryption key.

   ```bash
   sudo openvpn --genkey --secret /etc/openvpn/myvpn.tlsauth
   ```

### Note

- The static encryption key `myvpn` is of your choice.

## Routing

### How to set up using FirewallD 

1. Find the active zone.

   ```bash
   $ sudo firewall-cmd --get-active-zones
   ```

2. Add the `openvpn` service to the list of services allowed by **FirewallD** within the active zone.

   ```bash
   $ sudo firewall-cmd --zone=trusted --add-service openvpn
   $ sudo firewall-cmd --zone=trusted --add-service openvpn --permanent
   ```

3. Verify whether the `openvpn` service was added correctly.

   ```bash
   $ sudo firewall-cmd --list-services --zone=trusted
   ```

4. Add a masquerade to the current runtime instance.

   ```bash
   $ sudo firewall-cmd --add-masquerade
   $ sudo firewall-cmd --add-masquerade --permanent
   ```

5. Verify whether the masquerade was added correctly.

   ```bash
   $ sudo firewall-cmd --query-masquerade
   ```

6. Forward routing to the **OpenVPN** subnet.

   ```bash
   $ VAR=$(ip route get 8.8.8.8 | awk 'NR==1 {print $(NF-2)}')
   $ sudo firewall-cmd --permanent --direct --passthrough ipv4 -t nat -A POSTROUTING -s 10.8.0.0/24 -o $VAR -j MASQUERADE
   $ sudo firewall-cmd --reload
   ```

7. Enable IP forwarding.

   ```bash
   $ sudo vi /etc/sysctl.conf
   net.ipv4.ip_forward = 1
   ```

8. Restart the network service.

   ```bash
   $ sudo systemctl restart network.service
   ```

### Note

## Running

### How to run

#### Server

* Start the service.

  ```bash
  $ sudo systemctl start openvpn@server.service
  ```

* Enable the service to start up at boot.

  ```bash
  $ sudo systemctl -f enable openvpn@server.service
  ```

* Verify whether the service is active.

  ```bash
  $ sudo systemctl status openvpn@server.service
  ```

#### Client

1. Copy the following files from the server to the client.

   ```
   /etc/openvpn/easy-rsa/keys/ca.crt
   /etc/openvpn/easy-rsa/keys/client.crt
   /etc/openvpn/easy-rsa/keys/client.key
   /etc/openvpn/myvpn.tlsauth
   ```

2. Create the `client.ovpn` configuration file.

   ```
   client
   tls-client
   ca ~/client/ca.crt
   cert ~/client/client.crt
   key ~/client/client.key
   tls-crypt ~/client/myvpn.tlsauth
   proto udp
   remote [YOUR_SERVER_IP] 1194 udp
   dev tun
   topology subnet
   pull
   user nobody
   group nobody
   auth-user-pass
   comp-lzo
   ```

3. Connect a client to **OpenVPN**.

   ```bash
   $ sudo openvpn --config ~/client/client.ovpn
   ```

### Note

* The path `~/client/` to the keys, the certificates and the client configuration file is of your choice.
* [How to create authorized users for OpenVPN clients.](https://openvpn.net/faq/how-to-add-authorized-users-to-the-vpn/)

## References

[How To Set Up and Configure an OpenVPN Server on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-centos-7)

[How to Install OpenVPN on CentOS 7 or 8](https://phoenixnap.com/kb/openvpn-centos)
