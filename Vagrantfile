# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :master => {
        :box_name => "centos7",
        :ip_addr => '192.168.11.150'
  },
  :slave => {
        :box_name => "centos7",
        :ip_addr => '192.168.11.151'
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "2048"]
            # Подключаем дополнительные диски
            #vb.customize ['createhd', '--filename', second_disk, '--format', 'VDI', '--size', 5 * 1024]
            #vb.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 0, '--device', 1, '--type', 'hdd', '--medium', second_disk]
          end

      if boxname.to_s == "slave"
      box.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/main.yaml"
      ansible.host_key_checking = "false"
      ansible.limit = "all"
      end
	end
	end
  end
end
