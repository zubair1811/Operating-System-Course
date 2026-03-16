
# Operating Systems Course Repository

📌 Overview
- This repository contains  coursework, experiments, notes, and projects related to the Operating Systems (OS) course.
- The goal of this repository is to document the  learning process and provide practical implementations of core OS concepts.
- Topics covered include process management, memory management, file systems, system calls, and various OS tools.


## Practicle-1
### Install the SSH client and server in Linux OS

## Ubuntu OS
Download ISO File: https://ubuntu.com/download
```
sudo apt update
sudo apt install openssh-client
sudo apt install openssh-server
sudo systemctl status ssh
```

## CentOS OS
Download ISO File: https://www.centos.org/download/
```
sudo yum update -y
sudo yum –y install openssh-server openssh-clients
sudo systemctl start sshd
sudo systemctl status sshd
sudo systemctl enable sshd    
```
