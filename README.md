#Intro

This suite of playbooks should launch the c3.large EC2 and db.t2.small RDS instance, download and install Atlassian Stash, create a DNS record in Route53 and set up Postfix to work with SES.

#Prerequisites

The AWS-related playbooks require you to have the ec2-cli installed with working credentials.

A Key Pair has been set up in your AWS account.

A valid Atlassian Stash license (works with the evaluation key as well).

This repo was built while running these versions:

ec2-cli version

```
[dvaida@scat Atlassian-Stash-Ansible]$ which ec2-create-tags
~/Downloads/ec2-api-tools-1.7.2.2/bin/ec2-create-tags
```

ansible version

```
[dvaida@scat Atlassian-Stash-Ansible]$ ansible --version
ansible 1.9 (devel 3b80f63e22) last updated 2014/12/05 14:16:26 (GMT +200)
  lib/ansible/modules/core: (detached HEAD b766390ae2) last updated 2014/12/05 14:16:32 (GMT +200)
  lib/ansible/modules/extras: (detached HEAD 19e688b017) last updated 2014/12/05 14:16:37 (GMT +200)
  v2/ansible/modules/core: (detached HEAD cb69744bce) last updated 2014/12/05 14:16:41 (GMT +200)
  v2/ansible/modules/extras: (detached HEAD 8a4f07eecd) last updated 2014/12/05 14:16:46 (GMT +200)
  configured module search path = None
[dvaida@scat Atlassian-Stash-Ansible]$ 
```

python and boto version

```
[dvaida@scat Atlassian-Stash-Ansible]$ python
Python 2.7.5 (default, Nov  3 2014, 14:33:39) 
[GCC 4.8.3 20140911 (Red Hat 4.8.3-7)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import boto
>>> boto.Version
'2.34.0'
>>> 
```

Stash version: 3.5.0

#How-To

There are two files handling sensitive info. I suggest you encrypt them with ansible-vault. Be sure to edit them and put in your own values:
group_vars/local

```
db_master_password:
```

and

group_vars/all

```
smtp_password:
smtp_username:
```

And of course, edit the values in host_vars/localhost to match your own.

Finally, the provisioning can be done by running:

```
ansible-playbook vpc.yml --ask-vault-pass
```

Subsequent runs of the stash.yml playbook (i.e. in case you already have the Server ready) can be achieved by running:

```
ansible-playbook stash.yml --ask-vault-pass
```

#Steps after the playbooks completes successfully / while playbook is running

1. Once you set up AWS SES manually in your AWS account, don't forget to configure the e-mail settings in Stash. Only thing you need to do is to set the "Hostname" to "localhost" (without quotes). "E-mail from" can be something relevant (mind the subdomain). Please leave ALL other fields blank. Finally, you can test your settings.


#To Dos
Make the Postfix role work on Debian-based distros.

Provide some helpful links in this Readme.

Better naming of variables and cleanup of plays.
