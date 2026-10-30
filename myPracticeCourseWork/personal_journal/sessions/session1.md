[Personal Learning Record](../../personal_journal/personal_journal.md) | [Session Notes](../sessions/README.md) 

# COM511 Network Systems Automation - Session 1

## Topics covered
Exercise 
In Session 1, I learned about Git and GitHub, operating systems, virtualisation, Vagrant, Linux package management, web servers, networking and automated provisioning.

The session introduced how virtual machines can be created using VirtualBox and managed through Vagrant. I also explored the differences between Ubuntu and Rocky Linux, including their package managers and Apache service names.

The exercises covered:

Exercise 1.1 — Git and GitHub

Exercise 1.2 — VirtualBox and Linux virtual machines

Exercise 1.3 — Vagrant networking and manual Apache installation

Exercise 1.4 — Automated Apache installation using Vagrant

Exercise 1.5 — Rocky Linux Apache web server configuration

## Personal Notes and research following this session

Git and GitHub

Git is a version control system that records changes made to files. GitHub is an online platform used to host Git repositories and collaborate on projects.

I learned that version control is important for infrastructure automation because configuration files and scripts can be tracked, reviewed and restored if necessary.

Virtualisation 

Virtualisation allows a computer to run multiple operating systems through virtual machines.

Host: The physical computer running Windows.

Guest: The operating system running inside a virtual machine.

Hypervisor: Software that manages the virtual machine's hardware resources.

VirtualBox: A type 2 hypervisor used to run virtual machines on an existing operating system.

Vagrant

Vagrant is a tool used to create and manage virtual machines using configuration files. The Vagrantfile defines settings such as the operating system image, network ports and provisioning commands.

Apache

Apache is web server software that handles HTTP requests and delivers webpages. Ubuntu uses the apache2 package, while Rocky Linux uses httpd.

Networking

I learned that Vagrant commonly uses NAT networking. Port forwarding allows a service running inside the virtual machine to be accessed from the host computer.

For example, guest port 80 can be forwarded to host port 8080.

Research links

Git documentation

Vagrant documentation

VirtualBox documentation

Apache documentation

Rocky Linux documentation

## Exercises and results

Exercise 1.1 — Git and GitHub

Status: Completed

The aim of this exercise was to understand version control, create a personal fork of the COM511 repository and maintain my own learning record.

I created my own GitHub fork and organised my coursework using Eclipse IDE.

My repository:

https://github.com/Sashis07/COM511_Network_Systems_Automation

Git commands

The following commands are useful for checking, saving and uploading my coursework:

git status
git remote -v
git add .
git commit -m "Update Session 1 personal journal"
git push

Explanation

git status checks modified and untracked files.

git remote -v shows the configured remote repositories.

git add . stages changes in the current directory and its subdirectories.

git commit records the staged changes in the local repository.

git push uploads committed changes to the remote repository.

These are reference commands for my Git workflow. I will check my Eclipse Git history to confirm which commands or equivalent Eclipse actions I used.

Result

I created my own fork and prepared a structure for storing my personal learning record.

Exercise 1.2 — VirtualBox and Linux Virtualisation

Status: Completed

The aim of this exercise was to understand how virtual machines work and how to create a Linux environment using VirtualBox.

I installed VirtualBox and worked with Ubuntu as a guest operating system on my Windows computer.

I learned that virtual machines allow me to experiment with Linux without replacing the operating system on my laptop.

Commands for inspecting the Linux environment

whoami
hostname
uname -a
cat /etc/os-release
ip address

Explanation

whoami displays the current username.

hostname displays the machine's hostname.

uname -a displays Linux kernel and system information.

cat /etc/os-release identifies the Linux distribution.

ip address displays network interfaces and IP addresses.

Result

I gained experience working with a Linux virtual machine and understanding the difference between host and guest operating systems.

Exercise 1.3 — Vagrant Networking and Manual Apache Installation

Status: Completed

The aim was to create a virtual machine using Vagrant, configure port forwarding and manually install an Apache web server on Ubuntu.

Step 1 — Starting the Vagrant machine

vagrant status
vagrant up
vagrant ssh

Explanation

vagrant status displays the virtual machine's current state.

vagrant up creates or starts the virtual machine.

vagrant ssh connects to the Linux guest through SSH.

Step 2 — Installing Apache on Ubuntu

Inside the Ubuntu machine, the Apache installation commands were:

sudo apt update
sudo apt install -y apache2
sudo systemctl enable --now apache2
sudo systemctl status apache2

The apt package manager installs Apache, while systemctl controls the service.

Step 3 — Configuring port forwarding

Inside the Vagrantfile, I used the following setting:

config.vm.network "forwarded_port", guest: 80, host: 8080

This forwards HTTP traffic from port 80 inside the guest to port 8080 on the Windows host.

After changing the configuration, the VM can be reloaded using:

vagrant reload

Step 4 — Testing the web server

Inside Ubuntu:

curl http://localhost

From Windows:

http://localhost:8080

Result

I completed the manual Apache and Vagrant networking exercise and learned how a service running inside a virtual machine can be accessed from the host.


Exercise 1.4 — Automating Apache Installation

Status: Completed

The aim of this exercise was to automate Apache installation on Ubuntu using Vagrant provisioning.

Instead of installing Apache manually after creating the virtual machine, the required commands can be placed inside the Vagrantfile.

Step 1 — Destroying the existing test VM

From the Ubuntu Vagrant project directory:

vagrant destroy

This removes the existing virtual machine and its guest disk so that the automated installation can be tested from a fresh environment.

Step 2 — Adding a provisioning script

Inside the Ubuntu Vagrantfile:

config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  apt-get install -y apache2
SHELL

Explanation

config.vm.provision "shell" tells Vagrant to run a shell script during provisioning.

apt-get update updates the package index.

apt-get install -y apache2 installs Apache without requiring interactive confirmation.

Step 3 — Starting the machine

vagrant up

Vagrant creates the virtual machine and executes the provisioning commands.

Step 4 — Checking the result

vagrant ssh

Inside Ubuntu:

sudo systemctl is-active apache2
curl http://localhost

Result

I completed the automated Apache installation exercise and learned how provisioning scripts can make virtual machine configuration more consistent and repeatable.


Exercise 1.5 — Rocky Linux Web Server

Status: Completed

The aim of this exercise was to configure an Apache web server on Rocky Linux and understand the differences between Ubuntu and Rocky Linux.

I used the Rocky Linux 9.6 Vagrant example from the coursework repository.

Step 1 — Starting Rocky Linux

From Windows PowerShell:

vagrant status
vagrant up
vagrant ssh

I used the Linux terminal to work with the operating system and web server.

Step 2 — Installing Apache

Rocky Linux uses the httpd package rather than Ubuntu's apache2 package.

sudo dnf install -y httpd
sudo systemctl enable --now httpd
sudo systemctl status httpd

The dnf command installs Apache. The systemctl commands start the service, enable it at boot and display its status.

Step 3 — Creating the HTML webpage

I used the default Apache web directory:

/var/www/html/

The command to create the test page was:

echo '<h1>COM511 Rocky Linux Web Server</h1>' | sudo tee /var/www/html/index.html

I tested the page locally using:

curl http://localhost

Step 4 — Configuring the firewall

I configured the Rocky Linux firewall to allow HTTP traffic.

sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
sudo firewall-cmd --list-services

The firewall configuration returned successful results, and http appeared in the list of permitted services.

Step 5 — Configuring port forwarding

Initially, I checked the Vagrant port configuration:

vagrant port

The output showed SSH port forwarding, but the HTTP port mapping was missing.

I opened the existing Rocky Linux Vagrantfile and found the port-forwarding example commented out.

I enabled this setting:

config.vm.network "forwarded_port", guest: 80, host: 8080

I saved the file and used:

vagrant validate
vagrant reload
vagrant port

Step 6 — Testing the webpage

I opened the following address in my Windows browser:

http://localhost:8080

The webpage loaded successfully.

Result

I successfully accessed my Rocky Linux web server from Windows. This demonstrated that Apache, Vagrant networking and port forwarding were working together.

Problems encountered

Problem 1 — Incorrect location for the configuration command

I initially entered the Vagrant port-forwarding setting directly into the Rocky Linux terminal.

This did not work because the setting belongs in the Vagrantfile, not in the Linux shell.

I corrected the problem by editing the Vagrantfile from Windows.

Problem 2 — Suspended terminal process

While checking the Apache service, the terminal reported a stopped job associated with sudo systemctl status httpd.

I learned that a suspended process can prevent the terminal from exiting normally. I used job-control commands to investigate and terminate the stopped process.

Problem 3 — Website not accessible from Windows

Initially, the webpage did not load through localhost:8080.

I used vagrant port and discovered that the HTTP port forwarding had not been enabled.

After enabling the configuration and reloading Vagrant, the webpage became accessible.


## Summary of learning

Through Session 1, I developed practical knowledge of Git, GitHub, virtualisation, Vagrant, Linux administration and basic infrastructure automation.

I learned how GitHub can be used to maintain coursework and how VirtualBox allows Linux operating systems to run as virtual machines on Windows.

I also learned how Vagrant uses configuration files to create and manage virtual machines. Through the Apache exercises, I gained experience installing software, controlling Linux services, configuring networking and testing HTTP connectivity.

One of the most useful parts of this session was troubleshooting the Rocky Linux web server. Initially, I could not access the webpage from Windows because the port-forwarding setting was not enabled. By checking the Vagrant configuration and making the necessary changes, I successfully accessed the webpage.

I also learned that Ubuntu and Rocky Linux use different package managers and Apache service names. Understanding these differences is important when configuring and automating Linux systems.

Overall, this session helped me understand how virtualisation, networking and automation work together. These skills will be useful for future exercises involving infrastructure management and automated system configuration.