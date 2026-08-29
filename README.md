*This project has been created as part of the 42 curriculum by <login1>.*

# Born2beroot

## Description

**Born2beroot** is a system administration project from the 42 curriculum.  
The goal of this project is to create and configure a secure Linux virtual machine while learning the fundamentals of system administration, virtualization, user management, networking, security and scheduled tasks.

For this project, the operating system chosen is **Debian**.

The main objectives are:

- Install and configure a Linux virtual machine.
- Configure a secure partitioning scheme using LVM.
- Set up a strict password policy.
- Create users and groups with appropriate permissions.
- Configure and secure `sudo`.
- Configure SSH access securely.
- Configure a firewall using UFW.
- Enable and configure AppArmor.
- Create a monitoring script displaying information about the system.
- Run the monitoring script automatically using `cron`.
- Understand the differences between the main tools and technologies used for system administration.

---

## Instructions

### Virtual Machine

The virtual machine was created using **VirtualBox**.

After installing Debian, the system was configured according to the Born2beroot requirements.

### Installation

The ISO image used for the installation is the Debian Linux installer.

During installation, the following elements were configured:

- Hostname
- Encrypted partitions
- LVM partitioning
- Root account
- A personal user account
- Required user groups
- SSH service
- UFW firewall
- AppArmor
- `cron` scheduled tasks

### User and group management

A regular user is created for daily administration instead of using the root account directly.

The user belongs to the required administrative groups, including:

- `sudo`
- `user42`

The `sudo` configuration is restricted and logged to make administrative actions more controlled and traceable.

### SSH

SSH is used for remote administration of the virtual machine.

The SSH service is configured to use a non-default port and direct root login is disabled.

Example connection:

```bash
ssh <username>@localhost -p <ssh_port>
```

### Firewall

UFW (Uncomplicated Firewall) is used to control incoming and outgoing network traffic.

The SSH port used by the virtual machine is allowed through the firewall, while other unnecessary incoming connections are blocked.

Useful commands:

```bash
sudo ufw status
sudo ufw enable
sudo ufw allow <ssh_port>
```

### Monitoring script

The project includes a monitoring script:

```text
/usr/local/bin/monitoring.sh
```

The script displays information such as:

- System architecture
- Number of physical CPUs
- Number of virtual CPUs
- RAM usage
- Disk usage
- CPU load
- Last boot time
- LVM status
- Number of active TCP connections
- Number of logged-in users
- IPv4 address and MAC address
- Number of commands executed with `sudo`

The script is executed automatically using `cron`.

Example:

```cron
*/10 * * * * /usr/local/bin/monitoring.sh
```

Depending on the configuration, the output can be sent as a broadcast message using `wall`.

---

# Resources

## Documentation and references

- Debian documentation
- Linux manual pages (`man`)
- UFW documentation
- AppArmor documentation
- SSH documentation
- `sudo` documentation
- `cron` / `crontab` documentation
- LVM documentation
- VirtualBox documentation

Useful commands for discovering system configuration:

```bash
man sudo
man ssh
man ufw
man cron
man crontab
man passwd
man usermod
man groupadd
```

---

## Operating System Choice

### Debian vs Rocky Linux

#### Debian

**Advantages:**

- Stable and widely used.
- Large package repository.
- Good documentation and community support.
- Uses `apt`, which makes package management straightforward.
- AppArmor is well integrated.
- Lightweight and suitable for a virtual machine.

**Disadvantages:**

- Packages can sometimes be older because stability is prioritized.
- Some software versions may not be the latest available.

#### Rocky Linux

**Advantages:**

- Designed to be compatible with Red Hat Enterprise Linux.
- Commonly used in enterprise environments.
- Uses `dnf` for package management.
- SELinux is deeply integrated into the system.

**Disadvantages:**

- Can require more resources than a minimal Debian installation.
- Package management and administration can be less familiar to beginners coming from Debian-based systems.

### Why Debian was chosen

Debian was chosen because it provides a stable, lightweight and well-documented environment that is particularly suitable for learning Linux system administration inside a virtual machine.

---

# Security

## AppArmor vs SELinux

Both AppArmor and SELinux are Linux security systems based on mandatory access control.

### AppArmor

AppArmor uses profiles that define what resources a program is allowed to access.

**Advantages:**

- Easier to understand and configure.
- Uses path-based rules.
- Well integrated into Debian.
- Convenient for a beginner system administration project.

**Disadvantages:**

- Less granular than SELinux in some situations.
- Mainly focused on restricting applications through profiles.

### SELinux

SELinux uses security labels and policies to control access between processes, users and resources.

**Advantages:**

- Very powerful and granular.
- Widely used in enterprise Linux environments.
- Provides strong mandatory access control.

**Disadvantages:**

- More complex to understand.
- Configuration and troubleshooting can be more difficult.

### Choice

**AppArmor** was chosen because it is integrated with Debian and provides an effective security layer while remaining relatively simple to manage.

---

## UFW vs firewalld

### UFW

UFW stands for **Uncomplicated Firewall**.

It provides a simple interface for configuring firewall rules using commands such as:

```bash
sudo ufw allow 4242/tcp
sudo ufw deny 80/tcp
sudo ufw status
```

**Advantages:**

- Simple syntax.
- Easy to learn.
- Well suited to Debian/Ubuntu systems.
- Good for straightforward firewall configurations.

**Disadvantages:**

- Less flexible for complex firewall configurations.

### firewalld

`firewalld` provides dynamic firewall management using zones and services.

**Advantages:**

- Flexible and powerful.
- Dynamic configuration without restarting the firewall.
- Commonly used on Red Hat-based distributions.

**Disadvantages:**

- More complex for a simple configuration.
- More oriented toward systems using the Red Hat ecosystem.

### Choice

**UFW** was chosen because the project uses Debian and only requires a relatively simple firewall configuration.

---

# Virtualization

## VirtualBox vs UTM

### VirtualBox

VirtualBox is a general-purpose virtualization solution available on multiple operating systems.

**Advantages:**

- Free and open source.
- Available on Windows, Linux and macOS.
- Large community and extensive documentation.
- Easy to configure virtual machines.

**Disadvantages:**

- Can have higher overhead than some virtualization solutions.
- Performance depends on the host configuration.

### UTM

UTM is a virtualization and emulation application particularly popular on macOS.

**Advantages:**

- Good integration with macOS.
- Supports both virtualization and emulation.
- Useful on Apple Silicon systems.

**Disadvantages:**

- Less commonly used in traditional Linux/Windows system administration environments.
- Emulation can be slower when hardware virtualization is not available.

### Choice

**VirtualBox** was chosen because it provides a simple and widely supported environment for running the Debian virtual machine and is commonly used for the Born2beroot project.

---

# Technical Choices

## LVM

**LVM (Logical Volume Manager)** was used to manage the disk space.

Instead of assigning all storage directly to fixed partitions, LVM allows storage to be organized into logical volumes.

Advantages include:

- Flexible storage management.
- Logical volumes can be resized more easily.
- Better separation of system components.
- Easier management of disk space.

---

## Password Policy

A strict password policy is configured to improve system security.

The policy includes requirements such as:

- Minimum password length.
- Password expiration.
- Minimum time before a password can be changed again.
- Maximum password age.
- Restrictions on password composition.

The configuration is handled through the appropriate PAM and login configuration files.

---

## Sudo Policy

`sudo` allows authorized users to execute commands with administrative privileges without logging in directly as root.

The configuration includes:

- Limited administrative access.
- Authentication requirements.
- Logging of sudo commands.
- Restricted attempts.
- A controlled environment for privileged commands.

The configuration can be inspected with:

```bash
sudo visudo
```

---

# Monitoring

The monitoring script is located at:

```text
/usr/local/bin/monitoring.sh
```

It is responsible for periodically displaying the main system information required by the project.

The script can be tested manually with:

```bash
sudo bash /usr/local/bin/monitoring.sh
```

The automated execution is handled by `cron`.

To inspect the current crontab:

```bash
sudo crontab -l
```

To edit it:

```bash
sudo crontab -e
```

---

# Useful Commands

### Check OS

```bash
cat /etc/os-release
```

### Check hostname

```bash
hostname
```

### Check users

```bash
getent passwd
```

### Check groups

```bash
getent group
```

### Check firewall

```bash
sudo ufw status verbose
```

### Check SSH

```bash
sudo systemctl status ssh
```

### Check AppArmor

```bash
sudo aa-status
```

### Check cron

```bash
sudo systemctl status cron
```

### Check disk and LVM

```bash
lsblk
sudo pvdisplay
sudo vgdisplay
sudo lvdisplay
```

### Check sudo logs

```bash
sudo journalctl | grep sudo
```

---

# AI Usage

AI tools were used as a learning and debugging aid during the project.

They were used for:

- Understanding Linux commands and system administration concepts.
- Explaining configuration files and security mechanisms.
- Helping diagnose errors encountered during configuration.
- Helping understand and debug the monitoring script.
- Clarifying the differences between technologies such as Debian/Rocky Linux, AppArmor/SELinux, UFW/firewalld and VirtualBox/UTM.

AI was not used as a replacement for understanding the project. The final configuration and commands were tested and adapted manually.

---

# Author

42 student: `<login1>`
