---
layout: post
title: Scaling Ansible Roles
comments: true
---

I've been using Ansible for about a year now.  It started off relatively simple to manage, but as more people have started contributing to my company's Ansible repo we have started to step on each others toes.   Seemingly simple changes to a role would cause previously created playbooks to fail.  Testing these changes was difficult and time consuming.  We needed a better way to manage Ansible roles especially as we continue to increase our reliance on Ansible.

[Ansible Galaxy](http://docs.ansible.com/ansible/galaxy.html) solves many of these problems.  The `ansible-galaxy` command line helps manage roles listed at the Ansible Galaxy website as well as other SCN sources.  Using Ansible Galaxy to install a role at a specific version allows us to be able to continue developing that role without having to worry about future changes breaking someone else's work.  You can easily change the version of a role that is required by modifying `requirements.yml`.  This is great for infrastructure related playbooks because we are able to know for certain that a playbook we run today will run the same way months from now.

We are in the process of splitting out roles that were contained in a central Github repo to their own Github repos.  We are still using a central repo which contains a shared inventory, ansible-vault files and other variable files. We would like to keep the inventory centralized as there are a lot of variables defined that affect other roles and playbooks.  Encrypted Ansible vault files are also difficult to share since you can't do a diff to find out what's changed.


## External Role Setup

The workflow for developing and testing roles is slightly more complicated with this design, but I feel that the benefits outweigh the extra complexity.  Here are example workflows for using and developing external roles.

1. The directory structure on the development machine looks like this:

    ```
    .
    ├── ansible-development/
        ├── central-ansible-repo
        ├── ansible-role-feature1
        └── ansible-role-feature2
    ```

2. In *main-ansible-repo/feature1-playbooks* create the following files.
    * `ansible.cfg` - override roles_path

        ```
        [defaults]
        roles_path = roles:~/ansible-development
        ```

        Ansible Galaxy created roles will be located in the local `roles` folder and a Github clone of the repo will exist in `~/ansible-development`.  Ansible will first look for the role in `roles/` if it does not exist there it will search `~/ansible-development`.  This allows us to fairly easily switch between the ansible-galaxy installed role and a separate clone of the role for development.
    * `requirements.yml` - list all roles needed for the playbook.

        ```
        # from GitHub, specifying a specific tag and defining role path
         - src: https://github.com/bennojoy/nginx
           version: v1.0
           path: roles/
        ```

3. Create the role using Ansible Galaxy

    ```
    ansible-galaxy install -r requirements.yml
    ```

## External Role Development
1. When making changes to the role, delete the ansible-galaxy installed role and make changes directly to the cloned Github repository in `~/ansible-development`.

2. After changes have been made and tested, commit and push changes to Github.  Create a new release for this change and update `requirements.yml` to reflect the new version.

3. Create the new role using Ansible Galaxy

    ```
    ansible-galaxy install -r requirements.yml
    ```


## Helpful Links

* [Manage Ansible Playbooks and Roles]( http://guifreelife.com/blog/2015/05/16/Manage-Ansible-Playbooks-and-Roles/)
* [Techniques for Versioning Ansible II](http://willthames.github.io/2014/09/03/techniques-for-versioning-ansible-ii.html)
* [Ansible Galaxy Documentation]( http://docs.ansible.com/ansible/galaxy.html#id2)
