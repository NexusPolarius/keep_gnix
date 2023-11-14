# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure(2) do |config|
  
  machines = ["gnix1", "gnix2", "db", "back1", "back2", "storage"]
  ip_mach = {"gnix1"=>"192.168.56.10",
             "gnix2"=>"192.168.56.11",
             "back1"=>"192.168.56.12",
             "back2"=>"192.168.56.13",
             "storage"=>"192.168.56.14",
             "db"=>"192.168.56.15"} 

  machines.each do |machine|
     config.vm.define machine do |conf|
       conf.vm.network "private_network", adapter: 2, :device => "eth1", auto_config: false, virtualbox__intnet: "internal-net"
       conf.vm.network "private_network", adapter: 3, :device => "eth2", ip: ip_mach[machine], netmask: "255.255.248.0"
       conf.vm.hostname = machine
       conf.vm.box = "cent7"
       #conf.vm.box = "ubuntu/bionic64"
       #conf.vm.box_version = "20230607.0.0"
       conf.vm.provider :virtualbox do |vb|
         vb.name = machine
         vb.customize ["modifyvm", :id, "--memory", "2048"]
         vb.customize ["modifyvm", :id, "--cpus", "2"]
         if machine == "storage"
            unless File.exist?('./SecondDisk.vmdk')
            	vb.customize ['createhd', '--filename', './SecondDisk.vmdk', '--variant', 'Fixed', '--size', 20 * 1024]
            end
            vb.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', './SecondDisk.vmdk']
         end   
       end
       #if machine == "storage"
       #     conf.vm.disk :disk, disk_ext: "vhd", primary: false, size: "20GB", name: "storage05"
       #end   
     end
  end

end
