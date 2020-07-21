# -*- mode: ruby -*-
# vi: set ft=ruby :

# carrega o mesmo inventário do ansible, para termos uma fonte única
# das especificações
require 'yaml'
inventario = YAML.load_file("inventario.yml")

# não uso shared folders do VirtualBox porque causam problemas demais
# se o sshfs estiver disponível, use ele; caso contrário, use rsync
s_f_type = Vagrant.has_plugin?("vagrant-sshfs") ? "sshfs" : "rsync"

Vagrant.configure("2") do |config|

  if Vagrant.has_plugin?("vagrant-vbguest")
    # mais trabalho do que vale a pena
    # o que as guest additions trazem, não usamos neste laboratório:
    # - mouse pointer integration
    # - shared folders
    # - drag and drop
    # - hardware-accelerated graphics
    # - seamless windows
    # - time synchronization
    # - guest properties, control file manager and control of applications
    # - memory overcommit
    config.vbguest.auto_update = false
  end

  inventario["all"]["hosts"].each do |hostname, maquina|
    config.vm.define hostname do |config|
      config.vm.hostname = hostname
      config.vm.box = maquina["box"] || 'centos/8'
      grupo = maquina["grupo"] || File.basename(File.dirname(__FILE__))

      # sshfs quando possivel, rsync caso contrario. evita nfs
      config.vm.synced_folder ".", "/vagrant", type: s_f_type, disabled: true

      # rede host-only padrão do virtualbox: 192.168.56.0/24
      config.vm.network "private_network", ip: maquina["ansible_host"], auto_config: true

      config.vm.provider "virtualbox" do |virtualbox|
        virtualbox.name = hostname
        # agrupa as VMs deste laboratório
        virtualbox.customize ["modifyvm", :id, "--groups", "/" + grupo]

        virtualbox.cpus = maquina["cpus"] || 1
        virtualbox.memory = maquina["memoria"] || 512

        # usa tipo ideal de nic
        virtualbox.default_nic_type = "virtio"

        # usa controlador de video recomendado
        virtualbox.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
        virtualbox.customize ["modifyvm", :id, "--vram", "16"]

        # desabilita audio
        virtualbox.customize ["modifyvm", :id, "--audio", "none"]

        # desabilita saida de console para log
        virtualbox.customize ["modifyvm", :id, "--uart1", "off"]
        virtualbox.customize ["modifyvm", :id, "--uartmode1", "disconnected"]

        # desabilita dispositivos USB
        virtualbox.customize ["modifyvm", :id, "--usbohci", "off"]
        virtualbox.customize ["modifyvm", :id, "--usbehci", "off"]
        virtualbox.customize ["modifyvm", :id, "--usbxhci", "off"]
      end

      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "vagrant/vagrant.yml"
        ansible.host_vars = { hostname => {"ip" => maquina["ansible_host"], "grupo" => grupo} }
        ansible.skip_tags = ["upgrade"]
      end
      if Vagrant.has_plugin?("vagrant-reload")
        config.vm.provision :reload
      else
        config.vm.provision "shell", inline: "echo $(tput blink)$(tput bold)Execute agora \'vagrant reload\' para aplicar algumas configurações de provisionamento"
      end
    end
  end
end
