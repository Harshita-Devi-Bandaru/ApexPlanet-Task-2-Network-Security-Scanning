# Reconnaissance

Reconnaissance is the first step in a security assessment. It is used to collect information about a target before starting detailed scanning and testing.

There are two main types of reconnaissance:

- Passive Reconnaissance
- Active Reconnaissance

## Passive Reconnaissance

Passive reconnaissance means collecting information about a target without directly interacting with the target system.

### Whois

Whois is used to find publicly available information about a domain. It can provide details such as the registrar, registration information, domain status, and name servers.

Example:

```bash
whois example.com
```

### Nslookup

Nslookup is used to query DNS information. It can be used to find the IP address associated with a domain and other DNS-related information.

Example:

```bash
nslookup example.com
```

### Google Dorking

Google Dorking uses advanced search operators to find specific information that is already publicly indexed by search engines.

Some commonly used operators are:

```text
site:
filetype:
intitle:
inurl:
```

Example:

```text
site:example.com filetype:pdf
```

It can be useful for security research and identifying information that may be publicly exposed.

### Shodan

Shodan is a search engine for internet-connected devices and services. It can provide information such as IP addresses, open ports, service details, and server banners.

Shodan can be useful for understanding the public exposure of systems and services.

## Active Reconnaissance

Active reconnaissance involves directly interacting with the target system to collect information.

For this task, I performed active reconnaissance against my Metasploitable2 virtual machine in an isolated lab environment.

### Ping Sweep

A ping sweep is used to identify active hosts on a network.

I used Nmap to check the lab network:

```bash
nmap -sn 192.168.50.0/24
```

The scan identified 5 active hosts on the network. My Kali Linux machine was using `192.168.50.139` and the Metasploitable2 machine was using `192.168.50.140`.

![Ping Sweep]<img width="280" height="130" alt="Ping Sweep ss" src="https://github.com/user-attachments/assets/c8b7bd59-8dd9-4b36-8211-ce2e67d2bdd5" />


### Banner Grabbing

Banner grabbing is used to identify information about services running on a target system.

I checked the FTP service running on the Metasploitable2 machine using Netcat:

```bash
nc -v 192.168.50.140 21
```

The service returned the following banner:

```text
220 (vsFTPd 2.3.4)
```

This showed that FTP was running on port 21 and that the server identified itself as vsFTPd 2.3.4.

![FTP Banner]<img width="223" height="50" alt="FTK banner ss" src="https://github.com/user-attachments/assets/1ded10c9-fa04-497e-b5af-0ea73768e11c" />

)

## Summary

In this reconnaissance phase, I learned how information can be collected before performing detailed security testing.

Passive reconnaissance focuses on collecting publicly available information, while active reconnaissance involves directly interacting with the target system.

In my lab environment, the ping sweep helped identify active hosts on the network, and banner grabbing helped identify the FTP service running on the Metasploitable2 machine.

The information collected during reconnaissance will be useful for the next phase, where I will perform port and service scanning using Nmap.
