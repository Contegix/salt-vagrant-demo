=================
Salt Vagrant Demo
=================

A Salt Demo using Vagrant.


Instructions
============

Run the following commands in a terminal. Git, VirtualBox and Vagrant must
already be installed.

.. code-block:: bash

    git clone https://github.com/UtahDave/salt-vagrant-demo.git
    cd salt-vagrant-demo
    vagrant plugin install vagrant-vbguest
    vagrant up


This will download an Centos 7 VirtualBox image and create six virtual
machines for you.

You can then run the following commands to log into the Salt Master and begin
using Salt.

.. code-block:: bash

    vagrant ssh master
    sudo salt \* test.ping
