# Drupal Development VM

This project is a fork of [Jeff Geerling's](https://github.com/geerlingguy) [Drupal Dev VM](https://github.com/geerlingguy/drupal-dev-vm), which aims to make spinning up a simple local Drupal test/development environment incredibly quick and easy. Feel free to consult the original [project readme](README-prefork.md) for further background and information. The essential information from that readme has been copied here.

It will install the following on an Ubuntu 16.04 (by default) linux VM:

  - Apache 2.4.x (or Nginx 1.x)
  - PHP 7.0.x (configurable)
  - MySQL 5.7.x
  - Drush (configurable)
  - Drupal 7.x, or 8.x.x (configurable)
  - Optional:
    - Drupal Console
    - Varnish 4.x (configurable)
    - Apache Solr 4.10.x (configurable)
    - Node.js 0.12 (configurable)
    - Selenium, for testing your sites via Behat
    - Ruby
    - Memcached
    - Redis
    - SQLite
    - XHProf, for profiling your code
    - Blackfire, for profiling your code
    - XDebug, for debugging your code
    - Adminer, for accessing databases directly
    - Pimp my Log, for easy viewing of log files
    - MailHog, for catching and debugging email

It should take 5-10 minutes to build or rebuild the VM from scratch on a decent broadband connection.

Please read through the rest of this README and the [Drupal VM documentation](http://docs.drupalvm.com/) for help getting Drupal VM configured and integrated with your development workflow.

## Documentation

Full Drupal VM documentation is available at http://docs.drupalvm.com/

## Customizing the VM

See config.yml for customizing your specific configuration needs. The current configuration is based on the needs of the ILR Web Team for the [ILR School's public site](https://github.com/ilrWebServices/ilr-website).

## Quick Start Guide

This Quick Start Guide will help you quickly build a Drupal 8 site on the Drupal VM using the included example Drush make file. You can also use the Drupal VM with a [Local Drupal codebase](http://docs.drupalvm.com/en/latest/deployment/local-codebase/) or even a [Drupal multisite installation](http://docs.drupalvm.com/en/latest/deployment/multisite/).

### 1 - Install dependencies (VirtualBox, Vagrant, Ansible)

  1. If you've obtained the licenses and keys, download and install [VMWare Fusion Pro](http://www.vmware.com/products/fusion/fusion-evaluation) and the [VMWare Vagrant plugin](https://www.vagrantup.com/vmware). Alternatively, download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads), though this is less performant.
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html).
  3. [Mac/Linux only] Install [Ansible](http://docs.ansible.com/intro_installation.html).

### 2 - Build the Virtual Machine

  1. Download this project and put it wherever you want.
  2. Make sure that the appropriate Drupal project is cloned to your local system in the same parent directory as this project and referenced appropriately in config.yml.
  4. Open Terminal, `cd` to this directory (containing the `Vagrantfile` and this README file).
  6. Type in `vagrant up`, and let Vagrant do its magic.

Note: *If there are any errors during the course of running `vagrant up`, and it drops you back to your command prompt, just run `vagrant provision` to continue building the VM from where you left off. If there are still errors after doing this a few times, post an issue to this project's issue queue on GitHub with the error.*

### 3 - Configure your host machine to access the VM.

  1. [Edit your hosts file](http://www.rackspace.com/knowledge_center/article/how-do-i-modify-my-hosts-file), adding the line `192.168.88.89  www.ilr-website.dev` so you can connect to the VM.
    - You can have Vagrant automatically configure your hosts file if you install the `hostsupdater` plugin (`vagrant plugin install vagrant-hostsupdater`). All hosts defined in `apache_vhosts` or `nginx_hosts` will be automatically managed. The `vagrant-hostmanager` plugin is also supported.
    - You can also have Vagrant automatically assign an available IP address to your VM if you install the `auto_network` plugin (`vagrant plugin install vagrant-auto_network`), and set `vagrant_ip` to `0.0.0.0` inside `config.yml`.
  2. Follow the setup instructions in the [developer docs](https://github.com/ilrWebServices/ilr-website/blob/master/docs/installation.md) to complete the installation.

## Connecting to MySQL

By default, this VM is set up so you can manage mysql databases on your own. The default root MySQL user credentials are `root` for username+password, but you could change the password via `config.yml`. I use the MySQL GUI [Sequel Pro](http://www.sequelpro.com/) (Mac-only) to connect and manage databases, then Drush to sync databases (sometimes I'll just do a dump and import, but Drush is usually quicker, and is easier to do over and over again when you need it).

### Connect using Sequel Pro (or a similar client):

  1. Use the SSH connection type.
  2. Set the following options:
    - MySQL Host: `127.0.0.1`
    - Username: `drupal`
    - Password: `drupal` (or whatever password you chose in `config.yml`)
    - SSH Host: `192.168.88.89` (or whatever IP you chose in `config.yml`)
    - SSH User: `vagrant`
    - SSH Key: (browse to your `~/.vagrant.d/` folder and choose `insecure_private_key`)

You should be able to connect as the root user and add, manage, and remove databases and users.

## Extra software/utilities

By default, this VM includes the extras listed in the `config.yml` option `installed_extras`:

    installed_extras:
      - adminer
      # - blackfire
      - drupalconsole
      - mailhog
      # - memcached
      # - newrelic
      # - nodejs
      - pimpmylog
      # - redis
      # - ruby
      # - selenium
      # - solr
      - varnish
      # - xdebug
      # - xhprof

If you don't want or need one or more of these extras, just delete them or comment them from the list. This is helpful if you want to reduce PHP memory usage or otherwise conserve system resources.

## Using Drupal VM

Drupal VM is built to integrate with every developer's workflow. Many guides for using Drupal VM for common development tasks are available on the [Drupal VM documentation site](http://docs.drupalvm.com).

## System Requirements

Drupal VM runs on almost any modern computer that can run VirtualBox and Vagrant, however for the best out-of-the-box experience, it's recommended you have a computer with at least:

  - Intel Core processor with VT-x enabled
  - At least 4 GB RAM (higher is better)
  - An SSD (for greater speed with synced folders)

## Other Notes

  - To shut down the virtual machine, enter `vagrant halt` in the Terminal in the same folder that has the `Vagrantfile`. To destroy it completely (if you want to save a little disk space, or want to rebuild it from scratch with `vagrant up` again), type in `vagrant destroy`.
  - When you rebuild the VM (e.g. `vagrant destroy` and then another `vagrant up`), make sure you clear out the contents of the `drupal` folder on your host machine, or Drupal will return some errors when the VM is rebuilt (it won't reinstall Drupal cleanly).
  - You can change the installed version of Drupal or drush, or any other configuration options, by editing the variables within `config.yml`.
  - Find out more about local development with Vagrant + VirtualBox + Ansible in this presentation: [Local Development Environments - Vagrant, VirtualBox and Ansible](http://www.slideshare.net/geerlingguy/local-development-on-virtual-machines-vagrant-virtualbox-and-ansible).
  - Learn about how Ansible can accelerate your ability to innovate and manage your infrastructure by reading [Ansible for DevOps](http://www.ansiblefordevops.com/).

## License

This project is licensed under the MIT open source license.
