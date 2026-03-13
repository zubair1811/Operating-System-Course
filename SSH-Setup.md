# Install the SSH client and server in Linux OS

## Ubuntu OS
```
sudo apt update
sudo apt install openssh-client
sudo apt install openssh-server
sudo systemctl status ssh
```

## CentOS OS
```
sudo yum update -y
sudo yum –y install openssh-server openssh-clients
sudo systemctl start sshd
sudo systemctl status sshd
sudo systemctl enable sshd    
```
