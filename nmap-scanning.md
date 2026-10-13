# Nmap Port & Service Scanning

This section covers port and service scanning of the Metasploitable2 virtual machine using Nmap.

The purpose of this activity was to identify open TCP and UDP ports, determine the services running on those ports, detect the target operating system, and document the scan results.

## Target Information

| Item | Details |
|---|---|
| Scanner | Kali Linux |
| Target | Metasploitable2 |
| Target IP | `192.168.50.140` |
| Kali IP | `192.168.50.139` |
| Network | `192.168.50.0/24` |
| Environment | Isolated VMware Host-Only lab |
| Tool | Nmap |

---

## 1. TCP SYN Scan

A TCP SYN scan is used to identify open TCP ports on a target system.

I performed the scan against the Metasploitable2 machine using:

```bash
sudo nmap -sS 192.168.50.140
```

The scan identified several open TCP ports and services.

### TCP Scan Results

| Port | State | Service |
|---|---|---|
| 21/tcp | open | ftp |
| 22/tcp | open | ssh |
| 23/tcp | open | telnet |
| 25/tcp | open | smtp |
| 53/tcp | open | domain |
| 80/tcp | open | http |
| 111/tcp | open | rpcbind |
| 139/tcp | open | netbios-ssn |
| 445/tcp | open | microsoft-ds |
| 512/tcp | open | exec |
| 513/tcp | open | login |
| 514/tcp | open | shell |
| 1099/tcp | open | rmiregistry |
| 1524/tcp | open | ingreslock |
| 2049/tcp | open | nfs |
| 2121/tcp | open | ccproxy-ftp |
| 3306/tcp | open | mysql |
| 5432/tcp | open | postgresql |
| 5900/tcp | open | vnc |
| 6000/tcp | open | X11 |
| 6667/tcp | open | irc |
| 8009/tcp | open | ajp13 |
| 8180/tcp | open | unknown |

The large number of open ports shows that the Metasploitable2 system exposes many network services.

<img width="240" height="197" alt="image" src="https://github.com/user-attachments/assets/3e7a11fb-e35b-4041-9786-98f30e6dbbb3" />


---

## 2. UDP Scan

UDP scanning was performed to identify UDP services running on the target.

I used:

```bash
sudo nmap -sU 192.168.50.140
```

The UDP scan took longer to complete because UDP scanning generally requires more time to determine the state of ports.

### UDP Scan Results

| Port | State | Service |
|---|---|---|
| 53/udp | open | domain |
| 68/udp | open\|filtered | dhcp |
| 69/udp | open\|filtered | tftp |
| 111/udp | open\|filtered | rpcbind |
| 137/udp | open | netbios-ns |
| 138/udp | open\|filtered | netbios-dgm |
| 643/udp | open\|filtered | sanity |
| 2049/udp | open | nfs |
| 36778/udp | open\|filtered | unknown |

The scan also reported 991 closed UDP ports.

The `open|filtered` state means that Nmap could not clearly determine whether the port was open or being filtered because there was no definite response.

<img width="212" height="118" alt="image" src="https://github.com/user-attachments/assets/f598334a-cc51-42be-970d-556358241d1a" />


---

## 3. Service Version Detection

After identifying the open ports, I used Nmap service version detection to gather more information about the services.

Command used:

```bash
sudo nmap -sV 192.168.50.140
```

The `-sV` option attempts to identify the service and its version running on an open port.

For example, during the earlier reconnaissance phase, the FTP service on port 21 identified itself as:

```text
220 (vsFTPd 2.3.4)
```

This confirms that FTP was running on port 21 and that the server banner identified the software as vsFTPd 2.3.4.

The complete service-version scan was saved as:

```text
service-version-scan.txt
```

<img width="470" height="213" alt="image" src="https://github.com/user-attachments/assets/a135b120-2fc8-446e-8eab-54a1d4952d1e" />


---

## 4. OS Detection

I also performed operating system detection against the target.

Command used:

```bash
sudo nmap -O 192.168.50.140
```

The scan identified the target as a general-purpose Linux system.

### OS Detection Results

- Device type: General purpose
- Running: Linux 2.6.X
- OS CPE: `cpe:/o:linux:linux_kernel:2.6`
- OS details: Linux 2.6.9 - 2.6.33
- Network distance: 1 hop

Nmap also reported that OS detection was performed successfully.

<img width="382" height="261" alt="image" src="https://github.com/user-attachments/assets/31d760d4-ce7e-4fe8-ac8c-d1b6ba92ca78" />


---

## 5. Saving the Scan Results

I saved the different Nmap scan results as text files for documentation and further analysis.

### TCP Scan

```bash
sudo nmap -sS 192.168.50.140 -oN tcp-scan.txt
```

### UDP Scan

```bash
sudo nmap -sU 192.168.50.140 -oN udp-scan.txt
```

### Service Version Detection

```bash
sudo nmap -sV 192.168.50.140 -oN service-version-scan.txt
```

### OS Detection

```bash
sudo nmap -O 192.168.50.140 -oN os-detection.txt
```

The files created were:

```text
tcp-scan.txt
udp-scan.txt
service-version-scan.txt
os-detection.txt
```

---

## 6. Scan Analysis

The Nmap scans showed that the Metasploitable2 machine has many network services exposed.

The TCP scan found services such as FTP, SSH, Telnet, HTTP, SMB-related services, MySQL, PostgreSQL, VNC, IRC, NFS, and other services.

The UDP scan identified services including DNS, NetBIOS, NFS, and other UDP services.

The service version scan was used to gather additional information about the software running on the discovered ports.

The OS detection scan identified the target as an older Linux 2.6-based system.

From a security perspective, having a large number of exposed services increases the attack surface of a system. In a real environment, unnecessary services should be disabled or restricted and exposed services should be kept updated and properly secured.

Since Metasploitable2 is intentionally vulnerable, these results are expected and the machine is being used only as a controlled target in the isolated lab environment.

---

## 7. Commands Used

```bash
# TCP SYN scan
sudo nmap -sS 192.168.50.140

# UDP scan
sudo nmap -sU 192.168.50.140

# Service version detection
sudo nmap -sV 192.168.50.140

# OS detection
sudo nmap -O 192.168.50.140

# Save TCP results
sudo nmap -sS 192.168.50.140 -oN tcp-scan.txt

# Save UDP results
sudo nmap -sU 192.168.50.140 -oN udp-scan.txt

# Save service version results
sudo nmap -sV 192.168.50.140 -oN service-version-scan.txt

# Save OS detection results
sudo nmap -O 192.168.50.140 -oN os-detection.txt
```
<img width="550" height="41" alt="image" src="https://github.com/user-attachments/assets/6e402355-8c22-4157-b3e6-45dca694c541" />

---

## Conclusion

In this phase, I used Nmap to perform TCP SYN scanning, UDP scanning, service version detection, and OS detection against the Metasploitable2 virtual machine.

The scans helped me understand which ports and services were exposed on the target and provided information about the operating system.

The results will be useful for the next stage of the task, where the target will be checked for vulnerabilities using a vulnerability scanner.

## Lab Safety

All scans in this activity were performed against my own Metasploitable2 virtual machine inside an isolated VMware lab environment. No unauthorized systems were targeted.
