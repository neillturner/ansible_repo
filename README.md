# ansible_repo
ansible_repo for testing ansible using test kitchen and verfier-serverspec. 

There are three ways to run verifier serverspec:
  * Remotely directly on the server running serverspec in exec mode
  * Remotely directly on the server running serverspec in ssh mode
  * Locally on your workstation running serverspec in ssh mode

## Remotely directly on server running serverspec in exec mode

This allow testing directly on the server. Typicaly used in conjunction with ansible using local connection.

See .kitchen.yml 

## Remotely directly on the server running serverspec in ssh mode

This allow testing of multiple remote servers. Typicaly used in conjunction with ansible using ssh connection.

See .kitchen.yml.ssh_ssh and .kitchen.yml.ssh_vagrant

This demonstrates using test-kitchen, ansible and ansiblespec to build and verify a tomcat server.
  * Everything is done via ssh from the Ansible/Serverspec server so nothing is installed on the tomcat server apart from Java and Tomcat.
  * In this demonstration both servers are centos 7 running under virtual box on your workstation, however the Tomcat server
could be anywhere like Amazon EC2, or a Docker Container.
  * You can take an image of the server after it is build and no comfiguration software is install on the Tomcat Server.
  * this is using ansible in ssh connection mode to do remote configuration.

Alternatively tomcat can be installed and configured on the same server as ansible. This is using the ansible local connection mode.


![test-kitchen, ansible and serverspec](https://github.com/neillturner/ansible_repo/blob/master/kitchen-ansible.png "test-kitchen, ansible and serverspec")

## Locally on your workstation running serverspec in ssh mode

This allows you not to have to install ruby and serverspec on the server being configured as serverspec is run on your workstation in ssh mode.

see .kitchen.yml.local_ssh


## Workstation Software Installation

The first thing you need to do is install the test-kitchen environment on your workstation.
A useful link is: http://misheska.com/blog/2013/12/26/set-up-a-sane-ruby-cookbook-authoring-environment-for-chef/

The follow instructions are for Windows PC (it will be similar for Mac):

1. Download and install virtualbox from https://www.virtualbox.org/wiki/Downloads.
2. Download and install Vagrant from https://www.vagrantup.com/downloads.html
3. Download and install the Windows RubyInstaller for 64 bit Ruby 2.1 from http://rubyinstaller.org/downloads.
   * Check the option to add ruby to your path.
4. Download and install the Windows Ruby DevKit for use with Ruby 2.0 and above (64bits version only) from http://rubyinstaller.org/downloads.
5. Configure the Ruby DevKit
   * In the devkit directory run “ruby dk.rb init”.
   * Check the config.yml generated has added the the path of the ruby install, if not add it manaully.
   * run “ruby dk.rb install” to bind it to the ruby installation.
6. Then install the following gems
  * gem install librarian-ansible
  * gem install test-kitchen
  * gem install kitchen-ansible
  * gem install kitchen-vagrant
  * gem install kitchen-verifier-serverspec
7. git clone the repository https://github.com/neillturner/ansible_repo and in a command window in the ansible_repo directory run command
```
kitchen list
```
This will return a list if everything is correctly installed.

There are 2 ways to run ansible either locally or remotely. In the local option you just need one server and ansible and the software you are configuring are all installed on the one server.
In the remote option you need at least 2 servers. One server will get ansible installed on it and it will then use ssh to configure the second server remotely.

Use the following kitchen yml files for each option:
  * .kitchen.yml.local_ssh - locally against any cloud where server accessible via ip address.
  * .kitchen.yml.ssh_ssh - remotely against any cloud where server accessible via ip address.
  * .kitchen.yml.local_vagrant - locally using servers via vagrant.
  * .kitchen.yml.ssh_vagrant - remotely using servers via vagrant.

When using rename spec/spec_helper_local.rb to spec/spec_helper.rb and a separate tomcat servers is not required.

## Using AWS or any cloud where servers can be access via IP addresses.

1. Create 2 linux servers one for ansible and one for tomcat using a keypair using say AWS Cloud Formation.
2. In ansible_windows_repo update the inventory/hosts_ssh with IP address of tomcat server.
3. In the .kitchen.yml file
   * Set the ssh_key  to the aws keypair for linux server e.g. spec/test.pem
   * Set the hostname to ip address of linux server  e.g.'54.229.103.38'
4. create, converge and verify the ansible-centos-70 server
```
kitchen create ansible-centos-70 -l debug
kitchen converge ansible-centos-70 -l debug
kitchen verify ansible-centos-70 -l debug
```

## Create Servers in Vagrant on your Workstation.

1. Review the .kitchen.yml file, specifying IP address that are part of your workstation private address space or
use DHCP to let the network dynamically allocte IP addresses.

2. To bring servers up using DHCP on your workstation run
```
kitchen create ansible-centos-70 -l debug
kitchen create tomcat-centos-70 -l debug
```
2. So ansible can access the server get the “private_key” file of the tomcat servers from directory
  ansible_repo/.kitchen/kitchen-vagrant/kitchen-ansible_repo-tomcat-centos-70/.vagrant/machines/default/virtualbox/private_key
and copy to
  ansible_repo/spec/tomcat_private_key.pem
3. Update the hosts file with the  IP address of the tomcat server.

## Build the tomcat server.
```
kitchen converge ansible-centos-70 -l debug
```

## Verify the tomcat server.
```
kitchen verify ansible-centos-70 -l debug
```

##




