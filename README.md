Debian Jessie base box for Vagrant, up to date
----------------------------------------------

This repository holds a configuration file for bootstrap-vz, meant to
create a minimal Debian Jessie base box to be used in Vagrantfiles.

Use [boostrap-vz](http://andsens.github.io/bootstrap-vz/) from the git 'development' branch, run on Debian
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

Notes :
-------

* The following patch may be needed to fix a volume size calculation
  (see it [on github](https://github.com/andsens/bootstrap-vz/issues/127)):

```diff
diff --git a/bootstrapvz/common/fs/qemuvolume.py b/bootstrapvz/common/fs/qemuvolume.py
index 2f9adfe..955621d 100644
--- a/bootstrapvz/common/fs/qemuvolume.py
+++ b/bootstrapvz/common/fs/qemuvolume.py
@@ -8,7 +8,7 @@ class QEMUVolume(LoopbackVolume):
 
        def _before_create(self, e):
                self.image_path = e.image_path
-               vol_size = str(self.size.get_qty_in('MiB')) + 'M'
+               vol_size = str(int(self.size.get_qty_in('MiB')*1024.0/1000.0)) + 'M'
                log_check_call(['qemu-img', 'create', '-f', self.qemu_format, self.image_path, vol_size])
 
        def _check_nbd_module(self):

```

* The following patch makes use of zerofree to diminish the box size
  by trimming unused disk space :

```diff
diff --git a/manifests/virtualbox-vagrant.manifest.yml b/manifests/virtualbox-vagrant.manifest.yml
index e440ca7..36721d1 100644
--- a/manifests/virtualbox-vagrant.manifest.yml
+++ b/manifests/virtualbox-vagrant.manifest.yml
@@ -30,3 +30,6 @@ volume:
 packages: {}
 plugins:
   vagrant: {}
+  minimize_size : {
+    zerofree: true
+    }
```


TODOs :
-------
* none currently


