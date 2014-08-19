# Readme

## Description

Use this role to install Jenkins and install/update plugins for CentOS 6.x/7.x

## Provides

1. Installation of latest version of Jenkins server
2. Jenkins plugins support

## Requires

1. Ansible 1.6 or higher
2. Centos 6.x and highter 


## Usage

### Get the code

```bash
$ git clone git@github.com:Ochir/jenkins_centos.git
```


### Create a hosts file

Following example make ansible aware of the Virtual box reachable on localhost port 3022.

```bash
$ vim hosts
```

with

```ini
[jenkins]
127.0.0.1 ansible_ssh_port=3022
```

### Modify specific variables

Available variables are listed below, along with default values (see `vars/main.yml`):

    hostname: localhost

The system hostname; usually `localhost` works fine. This will be used during setup to communicate with the running Jenkins instance via HTTP requests.

    jenkins_jar_location: /opt/jenkins-cli.jar

The location at which the `jenkins-cli.jar` jarfile will be kept. This is used for communicating with Jenkins via the CLI.

    jenkins_plugins:
      - git
      - ldap
      - ssh

Jenkins plugins to be installed automatically during provisioning. You can always install more plugins via the Jenkins UI at a later time, but this is helpful in getting things up and running more quickly.

### Run the playbook

First create a playbook including the jenkins role, naming it test.yml.

```yml
- hosts: jenkins
  sudo: yes
  tasks:
    - include: tasks/main.yml
  handlers:
    - include: handlers/main.yml
```

Use *hosts* as inventory. Run the playbook only for the remote host *jenkins*. *--ask-sudo-pass* enables for the sudo password prompt.

```bash
$ ansible-playbook  -i hosts test.yml --ask-sudo-pass
```

### Example output

```
nsible-playbook -i hosts test.yml --ask-sudo-pass
sudo password: 

PLAY [jenkins] **************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [localhost]

TASK: [Check that Jenkins repo is installed.] ********************************* 
ok: [localhost]

TASK: [Add Jenkins repo GPG key.] ********************************************* 
ok: [localhost]

TASK: [Check if Jenkins is installed.] **************************************** 
changed: [localhost]

TASK: [Ensure Jenkins is started and runs on startup.] ************************ 
changed: [localhost]

TASK: [Wait for Jenkins to start up before proceeding.] *********************** 
ok: [localhost]

TASK: [Download the jenkins-cli jarfile from the Jenkins server.] ************* 
ok: [localhost]

TASK: [Create Jenkins updates folder.] **************************************** 
ok: [localhost]

TASK: [Update Jenkins plugin data.] ******************************************* 
skipping: [localhost]

TASK: [Permissions for default.json updates info.] **************************** 
ok: [localhost]

TASK: [Install Jenkins plugins.] ********************************************** 
skipping: [localhost] => (item=git)
skipping: [localhost] => (item=ssh)

PLAY RECAP ******************************************************************** 
localhost                  : ok=10   changed=2    unreachable=0    failed=0  

```