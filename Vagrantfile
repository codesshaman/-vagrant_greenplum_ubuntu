# -*- mode: ruby -*-
# vi: set ft=ruby :

###########################
## Network configuration ##
###########################

# All available ranges here
# 10.10.0.2 – 10.255.255.255
# 172.16.0.2 – 172.31.255.255
# 192.168.0.0 – 192.168.255.255
# (don't uncommit, for example only)

###########################
######### Default #########
# 192.168.56.11 - master_1  #
# 192.168.56.12 - master_2  #
# 192.168.56.13 - worker_1  #
# 192.168.56.14 - worker_2  #
# 192.168.56.15 - worker_3  #
# 192.168.56.16 - worker_4  #
###########################

OS = 'bento/ubuntu-22.04'

# You can change ip here
# (Range of ip addresses
# Without last octet):
IP_ADDRESS = "192.168.56"

# First subnet ip number for range
IP = 10

# Number of master nodes
NUM_MASTERS = 1
# Number of worker nodes
NUM_WORKERS = 1

# Name for the master nodes
MASTER_NAME = "master_of_puppets_"
# MASTER_NAME = "gp_master_host"
# Name for the worker nodes
WORKER_NAME = "puppet_"
# WORKER_NAME = "gp_worker_node"

# Alias for the master nodes
MASTER_ALIAS = "master"
# Alias for the worker nodes
WORKER_ALIAS = "worker"

# Variables lists fot the
# scripst (don't touch!)
MASTERS_LIST = ""
WORKERS_LIST = ""
MASTERS_IPS = ""
WORKERS_IPS = ""

i = 0
c = 10

# Variable creation
while i < NUM_MASTERS
    c += 1
    MASTERS_IPS = MASTERS_IPS + "#{IP_ADDRESS}.#{c}" + " "
    i += 1
    MASTERS_LIST = MASTERS_LIST + "#{MASTER_ALIAS}#{i}" + " "
end
i = 0
while i < NUM_WORKERS
    c += 1
    WORKERS_IPS = WORKERS_IPS + "#{IP_ADDRESS}.#{c}" + " "
    i += 1
    WORKERS_LIST = WORKERS_LIST + "#{WORKER_ALIAS}#{i}" + " "
end

# First port from the range
# For workers and masters
# WORKER_PORT = 9090
# MASTER_PORT = 8080

# CPU and memory
MASTER_CPU = "2"
MASTER_MEMORY = "16384"

SLAVE_CPU = "2"
SLAVE_MEMORY = "16384"

# Mac addresses
MAC_LIST_M = [
    '080027b893a3',
    '080027a20ad1',
    '08002740d567',
    '0800277489c4',
    '0800278a6977',
    '080027928568',
    '08002705b7d3',
    '080027b176c1',
    '0800278bc27b'
]

MAC_LIST_W = [
    '080027301f31',
    '08002708fd26',
    '080027a3e6a7',
    '0800278b0c4c',
    '080027836753',
    '080027ec85cc',
    '080027149319',
    '08002778228d',
    '080027598ca6'
]

# Unix default ssh key folder
# (create key with ssh-keygen)
key = File.read("#{Dir.home}/.ssh/id_rsa.pub")
# Masters and workers cycles
Vagrant.configure('2') do |config|
    # ######################### #
    # Master nodes create cycle #
    # ######################### #
    (1..NUM_MASTERS).each do |n|
        config.vm.define "master#{n}" do |master|
            IP += 1
            master.vm.box = OS
            master.vm.hostname = "master#{n}"
            # master.vm.network "forwarded_port",
            # guest: MASTER_PORT + n, host: MASTER_PORT + n
            master.vm.network 'private_network',
            ip: "#{IP_ADDRESS}.#{IP}", subnet: "255.255.255.0"
            master.vm.provision "copy ssh public key", type: "shell",
            inline: "echo \"#{key}\" >> /home/vagrant/.ssh/authorized_keys"
            master.vm.provision "shell", privileged: true, path:"setup.sh",
            args: [MASTERS_LIST, MASTERS_IPS, WORKERS_LIST, WORKERS_IPS]
            master.vm.synced_folder "shared", "/home/vagrant/shared_folder"
            master.vm.provision "shell", inline: "sudo swapoff -a"
            # master.vm.provision "shell",
            # inline: "sed -i 's!/dev/mapper/debian--11--vg-swap!#/dev/mapper/debian--11--vg-swap!1' /etc/fstab"
            master.vm.provider 'virtualbox' do |v|
                v.customize ["modifyvm", :id,
                "--macaddress1", "#{MAC_LIST_M[n]}"]
                v.name = "#{MASTER_NAME}#{n}"
                v.memory = MASTER_MEMORY
                v.cpus = MASTER_CPU
            end
        end
    end
    # ######################### #
    # Worker nodes create cycle #
    # ######################### #
    (1..NUM_WORKERS).each do |n|
        config.vm.define "worker#{n}" do |worker|
            IP += 1
            worker.vm.box = OS
            worker.vm.hostname = "worker#{n}"
            # worker.vm.network "forwarded_port",
            # guest: WORKER_PORT + n, host: WORKER_PORT + n
            worker.vm.network 'private_network', 
            ip: "#{IP_ADDRESS}.#{IP}", subnet: "255.255.255.0"
            worker.vm.provision "copy ssh public key", type: "shell",
            inline: "echo \"#{key}\" >> /home/vagrant/.ssh/authorized_keys"
            worker.vm.provision "shell", privileged: true, path:"setup.sh",
            args: [MASTERS_LIST, MASTERS_IPS, WORKERS_LIST, WORKERS_IPS]
            worker.vm.synced_folder "shared", "/home/vagrant/shared_folder"
            worker.vm.provision "shell", inline: "sudo swapoff -a"
            # worker.vm.provision "shell",
            # inline: "sed -i 's!/dev/mapper/debian--11--vg-swap!#/dev/mapper/debian--11--vg-swap!1' /etc/fstab"
            worker.vm.provider 'virtualbox' do |v|
                v.customize ["modifyvm", :id,
                "--macaddress1", "#{MAC_LIST_W[n]}"]
                v.name = "#{WORKER_NAME}#{n}"
                v.memory = SLAVE_MEMORY
                v.cpus = SLAVE_CPU
            end
        end
    end
end
