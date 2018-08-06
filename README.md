# vagrant-ckan
This project is Vagrant exercise of deploying Docker containers with Jenkins and CKAN, both provisioned with Ansible.
CKAN is an open-source DMS (data management system) for powering data hubs and data portals, http://ckan.org .

Local private network 172.x is created and used for communications with VMs and apps via Vagrant port forwarding. Jenkins VM is provisioned
by _common_ and _jenkins_ roles from [playbook.yml](playbook.yml), ckan VM is provisioned by _common_ and _ckan_ roles, see [Vagrantfile](Vagrantfile) .

## Prerequisites

- Vagrant 2.1.1 or higher

## Installation

```bash
git clone git@github.com:maximfox/vagrant-ckan.git
cd vagrant-ckan
vagrant up
```

Please wait up to 25 mins depending of download speed and your hardware resources. Major time consumer is CKAN build.

## Usage

Jenkins is available at http://172.17.177.22:8080

Username is admin. To obtain administrator password run the following code (or get this from Ansible run otput. Yes, this is against security practices but...):

```bash
vagrant ssh jenkins -c "sudo docker exec -i jenkins sh -c 'cat /var/jenkins_home/secrets/initialAdminPassword'"
```

CKAN is available at http://172.17.177.21:5000

Use ckan-admin for both username and password. The user is added during the deployment and the creds is defined in
clear text inside _group_vars_. Yes, I am aware about Vault or [git-crypt](https://github.com/AGWA/git-crypt) but this is another excercise...

If you would like to add another CKAN user (or if ckan-admin was failed during the deployment) use the following code:

```bash
vagrant ssh ckan -c "sudo docker exec -uroot -i ckan sh -c 'yes | /usr/local/bin/ckan-paster --plugin=ckan sysadmin -c /etc/ckan/production.ini add ckan-admin email=ckan@example.org password=ckan-admin'"
```

## TODO

- Jenkins Ansible plugin is installed (DONE)
- ckan role should be wrapped into Jenkins DSL job and run from Jenkins by Ansible plugin. Any volunteers for PR?
