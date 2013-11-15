Environment Information
========================

## IP

    Vagrant box:   33.33.33.10
    Remote XDebug: 33.33.33.1

## Database

    Name: vagrant
    User: root
    Pass: root


Installed Components
========================

 - Apache2
 - Compass
 - Composer
 - Less
 - Git
 - Memcached
 - MySQL
 - NodeJS
 - PHP
 - Ruby
 - SASS
 - SQLite
 - XDebug


Installation
========================

Ensure you have the following tools installed on your computer:

 - Vagrant (http://vagrantup.com)
 - VirtualBox (http://www.virtualbox.org)
 - PHP (http://www.php.net)
 - GIT (http://git-scm.com)
 - Composer (http://getcomposer.org)

 1. Clone this repo with the cookbooks submodule:

        git clone --recursive git://github.com/simshaun/symfony-vagrant.git

 2. Edit **vagrant/Vagrantfile** to suit your needs.

 3. Install Symfony using the installation instructions on http://symfony.com/download

 4. `cd` to the **vagrant** folder and run `vagrant up`. This may take a few minutes.

 5. Try to access the site. Unless you changed the server name and port in the Vagrantfile,
    the URL should be http://localhost:8080/. Edit your hosts file if you want a different URL.

        33.33.33.10  myproject


Post-Install
========================

Symfony limits access to **app_dev.php** and **config.php**.

To use either of these files:

 1. At the top of each file, add the *Vagrant box IP* (default: 33.33.33.10) to the
    array of allowed REMOTE_ADDR.


Multiple Projects in one Vagrant box
===============================================

Typically you have one Vagrant box per project, but if you want to conserve disk space
you can host multiple projects from one box.

 1. Add additional projects to the `web_apps` variable at the top of vagrant/Vagrantfile.

    Here is an example configuration for 2 projects:

        web_apps = {
          "site1" => {
            "host_project_folder"  => "../app1/",
            "guest_project_folder" => "/home/vagrant/site1",
            "guest_docroot"        => "/home/vagrant/site1/web",
            "server_name"          => "site1",
            "server_aliases"       => ["*.site1"],
            "php_timezone"         => "America/New_York"
          },
          "site2" => {
            "host_project_folder"  => "../app2/",
            "guest_project_folder" => "/home/vagrant/site2",
            "guest_docroot"        => "/home/vagrant/site2/web",
            "server_name"          => "site2",
            "server_aliases"       => ["*.site2"],
            "php_timezone"         => "America/New_York"
          }
        }

 2. On the host machine, add a new line to your `hosts` file with each projects' `server_name` value

    For example:

        33.33.33.10  site1
        33.33.33.10  site2

    `33.33.33.10` is the IP specified in the Vagrantfile `guest_ip` variable.


Troubleshooting
=============================

 1. **Vagrant is complaining about the box's VBoxGuestAdditions not matching the host version.**

    The VBoxGuestAdditions in the guest box are probably older than the ones you have on your host machine.

    There is a gem you can install called [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) that upgrades the guest additions on the guest box automatically.
