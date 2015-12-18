# ansible_repo
ansible_repo for testing ansible using test kitchen and ansiblespec verifying

```
                                                                     TOMCAT SERVERS
     TEST KITCHEN              ANSIBLE AND SERVERSPEC
     WORKSTATION               SERVER                             +------------------------+
                             +-----------------------+            |   +---------+          |
                             |                       |            |   |Tomcat   |          |
+-------------------+        |                   +---------------->   |         |          |
|                   |        |                   |   |            |   +---------+          |
|    Workstation    |        |                   |   |    +------->                        |
|    test-kitchen   |        |                   |   |    |       |                        |
|    kitchen-ansible|        |                   |   |    |       |                        |
|                   |  create|                   |   |    |       +------------------------+
|     CREATE +--------------->      install      |   |    |
|                   |  server|      and run      |   |    |
|     CONVERGE+-------------------->ANSIBLE  +---+   |    |       +------------------------+
|                   |        |               +-------------------->  +----------+          |
|                   |        | install and run       |    |       |  |Tomcat    |          |
|    VERIFY+------------------>Busser-ansiblespec +-------+       |  |          |          |
+-------------------+        |  +                 |  |            |  +----------+          |
                             |  +--->ServerSpec   +--------------->                        |
                             |                       |            |                        |
                             +-----------------------+            |                        |
                                                                  +------------------------+


                   * All connections over SSH

```

## Standalone Tomcat Deployment

- Requires Ansible 1.2 or newer
- Expects CentOS/RHEL 6.x hosts

These playbooks deploy a very basic implementation of Tomcat Application Server,
version 7.

## Usage

  * Create one of more servers in a cloud like AWS. THey could even be docker containers.

  * Edit the "hosts" inventory file to contain the hostnames of these servers.

  * If using an SSH Key add to repository and config in
    test/integration/tomcat/ansiblespec/config.yml

  * Config .kitchen.yml

  * kitchen create tomcat-centos-6

  * kitchen converge tomcat-centos-6

  * kitchen verify tomcat-centos-6

##




