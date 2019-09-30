# offline_nifi_ansibleized
Using Ansible playbooks in concert with other user's roles (@cavemandaveman) to simplify the configuration and deployment of various types of NiFi configurations - in an offline mode (where you cannot reach the Interwebs)

## Files
* remove_nifi.yml - Remove all traces of nifi from your systems to start anew
* http_server.yml - Create an offline http server (needed for each follow-on task)
* single_node.yml - Create a single nifi instance from your own (also created) offline http server 
* multi_node.yml - Create several nifi servers that cluster to support a flow
* multi_tls_node.yml - Create several nifi servers that cluster to support a flow using encryption and client authentication
* inventory (staging.yml for me) - Ensure you have at least a nifiservers group and at least one system/VM referenced below:
  * `[nifiservers]`
  * `nifi1.test.local`

## Prep
1. Build up to 3 VMs/machines ideally with RHEL 7.x with fqdns in your /etc/hosts that match staging.yml (or whatever you call your inventory)
1. Ensure that the 3 VMs/machines have the ability to pull package for update/install from a local repository. We'll at least need:
   1. openssl
   1. java
   1. httpd
   1. ansible
1. Configure ssh keys and sudo privileges for your systems and your ansible user. The ansible user will need to *become*
1. Get a copy for offline usage of cavemandaveman.nifi
   1. For simplicity sake, edit /etc/hosts and add a line for your first nifiserver instance (nifi1.test.local in my case) to include: apache.cs.utah.edu. i.e.,
`192.168.x.x <myhostname.fqdn> <myhostname> apache.cs.utah.edu.` 
1. Get the nifi-toolkit-x.x.x matching current version downloaded. 
1. Create a role to store your work
   1. `ansible-galaxy init nifi` 
   1. Download the latest nifi-x.x.x-bin.tar.gz and store in roles/nifi/files/
   1. Copy the provided roles/* over your own


## Running the examples
### Create a single node with no login creds nor encrypted link

`ansible-playbook -i staging.yml remove_nifi.yml http_server.yml single_node.yml`

### Create a multi node nifi setup with no login creds nor encrypted link

`ansible-playbook -i staging.yml remove_nifi.yml http_server.yml multi_node.yml` 

### Create a multi node nifi setup with TLS and default client certificate to load into your browser

Let's create ssl keys for our systems quick

`tls-toolkit.sh standalone -n 'nifi1.test.local,nifi2.test.local,nifi3.test.local' -C 'CN=nifitest, OU=ApacheNiFi' -o certs -O`
Build a vault with your keys to reference from our playbooks:

`for file in certs/nifi*.test.local; do echo ${file##certs/}; egrep -r "keystorePasswd|keyPasswd|truststorePasswd" $file/nifi.properties | cut -d. -f3; done`

You'll want to create the above ^^^ cert information into separate yml variable files per host, i.e., host_vars. So, for instance, copy nifi1.test.local's output and paste into:

`ansible-vault edit host_vars/nifi1.test.local.yml`
Also, be sure to import the p12 certificate with password for client login to your new TLS protected cluster!

`ansible-playbook --ask-vault-pass -i staging.yml remove_nifi.yml http_server.yml multi_tls_node.yml`

