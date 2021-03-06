vagrant-centos
==============

Install
-------

This script depends on VirtualBox and Vagrant. Make sure both are
installed; on Mac OSX this can be done with Cask:

- brew tap Caskroom/cask
- brew cask install virtualbox
- brew cask install vagrant

Download
--------

Download a minimal CentOS 7 disk image from https://wiki.centos.org/Download

Run
---

```
./setup /path/to/CentOS.iso ./ks.cfg
```

This will launch a new VirtualBox VM, mount the CentOS install iso and run an
automated installation to build a box ready for packaging as a CentOS server.

`VBoxHeadless` will run for at least 10 minutes with no output.  When
it finishes, Vagrant will package a .box to the boxes subdirectory.

Congratulations! You have just created a new Vagrant CentOS server
box.  Add it to the Vagrant box list with `vagrant box add boxes/centos70-x86_64.box --name centos7`

Specification
-------------

The box is constrained to 613 MiB of memory to vaguely resemble an
Amazon AWS micro instance. You may want to consider adjusting this
for your needs using options like:

    config.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", 2048]
        vb.customize ["modifyvm", :id, "--ioapic", "on", "--cpus", 2]
    end

in your `Vagrantfile`.

This box has a heavy bias towards US English locales. If this
affects you, please adjust the `ks.cfg` file accordingly. This was
mainly done as a space saving measure.


Additional Notes
----------------

Please be aware that these scripts will *not* install any special
provisioners beyond the shell. Patches will be considered if you
wish to contribute support for Puppet, Chef, etc.

The development tools group package is also included for
convenience. This includes things like `gcc` and `make` as well as
VCSs like `git`, `hg`, `bzr`, etc.

Environmental variables can be set to alter the configuration to best
suit your needs; see ./setup for the configuration options.

If you wished to be emailed with the various logs the build produces
see the `ks.cfg` file and find the line:

    EMAIL=root

and adjust accordingly.
