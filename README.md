# ansible_repo
Ansible Repository for testing ansible using test kitchen and verfier-serverspec via ssh.
It is assumed that the servers is already running (in AWS or somewhere) and accessible via ssh.

## Verifier Serverspec run options
verifier-serverspec installs software on the server and runs serverspec on the server.
  * Remotely directly on the server running serverspec in exec mode
  * Locally on your workstation running serverspec in ssh mode
  * Remotely directly on the server running serverspec in ssh mode
It can also run on your workstation and test the server remotely, to do this set
   remote_exec: false
in the kitchen.yml file.

![test-kitchen, ansible and serverspec](https://github.com/neillturner/ansible_repo/blob/master/kitchen-ansible.png "test-kitchen, ansible and serverspec")
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
  * gem install test-kitchen
  * gem install kitchen-ansible
  * gem install kitchen-vagrant
  * gem install kitchen-verifier-serverspec
7. git clone the repository https://github.com/neillturner/ansible_vagrant_repo
```
kitchen list
```
This will return a list if everything is correctly installed.

## Access the servers via ssh.
```
kitchen create ansible-centos-70 -l debug
```

## Build the server.
```
kitchen converge ansible-centos-70 -l debug
```

## Verify the server.
```
kitchen verify ansible-centos-70 -l debug
```

## Destroy the server.
```
kitchen destroy ansible-centos-70 -l debug
```
##




