# ansible-kstars-pi

Ansible playbook to install and configure KStars and VNC server on a Raspberry Pi with Ubuntu.

## Requirements
Install ansible on your local machine:

MacOS:
```bash
brew install ansible
```
Ubuntu:
```bash
sudo apt install ansible
```
Don't know how to install ansible on Windows, but you can use WSL2 or a Docker container with VSCode for example.

Then, run `install script` to install ansible-galaxy roles and copy vars file:
```bash
./install
```

or manually:
```bash
ansible-galaxy role install gantsign.oh-my-zsh
```
don't forget to edit vars.yml.example and rename it to `vars.yml`, it's included in .gitignore so if BE CAREFUL if you rename it, edit `.gitignore` file ...

## Ansible usage
```bash
ansible-playbook playbook.yml -i "ip address of your raspberry pi," -u "username of your raspberry pi" --ask-pass --ask-become-pass
```

actually i rename my user (not pi), my user is in sudoers file without password, and I connect with SSH key, so i launch playbooks like this:
```bash
ansible-playbook playbook.yml -i "192.168.1.2," -u "juju"
```
DON'T FORGET THE COMMA AFTER THE IP ADDRESS (unless you have multiple hosts in an inventory file)

## Playbooks

- 1-softs.yml: Install XFCE4, lightdm,  vnc server, oh-my-zsh, log2ram

- 2-astro.yml: Install KStars, INDI and PHD2

- 3-screen.yml: Install MHS35 screen driver for testing purpose, don't use it if you don't know what it is

## Raspberry Pi Usage

After few seconds, you can connect to your raspberry pi with VNC Viewer (or any other VNC client) on port 5901
If your pi doesn't connect to your wifi, you can look for the SSID of your AP you set in vars.yml

Wifi works with netplan and NetworkManager, so you can use nmcli to connect to your wifi:
```bash
sudo nmcli c up netplan-wlan0-<your client ssid>
```
to connect it to your wifi, and
```bash
sudo nmcli c up netplan-wlan0-<your ap ssid>
```
to transform your pi into an AP.

If you want to change credentials of your AP, you can edit vars.yml and run playbook `1-softs.yml` again or edit `/etc/netplan/01-netcfg.yaml` and run:
```bash
sudo netplan generate
sudo netplan apply
```