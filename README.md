# Vagrant Craft Application
Basic development server for [Craft][craft] applications. Uses the standard Vagrant Ubuntu Precise base box (`precise64.box`).

[craft]:http://buildwithcraft.com/

## Requirements
As you might expect, in order for this to work you need:

1. [Vagrant][vagrant]
2. A Craft project

[vagrant]: http://vagrantup.com/

## Configuration
You may customise the server for your application using the clearly marked "configurable properties" in the following files:

- `/Vagrantfile`
- `/puppet/modules/app/manifests/init.pp`

## Getting started
1. `git clone --recursive https://github.com/Globegitter/vagrant-craft-application.git`
2. Copy the files to the root of your Craft application
3. `rm -r .git` if necessary
4. Run `vagrant up`
5. Wait a while

### What it does
Creates a virtual machine running a basic LAMP stack, configured to work with your Craft application.

In addition to the standard LAMP installation, we:

- Create a database and user, as per the settings in `/puppet/modules/app/manifests/init.pp`;
- Install Composer globally;
- Install PHPUnit;
- Install the PHP extensions [required by Craft][craft_requirements]
- Set the [correct ownership and permissions][craft_installing] on the `craft/app`, `craft/config`, and `craft/storage` directories.

[craft_installing]: http://buildwithcraft.com/docs/installing
[craft_requirements]: http://buildwithcraft.com/docs/requirements

### What it doesn't do
Modify your `/etc/hosts` file. Assuming you left the I.P. address setting in the `Vagrantfile` as `192.168.11.11`, you'll need to add your chosen development domain, as follows:

~~~~~
# /etc/hosts
192.168.11.11  my-project.dev
~~~~~

## Usage
The following examples assume that your configurable properties are as follows:

- _Hostname_: `my-project.dev`
- _Database Name_: `devdb`
- _Database User_: `devdba`
- _Database Password_: `devdba_password`

### Accessing the site via a browser
Assuming you've added the appropriate entry to your `/etc/hosts` file, you can access your site at `http://my-project.dev/`.

### Accessing the database using Sequel Pro
You can connect to the database using Sequel Pro (or an equivalent piece of software) using the following settings:

- _Connection Type_: `SSH`
- _MySQL Host_: `127.0.0.1`
- _Username_: `devdba`
- _Password_: `devdba_password`
- _Database_: `devdb`
- _Port_: `3306`
- _SSH Host_: `localhost`
- _SSH User_: `vagrant`
- _SSH Key_: `~/.vagrant.d/insecure_private_key`
- _SSH Port_: `2222`

### Accessing the server via SSH
`cd` to the directory containing your `Vagrantfile` (the project root, unless you've done something unexpected), and run `vagrant ssh`.
