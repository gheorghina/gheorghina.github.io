---
layout: post
title:  "Hot to setup with Vagrant and Ansible on Windows 10"
date:   2018-11-24 00:18:23 +0700
categories: [ansible, vagrant, windows10, linux]
---

If all you have is a windows 10 machine, but if you have to build an automated configuration deployment with Ansible on a Linux machine, this small article comes to rescue.
You can easily creade and destroy new infrastructure configurations on your local machine with Vagrant. 

In the next steps I will describe how you can create virtual linux machines on a windows 10 system with Vagrant and then how you can access them to install Ansible and run a small setup configuration on them.


**Prerequisites**

  1. Enabling Linux on Windows 10 is now wasy. 
     You can check on this article for a detailed net to step approach: [Linux on Windows 10](https://www.laptopmag.com/articles/use-bash-shell-windows-10) 

  2. Install ubuntu bash: [Ubuntu bash on Windows 10](https://www.windowscentral.com/how-install-bash-shell-command-line-windows-10)   

  3. Upgrade powershell to latest version: [Powershell](https://github.com/PowerShell/PowerShell)  

  4. Download and install Vagrant: [Vagrantup](https://www.vagrantup.com) 


**Setting up a Vagrant Configuration**

  1. Create a path on your local machine, where you would like to have the vagrant configuration. 
     Make sure the path does not contain any space in the names. I took me a while to catch this limitation.

     Let's say we work with: d:\vagrant\demo

  2. Create a vagrant file: `vagrantfile`. The next script sample shows a configuration for two similar linux virtual machines, with 2 CPUs and 2 GB of Memory.

      ```
      #########################################################################
      ###### VM1 config #####################################################
      #########################################################################
      config.vm.define "vm1", primary: true do |vm1|
        vm1.vm.box = "centos/7"
        
        vm1.vm.network "private_network", ip: "192.160.55.10"

        vm1.vm.provider "virtualbox" do |vb|
          # Don't boot with headless mode
          # vb.gui = true
          
          # Use VBoxManage to customize the VM. For example to change memory:
          vb.memory = 2048
          vb.cpus = 2
        end
      end

      #########################################################################
      ###### VM2 config ####################################################
      #########################################################################
      config.vm.define "vm2", autostart: false do |vm2|
        vm2.vm.box = "centos/7"

        vm2.vm.network "private_network", ip: "192.160.55.20"
        
        vm2.vm.provider "virtualbox" do |vb|
          # Don't boot with headless mode
          # vb.gui = true
          
          # Use VBoxManage to customize the VM. For example to change memory:
          vb.memory = 2048
          vb.cpus = 2
        end
      end

      ```

  3. For creating the virtual machines, the `virtual up machine_name` command has to be used
     As you can notice, the first machine configuration specifies that vm1 is the primary machine, this means that the `machine_name` can be skipped from the command.

     Therefore, start the `cmd` under `admin` rights and navigate to `d:\vagrant\demo` path where the `vagrantfile` can be found

     For creating the first machine, execute


     ```
     $ vagrant up
     ```

     For creating the second machine, execute


     ```
     $ vagrant up vm2
     ```

  4. to be continued..


**Other Good References** 

[Ansible Docs](https://docs.ansible.com/ansible/2.5/reference_appendices/faq.html)


[Dev Docs](https://devdocs.io/)


That's it! Now you are ready to go! 


