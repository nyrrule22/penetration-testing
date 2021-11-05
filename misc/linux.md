# Linux

## Kali

## Commands

### Sudo Overview

```bash
# Run command as root
sudo cat /etc/shadow
# Switch to root user
sudo su -
sudo -i
```

### Navigating Filesystem

```bash
pwd  # Print working directory
cd  # Change directory
ls  # List files and directories
mkdir  # Make a directory
touch  # Make a file
cp  # Copy a file or directory
rm  # Remove a file or directory
mv  # Move or rename a file or directory
locate  # Locat a file or directory
man  # Manual pages for a command
```

### Users and Privileges

```bash
d rwx r-x r-w  # Directory with permissions 755
- rwx r-- r--  # File with permissions 744
chmod  # Change mode of a file or directory
chown  # Change owernship of a file or directory
adduser  # Adds a new user
passwd  # Change the password for a user
su  # Switch/substitute to a user
```

### Network Commands

```bash
ifconfig / ip a  # Configure a network interface
iwconfig  # Configure a wireless network interface
ping  # Send ICMP ECHO_REQUEST to network hosts
arp  # Resolve IP address to a MAC address
netstat  # Print network connections, tables, and statistics
route / ip r  # Print routing table
```

### Installing and Updating Tools

```bash
apt update  # Updates list of available packages and their versions, but it does not install or upgrade them.
apt upgrade  # Actually installs newer versions of the packages you have.
apt install  # Install software
git clone  # Download software from GitHub or other git provider
```

### Files

```bash
echo "hello" > file.txt  # Send/overwrite text to a file
echo "world" >> file.txt  # Append text to a file
touch  # Create a new file
vim/nano  # CLI file editors
gedit  # UI file editor
cat  # Concatenate files and print on the standard out
```

## Scripts
