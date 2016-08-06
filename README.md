# Restore Geonode with Vagrant + Ansible

#### In this project, we developed a tool to automate the installation of Geonode 2.4 for production and its enviroment by using Vagrant and Ansible.

## Prerequisites:
1. Install [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git);
2. Install [Vagrant](https://www.vagrantup.com/downloads.html).

## Quick start:
1. Open a terminal tab at the location where you want to clone the project and run:

  ```
  $ git clone https://github.com/MalawiGeospatialTools/restore_geonode.git
  ```

2. Initialize Vagrant:

  ```
  $ vagrant init
  ```
3. Move Vagrantfile_prj content to default Vagrantfile created with ``` vagrant init ```:

 <pre>
  $ mv Vagrantfile_prj Vagrantfile
 </pre>
4. Open in a text editor ``` ansible_variables.yml ``` and set your entries for Geonode superuser name, email and password:

  ```
  geonode_super_usr: <super_user-name>
  geonode_super_usr_email: <super_user_email>
  geonode_super_usr_pwd: <super_user_pwd>
  ```
You can also set a custom IP address of the remote host. The default is: 

 ``` 
 node_ip: 10.0.15.11
 ```
 If you need to change this address, you must also update the remote host network identifier in the  ``` Vagrantfile ```:
 
  ``` 
  node1_config.vm.network :private_network, ip: "10.0.15.11"
   ``` 
5. Create and configure your Vagrant environment according to the Vagrantfile:

  ```
  $ vagrant up
  ``` 
  This will launch two machines (mgmt and node1), install Ansible on the management node and copy over the project files;
6. Log into the management node by running:

  ```
  $ vagrant ssh mgmt
  ```
7.  You are now logged in as the vagrant user and sitting in its home directory:

   ```
  vagrant@mgmt:~$
  ```
8. Setup SSH trust between the nodes mgmt and node1:

   ```
  vagrant@mgmt:~$ ssh-keygen -t rsa -b 2048
  ```
  if you do not have particular needs, just press ``` <return> ``` when asked to respond to the prompts. Then:
  ```
  vagrant@mgmt:~$ ssh-keyscan node1 >> .ssh/known_hosts
  ```
9. Launch the Ansible script to configure and install Geonode on node1 (the pwd is ```vagrant```):

   ```
  vagrant@mgmt:~$ ansible-playbook geonode-install.yml --ask-pass
  ```
  Beaware that it takes at least 8 minutes to complete the installation.

10. Once the script has successfully completed, you can start using Geonode by opening a browser window and go to:

   ```
  10.0.15.11
  ```

