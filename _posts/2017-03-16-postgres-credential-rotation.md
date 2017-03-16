---
layout: post
title: Postgres Credential Rotation
comments: true
---

We recently needed a way to rotate the database credentials for an application without incurring any downtime.  Up until this point we had a single user account that the application used to connect.  This made it impossible to change passwords without incurring an outage.  We are now moving to a method of using 3 roles.  One role is assigned all the database privileges needed for the application.  The other two roles inherit the first role's privileges.  We allow only one of the roles access to login to the database at a time (except for when we're rotating credentials).  Here's an example of how we setup the roles.

```
create role foo nologin;
create role foo_a with encrypted password 'md5...' login;
create role foo_b with encrypted password 'md5...' nologin;
grant foo to foo_a;
grant foo to foo_b;
```

This gets us most of the way to a seamless credential rotation.  The issue I ran into next was any new objects created by foo_a or foo_b users were owned by foo_a and foo_b respectively.  To ensure all objects created by foo_a and foo_b are owned by the group user foo we need to override their default role.  

```
alter role foo_a set role foo;
alter role foo_b set role foo;
```

Whenever foo_a and foo_b login, their default role is set to foo.  All objects they create will then be owned by foo, so we will not have to go back and modify permissions or change ownership when rotating credentials.


## Credential rotation

To rotate credentials for an application we would perform these steps.

Allow logins from both roles

```
alter role foo_b login;
```

Distribute new username and password and redeploy the application.  

Confirm all connections have cleared from the old user and the application is able to connect as the new user.

```
select datname, usename, count(usename)
from pg_stat_activity
group by datname, usename
order by datname;
```

Disable the old account (optionally change the password at this time).

```
alter role foo_a with encrypted password 'md5...' nologin;
```

## References

Huge thanks to Aaron Brashears' great write-up on [How Twitch uses PostgreSQL](https://blog.twitch.tv/how-twitch-uses-postgresql-c34aa9e56f58#.1yfjcma6l).  The final key component came from an old post by Magnus Hagander, [Faking the dbo role](https://www.hagander.net/blog/faking-the-dbo-role-70/).  Thanks so much for all the work you do and helping to improve this community!
