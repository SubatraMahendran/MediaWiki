# MediaWiki
Mediawiki Deployment


## Technologies Used
This Technologies used in this deployment is
* Yaml Scripting
* Cloud Formation Template
* Ansible Playbooks

### File Details
* deployment.yaml - Cloud Formation template for blue green deployment.
* playbook.yaml   - Ansible playbook to install dependencies , setup db.

### Deployment Process
* To Deploy the Cloud Formation Stack using cli
```
echo "Creating the stack"
aws cloudformation deploy --template-file ./deployment.yaml --stack-name bluegreenapp1 --parameter KeyName=test
Waiting for changeset to be created..
Waiting for stack create/update to complete
Successfully created/updated stack - bluegreenapp1

* To install mediaWiki using ansible.
```
echo "Run Ansible Playbook to deploy mediaWiki"
ansible-playbook playbook.yaml -i invertory.txt
```
