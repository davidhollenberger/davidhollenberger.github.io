---
layout: post
title: Ansible Tower EC2 Inventory
tags:
  - ansible
  - aws
---


Setting Up EC2 Inventory
--------------------------

* Create new Inventory
* Inside inventory, create new group
  * Set source to Amazon EC2
  * Set cloud credential, and other filters if needed
  * Under Update Options, click Update on launch
  * In source variables set the following

```yaml
vpc_destination_variable: private_ip_address
```

* Click save and start sync process

Import group_vars from git project
----------------------------------
* Drill down through Amazon EC2 group to the group that you want to import static variables from your project
* Click the "Copy or move group" button.
* Select "Move", then check "Use the inventory root" option.  Click OK.
* Add a group name heading to the hosts file for your Ansible project
* SSH to Tower host and run tower-manage to sync variables from your inventory
* Before running tower-manage, delete ec2.py and ec2.ini from the project inventory. This conflicts with tower's local ec2.py for some reason.

```bash
tower-manage inventory_import --source=/var/lib/awx/projects/<path_to_inventory> --inventory-name="<Tower_Inventory_Name>"
```

Oops
----
Next time this inventory is updated it moves the group that was previously moved to the inventory root back under the EC2 group.  Any variables that have been imported to the group will remain there, but if there are any changes to the variables in your project you will need to repeat the steps above to import them.

Working with Ansible Tower support on this problem.

Reference
---------
[Ansible Tower Docs](http://docs.ansible.com/ansible-tower/)

[Private EC2 VPC Instances in Tower Inventory](https://support.ansible.com/hc/en-us/articles/201958007-Private-EC2-VPC-Instances-in-Tower-Inventory)

[Importing existing inventory files and host/group vars into Tower](https://support.ansible.com/hc/en-us/articles/201958047-Importing-existing-inventory-files-and-host-group-vars-into-Tower)
