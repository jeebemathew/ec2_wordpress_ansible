# ec2_wordpress_ansible

This ansible playbook is written to launch an ec2 instance from AWS with WordPress installed on it.

## Requirements 

- AWS IAM account with  programmatic access
- boto

## Please save the following files as provided and run the ansible playbook

### virtualhost.j2

```
<virtualhost *:80>
  servername {{ domain }}
  documentroot /var/www/html/{{domain}}
  directoryindex index.php index.html info.html info.php
</virtualhost>

```

### wp-config.php.tmpl

```
<?php

define( 'DB_NAME', '{{ mysql_database }}' );

define( 'DB_USER', '{{ mysql_user }}' );

define( 'DB_PASSWORD', '{{ mysql_password }}' );

define( 'DB_HOST', 'localhost' );

define( 'DB_CHARSET', 'utf8' );

define( 'DB_COLLATE', '' );


define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );


$table_prefix = 'wp_';

define( 'WP_DEBUG', false );

if ( ! defined( 'ABSPATH' ) ) {
	define( 'ABSPATH', dirname( __FILE__ ) . '/' );
}

require_once( ABSPATH . 'wp-settings.php' );

```

Run the ansible playbook as follows

```

[root@ip-172-31-35-128 ansible]# ansible-playbook -ec2-wordpress.yml

PLAY [EC2_wordpress] ***************************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************************
[WARNING]: Platform linux on host localhost is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could
change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.
ok: [localhost]

TASK [Launch_new_EC2] **************************************************************************************************************************************************
changed: [localhost]

TASK [debug] ***********************************************************************************************************************************************************
TASK [adding host] *****************************************************************************************************************************************************
changed: [localhost]

TASK [ec2 set time] ****************************************************************************************************************************************************
ok: [localhost]

TASK [Setup LAMP] ******************************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Apache - creating index.html] ************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Apache - Restarting/Enabling] ************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Mariadb-Server - Installation] ***********************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Mariadb-server Restarting/Enabling] ******************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [creating virtualhost] ********************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Maria-db server root password reset] *****************************************************************************************************************************
[WARNING]: Module did not set no_log for update_password
changed: [localhost -> 13.58.164.76]

TASK [Removing Anonymous users] ****************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Creatinig database] **********************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Mariadb- User add] ***********************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Creating document root] ******************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Wordpress downloading] *******************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Archiving files] *************************************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Copying wordpress contents to document root] *********************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Wordpress- Creating wp-config.php] *******************************************************************************************************************************
changed: [localhost -> 13.58.164.76]

TASK [Wordpress - file ownership] **************************************************************************************************************************************
[WARNING]: Consider using the file module with owner rather than running 'chown'.  If you need to use command because file is insufficient you can add 'warn: false' to
this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.
changed: [localhost -> 13.58.164.76]

TASK [Deleting files from /tmp folder] *********************************************************************************************************************************
changed: [localhost -> 13.58.164.76] => (item=/tmp/wordpress.tar.gz)
changed: [localhost -> 13.58.164.76] => (item=/tmp/wordpress)

TASK [Lampstack - restarting services] *********************************************************************************************************************************
changed: [localhost -> 13.58.164.76] => (item=httpd)
changed: [localhost -> 13.58.164.76] => (item=mariadb)

PLAY RECAP *************************************************************************************************************************************************************
localhost                  : ok=23   changed=20   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

