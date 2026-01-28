*This project has been created as part of the 42 curriculum by akouiss.*

---

## Description

**Born2beRoot** is a system administration project that introduces the fundamentals of configuring and securing a Linux server from scratch.

The objective is to create a virtual machine using a minimal Linux distribution (Debian or Rocky Linux) without any graphical interface. This forces a real understanding of how a server operates at the system level.

The project focuses on core system administration concepts such as:
- User and group management
- Privilege escalation with `sudo`
- Secure SSH configuration
- Firewall setup
- Password and security policies
- Disk partitioning and encryption
- Basic monitoring and automation

Root access is restricted, and all administrative tasks must follow strict security rules. By the end of the project, the system is hardened and behaves like a properly managed server.

---

## Operating System Choice

For this project, **Debian (stable)** was chosen.

Debian is known for its stability, reliability, and extensive documentation. It provides a clean and predictable environment, making it well-suited for learning system administration fundamentals without unnecessary abstraction.

### Debian (stable)

**Pros**
- Extremely stable due to conservative package updates  
- Clear and well-maintained documentation  
- Simple and transparent system configuration  
- Uses **AppArmor**, which is easier to understand and configure  
- Ideal for learning core concepts such as users, permissions, services, SSH, and firewall rules  

**Cons**
- Older package versions compared to rolling or enterprise distributions  
- Less exposure to enterprise-grade security tools  
- Not commonly used in large corporate infrastructures compared to RHEL-based systems  

### Rocky Linux

Rocky Linux is a community-driven rebuild of Red Hat Enterprise Linux (RHEL) and is primarily designed for enterprise environments.

**Pros**
- Enterprise-oriented and widely used in professional infrastructures  
- Strong security model based on **SELinux**  
- Long-term support and predictable lifecycle  
- Closer to real-world production server environments  

**Cons**
- SELinux adds significant complexity for beginners  
- Security policies can obscure system behavior  
- Steeper learning curve for basic administration tasks  
- Less suitable for focusing on fundamentals without additional overhead  

### Conclusion

Debian was chosen because it allows focusing on **learning and understanding system administration fundamentals** rather than managing enterprise-level complexity. While Rocky Linux is more representative of production servers, Debian provides a clearer and more accessible environment aligned with the educational goals of this project.


## Disk Partitioning

The disk is **encrypted** and managed using **LVM (Logical Volume Manager)**.

Only the **partitioning bonus** was completed. No additional services were added.

### Partition Layout

/boot (standard partition, unencrypted)
/ (LVM, encrypted)
/home (LVM, encrypted)
/var (LVM, encrypted)
/var/log (LVM, encrypted)
/tmp (LVM, encrypted)
swap (LVM, encrypted)


### Explanation

- **LVM** allows flexible disk management and resizing without reinstalling the system.
- **Encryption** protects data in case the virtual disk is copied or accessed without authorization.
- `/` is kept minimal and contains only essential system files.
- `/home` is separated to isolate user data.
- `/var` is separated because it can grow quickly due to logs and package data.
- `/var/log` is isolated to prevent log files from filling the system.
- `/tmp` is separated to avoid temporary files impacting system stability.

Partition sizes were chosen to be simple and sufficient for the project requirements.

---

## SSH Configuration

- SSH runs on port **4242**
- Root login via SSH is disabled
- Only non-root users are allowed to connect

This setup improves security and complies with the subject requirements.

---

## Firewall

- **UFW** is enabled at startup
- Only port **4242** is allowed
- All other incoming connections are blocked by default

---

## Users and Groups

- A non-root user with my login exists
- The user belongs to the `user42` and `sudo` groups

During evaluation, new users and groups can be created and managed correctly.

---

## Sudo Configuration

The sudo configuration strictly follows the subject rules:

- Maximum of **3 authentication attempts**
- Custom error message on incorrect password
- All sudo input/output is logged in `/var/log/sudo/`
- TTY mode is enabled
- Restricted executable paths are enforced

---

## Password Policy

The password policy enforces the following rules:

- Password expires every **30 days**
- Minimum length: **10 characters**
- Must contain:
  - at least one uppercase letter
  - at least one lowercase letter
  - at least one number
- Must not contain the username
- No more than **3 identical characters** in a row
- Warning issued **7 days** before expiration

The root account follows the same policy.

---

## Monitoring Script

A Bash script named `monitoring.sh` is executed every **10 minutes** using `cron`.

The script displays system information on all terminals using `wall`.

Displayed information:
- Operating system and kernel version
- Number of physical and virtual CPUs
- RAM usage
- Disk usage
- CPU load
- Last reboot time
- LVM status
- Active TCP connections
- Number of logged-in users
- Network information (IP and MAC address)
- Number of executed sudo commands

The script runs without errors and can be stopped without modification.

---

## Usage

1. Start the virtual machine using **VirtualBox**
2. Log in with the non-root user
3. SSH access is available on port `4242`

---

## Comparisons

### Debian vs Rocky Linux

**Rocky Linux**
- Enterprise-oriented
- Heavy use of SELinux
- Strong security features
- No rolling upgrade path

**Debian**
- Community-maintained
- Large software repository
- Easier installation and configuration
- Better suited for learning system administration

---

### AppArmor vs SELinux

- **Access Control Model**
  - AppArmor: path-based
  - SELinux: label-based

- **Complexity**
  - AppArmor is easier to learn and debug
  - SELinux has a steep learning curve

- **Granularity**
  - SELinux provides deeper, fine-grained control
  - AppArmor offers sufficient control for most use cases

- **Performance**
  - AppArmor has lower overhead
  - SELinux is optimized for enterprise environments

- **Default Usage**
  - AppArmor: Debian, Ubuntu, SUSE
  - SELinux: RHEL, CentOS, Fedora

---

### UFW vs firewalld

**firewalld**
- Dynamic rule management
- Supports zones and services
- Common on Red Hat-based systems

**UFW**
- Simple interface for iptables
- Lightweight and easy to use
- Ideal for small setups
- Default on Debian/Ubuntu systems

---

### VirtualBox vs UTM

VirtualBox and UTM target different users and hardware.

- **VirtualBox**
  - More powerful and feature-rich
  - Better Linux support
  - Poor performance on Apple silicon

- **UTM**
  - Optimized for Apple silicon
  - Simpler and faster on M-series Macs
  - Limited advanced features

On Intel machines, VirtualBox is the clear choice.  
On Apple silicon, UTM is easier but less capable.

---

## Resources

- Debian Administrator’s Handbook  
  https://debian-handbook.info/browse/stable/

- Linux Bible (Christopher Negus)  
  https://edu.anarcho-copy.org/Against%20Security%20-%20Self%20Security/linux-bible-christopher-negus-10th.pdf

---

## AI Usage

AI was **not** used to generate configurations, scripts, or system logic.

All system setup, commands, and testing were done manually.

The only indirect AI involvement was through Google Search result summaries that occasionally appear at the top of search results.
