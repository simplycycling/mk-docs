# Linux Dev Setup

This document describes how I set up my development/systems engineer-centric
environment on a fresh Linux installation. I'll be setting up Python, 
Go, Vagrant, and Docker, and using Ansible to do the bulk of the work.

Overall, this guide is specific to my preferred setup - Debian Jessie on
my 15" MBPr, but it will work for many other setups with minor tweaking,
if you know how to use Ansible.

## Installing Debian on the Macbook Pro Retina

Do not forget to fill in this section!

## Pre-Ansible tasks

At this point, Ansible does the bulk of the work for me, and in the future
I may end up ansible-izing a couple of these tasks as well, but I'll write
it out to make it a bit easier on anybody who wants to run Debian on a
MBPr.

### Set up wifi 

This seems to be the big sticking point, for some people. Fortunately,
not that hard.

    Add to sources:
    # Debian 8 "Jessie"
    deb http://httpredir.debian.org/debian/ jessie main contrib non-free
    sudo apt-get update
    sudo apt-get install linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') broadcom-sta-dkms
    sudo modprobe -r b44 b43 b43legacy ssb brcmsmac bcma
    sudo modprobe wl

### Install pip

    sudo apt-get python-pip

### Install Ansible

    sudo pip install ansible

### Install git

    sudo apt-get install git

## Run Ansible

    ansible-playbook main.yml -K
    
I don't like passwordless sudo, so I use the -K flag, which prompts for
sudo password.

## Post-Ansible tasks
