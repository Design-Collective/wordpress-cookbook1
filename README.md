Description
===========

Installs and configures Wordpress according to the instructions at http://www.designcollective.io/blogs/manage-wordpress-with-git. Does not set up a wordpress blog. You will need to do this manually by going to http://hostname/wp-admin/install.php (this URL may be different if you change the attribute values).

Essentially, it configures a VM and also extracts the wp-content directory from the wordpress dir inorder to manage it as a submodule.

You can use https://github.com/Design-Collective/wordpress-chef-boilerplate to get started with a project using this cookbook. The project includes a Berkshelf File,  Thorfile, Vagrant and Knife.rb.

For more information on how to get started, visit:
http://www.designcollective.io/blogs/preconfigured-wordpress-vm-via-chef-vagrant-berkshelf


Requirements
============

Platform
--------

* Debian, Ubuntu

Tested on:

* Ubuntu 9.04, 9.10, 10.04

Cookbooks
---------

* mysql
* php
* apache2
* opensssl (uses library to generate secure passwords)

Attributes
==========

* `node['wordpress']['version']` - Set the version to download. Using 'latest' (the default) will install the most current version.
* `node['wordpress']['checksum']` - sha256sum of the tarball, make sure this matches for the version! (Not used for 'latest' version.)
* `node['wordpress']['dir']` - Set the location to place wordpress files. Default is /var/www.
* `node['wordpress']['db']['database']` - Wordpress will use this MySQL database to store its data.
* `node['wordpress']['db']['user']` - Wordpress will connect to MySQL using this user.
* `node['wordpress']['db']['password']` - Password for the Wordpress MySQL user. The default is a randomly generated string.
* `node['wordpress']['server_aliases']` - Array of ServerAliases used in apache vhost. Default is `node['fqdn']`.

Attributes will probably never need to change (these all default to randomly generated strings):

* `node['wordpress']['keys']['auth']`
* `node['wordpress']['keys']['secure_auth']`
* `node['wordpress']['keys']['logged_in']`
* `node['wordpress']['keys']['nonce']`

The random generation is handled with the secure_password method in the openssl cookbook which is a cryptographically secure random generator and not predictable like the random method in the ruby standard library.

Usage
=====

If a different version than the default is desired, download that version and get the SHA256 checksum (sha256sum on Linux systems), and set the version and checksum attributes.

Add the "wordpress" recipe to your node's run list or role, or include the recipe in another cookbook.

Chef will install and configure mysql, php, and apache2 according to the instructions at http://codex.wordpress.org/Installing_WordPress. Does not set up a wordpress blog. You will need to do this manually by going to http://hostname/wp-admin/install.php (this URL may be different if you change the attribute values).

The mysql::server recipe needs to come first, and contain an execute resource to install mysql privileges from the grants.sql template in this cookbook.

## Note about MySQL

This cookbook will decouple the mysql::server and be smart about detecting whether to use a local database or find a database server in the environment in a later version.

