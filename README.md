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
```

* To install mediaWiki using ansible.
```
echo "Run Ansible Playbook to deploy mediaWiki"
ansible-playbook playbook.yaml -i invertory.txt
PLAY [Setting Up MediaWiki] ****************************************************

TASK [Gathering Facts] *********************************************************
ok: [VM2]
ok: [VM1]

TASK [update the system] *******************************************************
ok: [VM2]
ok: [VM1]

TASK [Install Required Packages] ***********************************************

[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via 
squash_actions is deprecated. Instead of using a loop to supply multiple items 
and specifying `name: "{{ item }}"`, please use `name: ['php73', 
'php73-mysqlnd', 'php73-gd', 'php73-xml', 'mariadb-server', 'mariadb', 
'php73-mbstring', 'php73-json', 'wget', 'firewalld', 'php73-intl']` and remove 
the loop. This feature will be removed in version 2.11. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via 
squash_actions is deprecated. Instead of using a loop to supply multiple items 
and specifying `name: "{{ item }}"`, please use `name: ['php73', 
'php73-mysqlnd', 'php73-gd', 'php73-xml', 'mariadb-server', 'mariadb', 
'php73-mbstring', 'php73-json', 'wget', 'firewalld', 'php73-intl']` and remove 
the loop. This feature will be removed in version 2.11. Deprecation warnings 
can be disabled by setting deprecation_warnings=False in ansible.cfg.

changed: [VM2] => (item=[u'php', u'php-mysqlnd', u'php-gd', u'php-xml', u'mariadb-server', u'mariadb', u'php-mbstring', u'php-json', u'wget', u'firewalld', u'php-intl'])
changed: [VM1] => (item=[u'php', u'php-mysqlnd', u'php-gd', u'php-xml', u'mariadb-server', u'mariadb', u'php-mbstring', u'php-json', u'wget', u'firewalld', u'php-intl'])

TASK [start httpd service] *****************************************************
ok: [VM2]
ok: [VM1]

TASK [enable httpd service] ****************************************************
ok: [VM2]
ok: [VM1]

TASK [start mariadb service] ***************************************************
changed: [VM2]
changed: [VM1]

TASK [Enable firewall] *********************************************************
ok: [VM2]
ok: [VM1]

TASK [Start firewall] **********************************************************
changed: [VM2]
changed: [VM1]

TASK [Download Mediawiki] ******************************************************
changed: [VM2]
changed: [VM1]

TASK [Extract the Mediawiki file] **********************************************
changed: [VM1]
changed: [VM2]

TASK [Move the mediawiki file] *************************************************
changed: [VM2]
changed: [VM1]

TASK [Change the ownership of folder] ******************************************
[WARNING]: Consider using the file module with owner rather than running
'chown'.  If you need to use command because file is insufficient you can add
'warn: false' to this command task or set 'command_warnings=False' in
ansible.cfg to get rid of this message.
changed: [VM1]
changed: [VM2]

TASK [Update the permissions of folder] ****************************************
[WARNING]: Consider using the file module with mode rather than running
'chmod'.  If you need to use command because file is insufficient you can add
'warn: false' to this command task or set 'command_warnings=False' in
ansible.cfg to get rid of this message.
changed: [VM2]
changed: [VM1]

TASK [Add port 80 firewall-cmd] ************************************************
changed: [VM2]
changed: [VM1]

TASK [reload service firewalld] ************************************************
changed: [VM2]
changed: [VM1]

TASK [update the iptables] *****************************************************
changed: [VM2]
fatal: [VM1]: FAILED! => {"changed": true, "cmd": "iptables-save | grep 80", "delta": "0:00:00.010434", "end": "2021-11-15 03:30:53.817398", "msg": "non-zero return code", "rc": 1, "start": "2021-11-15 03:30:53.806964", "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
...ignoring

TASK [run getenforce command] **************************************************
changed: [VM2]
changed: [VM1]

TASK [restore configuration] ***************************************************
changed: [VM1]
changed: [VM2]

TASK [mysql_db] ****************************************************************
changed: [VM1]
changed: [VM2]

TASK [mysql_user] ****************************************************************
changed: [VM1]
changed: [VM2]

PLAY RECAP *********************************************************************
VM1                        : ok=20   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=1   
VM2                        : ok=20   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


```
* To setup mediaWiki via Browser.

Load Balancer URL : http://wikiapp-internal-elb-755010802.ap-south-1.elb.amazonaws.com/mediawiki/
Set up the DB and upload the LocalSettings.php file to /var/www/html/mediawiki/ folder 

Uploading LocalSetting.php :  scp -i  keyname.pem ./LocalSettings.php  ec2-user@InstanceIPaddress:/home/ec2-user/
                              mv /home/ec2-user/LocalSettings.php /var/www/html/mediawiki/
                              
MediaWiki Page : http://wikiapp-internal-elb-755010802.ap-south-1.elb.amazonaws.com/mediawiki/index.php?title=Main_Page

Please refer the screenshot for the browser images .

```

