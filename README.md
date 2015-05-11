# vagrant-ceph
Create a vagrant configuration to support multiple ceph cluster topologies.  Ideal for development or exploration of Ceph.

## Usage
Review the config.yml.  All addresses are on private networks.  Each commented section lists the requirements for a configuration and approxiamate initialization time.

The current setup only supports libvirt as a provider.  Install the plugin if needed.

`$ vagrant plugin install vagrant-libvirt`

Note: fog-1.30.0 seems to remove support for libvirt.  To workaround this issue, run the following as well if needed

`$ vagrant plugin uninstall fog`

`$ vagrant plugin install --plugin-version 1.29.0 fog`

Next, add the vagrant box.  (If the following fails, see <a href="#Downloading">Downloading boxes</a> below)

`$ vagrant box add boxes/VagrantBox-openSUSE-13.2.x86_64-1.13.2.libvirt.json`

Edit the _Vagrantfile_ and set BOX, INSTALLATION and CONFIGURATION.  Use the following for an initial test.

`BOX = 'VagrantBox-openSUSE-13.2'` <br>
`INSTALLATION = 'ceph-deploy'` <br>
`CONFIGURATION = 'small'` <br>

Start the environment.

`$ vagrant up`

Note that the base box image and VM OS disks are added to /var/lib/libvirt/images with additional disks added as 1G raw partitions for the data nodes.

Next, log into the admin node and become root.

`$ vagrant ssh admin`

`vagrant@admin:~> sudo su -`

You may now begin a ceph installation.  

## <a name="Downloading"></a>Downloading boxes
If the boxes do not download automatically, download the boxes from Google Drive.

[openSUSE 13.2](https://drive.google.com/file/d/0B1474649NCkYZ0RqVkFudkNJR0U/view?usp=sharing) <br>
[openSUSE Tumbleweed](https://drive.google.com/file/d/0B1474649NCkYSWQzSGRYTWo2d0U/view?usp=sharing) <br>
[SLE 12](https://drive.google.com/file/d/0B1474649NCkYNkZLZGpoTUtmanc/view?usp=sharing)

and save to the _boxes/_ subdirectory.  Edit the appropriate _json_ file, remove the url entry and change '\_local-example\_url' to 'url'.  Then run

`$ vagrant box add boxes/VagrantBox-openSUSE-13.2.x86_64-1.13.2.libvirt.json`

## Caveats
For the sake of completeness and stating the obvious, the private ssh key is only suitable for demonstrations and should never be used in a real environment.

The automation does not install Ceph.  

The default root password is 'vagrant'.

Depending on your hardware, an ssh command may fail on the initialization when a virtual machine did not respond quickly enough.  Run the provisioning again with

`$ vagrant provision`
