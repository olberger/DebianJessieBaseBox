Debian Jessie base box for Vagrant, up to date
----------------------------------------------

This repository holds a configuration file for bootstrap-vz, meant to
create a minimal Debian Jessie base box to be used in Vagrantfiles.

Use [boostrap-vz](http://bootstrap-vz.readthedocs.org) from the git 'master' branch, run on Debian
testing/jessie to generate an image, using the manifest provided in
'bootstrap-vz-manifests/'.

Syntax : sudo ./bootstrap-vz --debug --log logs bootstrap-vz-manifests/virtualbox-vagrant.manifest.yml

Note:
 * set the correct 'architecture': 'amd64' or 'i386' in the manifest file.

The resulting bas box will be found in /target/debian-jessie-amd64-YYMMDD.box

Then upload it to vagrantcloud/atlas.hashicorp.com.

Cf. "oberger/debianjessiemini-[amd64|i686]"
[on vagrantcloud](https://vagrantcloud.com/oberger/) for releases of
the boxes.

Testing the base box
--------------------

You may use vagrant add to register the box locally, something like :
 $ vagrant box add test-debian-amd64 /target/debian-jessie-amd64-YYMMDD.box

Then create a test dir and use vagrant init and vagrant up inside.

TODOs :
-------
* none currently


