# -*- mode: ruby -*-
# vi: set ft=ruby :

VB_NAME ||= "WolfieMouse"
FOLDER_NAME ||= "wolfiemouse"

# A function that add a usb device without duplication.
# See: https://github.com/mitchellh/vagrant/issues/5774

def usbfilter_exists(name)
  #
  # Determine if a usbfilter with the provided Vendor/Product ID combination
  # already exists on this VM.
  #
  # TODO: Use a more reliable way of retrieving this information.
  #
  # NOTE: The "machinereadable" output for usbfilters is more
  #       complicated to work with (due to variable names including
  #       the numeric filter index) so we don't use it here.
  #
  machine_id_filepath = ".vagrant/machines/default/virtualbox/id"
  if not File.exists? machine_id_filepath
    # VM hasn't been created yet.
    return false
  end
  file = File.open(machine_id_filepath, "r")
  machine_id = file.read
  vm_info = `VBoxManage showvminfo #{machine_id}`
  filter_match = "Name:             #{name}\n"
  return vm_info.include? filter_match
end

def better_usbfilter_add(vb, filter_name, manufacturer, product)
  #
  # This is a workaround for the fact VirtualBox doesn't provide
  # a way for preventing duplicate USB filters from being added.
  #
  # TODO: Implement this in a way that it doesn't get run multiple
  #       times on each Vagrantfile parsing.
  #
  if not usbfilter_exists(filter_name)
    vb.customize ["usbfilter", "add", "0",
       "--target", :id,
       "--name", filter_name,
       "--manufacturer", manufacturer,
       "--product", product]
  end
end

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # Enable X11 forwarding
  config.ssh.forward_x11 = true

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

  # Use a temporary box instead.
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # RDP
  # config.vm.network "forwarded_port", guest: 3389, host: 33890
  # VNC
  # config.vm.network "forwarded_port", guest: 5901, host: 59010

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.

  config.vm.synced_folder "./", "/home/vagrant/" + FOLDER_NAME

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |vb|
    # # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Set virtualbox name
    vb.name = VB_NAME

    # Customize the amount of memory on the VM:
    vb.memory = "2048"

    # Enable USB device
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbxhci", "on"]

    # Add USB device filter.
    # Reference: http://code-chronicle.blogspot.com/2014/08/connect-usb-device-through-vagrant.html
    # https://github.com/mitchellh/vagrant/issues/5774
    better_usbfilter_add(vb, "J-Link Debugger", "SEGGER", "J-Link")
    better_usbfilter_add(vb, "STLink Debugger", "STMicroelectronics", "STM32 STLink")
    better_usbfilter_add(vb, "FTDI USB UART", "FTDI", "FT232R USB UART")
  end
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.

<<<<<<< HEAD
  config.vm.provision "shell", path: "tools/vagrant/install_usb_serial_modules.sh"
  config.vm.provision "shell", path: "tools/vagrant/install_gcc_arm_tools.sh"
  config.vm.provision "shell", path: "tools/vagrant/eclipse_cdt.sh"
  config.vm.provision "shell", path: "tools/vagrant/install_coolterm.sh"
  config.vm.provision "shell", inline: "sudo apt-get install -y gdb python3 python3-pip python python-pip"
=======
>>>>>>> 1fc1053c20261bdbe2bb2a9c781c14dc135a1002
  # Install ZSH and grml-zsh-config.
  # See https://grml.org/zsh/#grmlzshconfig
  # ZSH must be installed first, as other installations depends on .zshrc
  config.vm.provision "shell", inline: "sudo apt-get install -y zsh wget && sudo chsh -s /usr/bin/zsh vagrant"
  config.vm.provision "shell", inline: "wget -nv -O /home/vagrant/.zshrc https://git.grml.org/f/grml-etc-core/etc/zsh/zshrc && chown vagrant:vagrant /home/vagrant/.zshrc"
  config.vm.provision "shell", inline: "wget -nv -O /home/vagrant/.zshrc.local  https://git.grml.org/f/grml-etc-core/etc/skel/.zshrc && chown vagrant:vagrant /home/vagrant/.zshrc.local"
  config.vm.provision "shell", path: "tools/vagrant/modify_shell.sh"
  # Install tools
  config.vm.provision "shell", path: "tools/vagrant/install_usb_serial_modules.sh"
  config.vm.provision "shell", path: "tools/vagrant/install_gcc_arm_tools.sh"
  # We don't use eclipse anymore.
  #config.vm.provision "shell", path: "tools/vagrant/eclipse_cdt.sh"
  config.vm.provision "shell", path: "tools/vagrant/install_coolterm.sh"
  config.vm.provision "shell", inline: "sudo apt-get install -y gdb python3 python3-pip python python-pip"

  # The official Vagrant Ubuntu box has a broken group permssion.
  # So a group for USB should be manually added.
  config.vm.provision "shell", inline: "sudo usermod -aG plugdev vagrant"
end
