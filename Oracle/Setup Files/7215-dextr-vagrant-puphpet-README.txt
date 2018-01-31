DextR-Vagrant
========================

Virtual Machine optimized to develop DextR project

----------

How to use ?
-------------

[PuPHPet](https://puphpet.com/help) allows to easily build a custom Virtual Machine which can be shared between each developers. To have a better idea of what will be available in this VM, take a look at [puphpet/config.yaml](https://github.com/LinkValue/dextr-vagrant/blob/master/puphpet/config.yaml). If you edit this file to fit your needs (for example to decrease the VM memory usage), please don't commit it (except if you are sure that this is an improvement for everyone).


### Requirements

- Git
- VirtualBox
- Vagrant
- UNIX system (It might work out of the box using *Windows* but you'll probably encounter some issues especially while dealing with "synced folder" and "symlink" stuffs) 


### Installation

This section explains what you'll need to do in order to run a generic symfony2 project on your side (here we will run a project named `dextr`, but you could easily use the same VM for another project if you update [puphpet/config.yaml](https://github.com/LinkValue/dextr-vagrant/blob/master/puphpet/config.yaml) a bit):


1. Clone the Symfony project repository

        git clone https://github.com/LinkValue/dextr.git

2. Clone the VM repository

        git clone https://github.com/LinkValue/dextr-vagrant.git

3. Copy the folder `puphpet` and the file `Vagrantfile` to the root of your Symfony project
        
4. Go to the root of your Symfony's project (where you now have the `VagrantFile`), then start building the Virtual Machine:

        vagrant up

5. If everything worked, you should be able to connect to the guest machine (VM) through SSH:

        vagrant ssh

6. Now that you have a terminal opened on the guest machine (which runs on Ubuntu) go to the project root folder:

        cd /var/www/html/dextr

7. Then download all the project dependencies by running:

        composer install
        npm install
        bower install

  You'll be asked to set some "new" parameters, just type `enter` to use the default value for all parameters, except the `secret` one where you should put some random characters/digits.

8. When you're done, execute the following command to get the last version of the database schema:

        php app/console doctrine:migrations:migrate

  You may also want to load some data into your database:

        php app/console doctrine:fixtures:load

9. Now come back to your host machine (where your browser is waiting for you), then [update your hosts file](https://puphpet.com/help#update-my-hosts-file) by appening this line:

        192.168.100.50 dextr.dev

10. And voila, you should be able to see the application running on your browser at this address: 

        http://dextr.dev/app_dev.php/


---

Troubleshooting
-------------------------------

#### `vagrant up` does not work =( 
> If you're working on a UNIX system, make sure you have **NFS** enable on your host machine. By running `sudo apt-get install nfs-kernel-server nfs-common portmap` . Then try again.

#### `composer install` does not work =(
> If you see that composer can't download all the dependencies because of a *timeout exception*, you'll need to run `export COMPOSER_PROCESS_TIMEOUT=3600` (that will increase the composer process timeout param which is limited to 300 seconds by default). Now try again, and wait...
  


