# Vagrant virual machine configuration for greenplum testing

Your need install vagrant and virtualbox for up this configuration.

Your need install [vagrant](https://github.com/hashicorp/vagrant-installers/releases/tag/v2.3.4.dev%2Bmain "vagrant") and  [virtualbox](https://www.virtualbox.org/ "virtualbox") for up this configuration. Optional you can use [make](https://www.gnu.org/software/make/ "make").

### Step 1

Download box bento/ubuntu-22.04 for virtualbox from [vagrantup](https://app.vagrantup.com/bento/boxes/ubuntu-22.04 "vagrantup").

### Step 2

Clonr this repository: git clone https://github.com/codesshaman/vagrant_greenplum_ubuntu.git

### Step 3

Copy box and go inside the repository folder:

``cp ~/Downloads/fcd7e50d-f8f2-4156-8349-dbe7cc378101 path_to/vagrant_greenplum/ubuntu``

``cd vagrant_greenplum``

### Step 4

Inicialize configuration:

``vagrant box add bento/ubuntu-22.04 ubuntu``

or with make:

``make build``

### Step 5

Install configuration:

``vagrant up --provider=virtualbox``

or with make:

``make``

### Step 6

Connect:

``ssh vagrant@192.168.58.88``

or with make:

``make connect``
