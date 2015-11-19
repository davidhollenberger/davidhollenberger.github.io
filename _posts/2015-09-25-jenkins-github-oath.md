---
layout: post
title: Configure Github Oauth on Jenkins
comments: True
---

We are configuring a Jenkins instance on AWS and would like to use Github Oauth for user authentication and privileges.

A high level overview of our Jenkins setup.  The Jenkins EC2 instance is in a private VPC subnet with no public IP address.  We have a public ELB that forwards traffic to port 8080 of the Jenkins instance.  Lets go through the steps to configure Github Oath on a new Jenkins instance.


Register Github Application
---------------------------

* Go to your Github Settings page, then Applications.  
* Click "Register new application".  
* Fill out registration form.  Make sure to enter http://*jenkins.dns.address*/securityRealm/finishLogin for Authorization Calback URL.

Jenkins configuration
---------------------
**Install github-oauth plugin**

See the [github-oauth documentation page](https://wiki.jenkins-ci.org/display/JENKINS/Github+OAuth+Plugin#GithubOAuthPlugin-Setup).

**Configure Global Security**

* Select *Github Authentication Plugin*
  * Enter Client ID and Client Secret from the Github Application registration page.  
  * Keep the other fields default.

**Authorization**

* select *Github Commiter Authorization Strategy*.

**Github Authorization Settings**

* *Admin User Names* enter Github username for any administrators
* *Participant in Organization*  Enter the GitHub Organization name.  Any user who is registered with the organization will have job view and build
* Check *Use Github repository permissions*
* Check *Grant READ permissions for /github-webhook*
