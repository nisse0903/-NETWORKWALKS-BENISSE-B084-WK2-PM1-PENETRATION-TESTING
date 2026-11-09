# -NETWORKWALKS-BENISSE-B084-WK2-PM1-PENETRATION-TESTING
This contains my works for week two. 

# Footprinting & Network Scanning Report

![Category](https://img.shields.io/badge/Category-Cybersecurity-darkred)
![Phase](https://img.shields.io/badge/Phase-Reconnaissance%20%26%20Scanning-orange)
![Tools](https://img.shields.io/badge/Tools-Kali%20Linux%20%7C%20Zenmap-blue)
![Week](https://img.shields.io/badge/Week-02-green)

> Penetration Testing Report: Footprinting & Network Scanning Phases
> **W2-PM-Final | Cybersecurity | Networkwalks**

---

## Author Information

| | |
|---|---|
| **Author** | Benisse Villasoto |
| **Role** | Pentester (Cybersecurity Intern) |
| **Internship Batch No.** | B084 – Networkwalks |
| **LinkedIn** | [linkedin.com/in/benisse-villasoto](https://www.linkedin.com/in/benisse-villasoto/) |

---

## Project Information

| | |
|---|---|
| **Project** | Cybersecurity Program at Networkwalks |
| **Week** | 02 |
| **Report Date** | November 10, 2026 |
| **Modules Completed** | W2-PM1 (Multiple Kali Tools), W2-PM5 (Zenmap Scanning) |
| **Client Targets** | Networkwalks (written permission secured) and my own local LAN network |
| **Permission Secured** | Yes |
| **Phases Covered** | Phase 1: Reconnaissance & Footprinting, Phase 2: Scanning & Network Discovery, Phase 5: Report on Findings |

### Liability Disclaimer

Everything described here was done only on equipment I own or on systems where I had **written permission in advance**. This material is for educational and research purposes only, so please make sure any use of it stays within the law. The instructor, the author, and Networkwalks are not responsible for how this knowledge is applied, and each person is accountable for their own actions. Misuse can lead to criminal charges, heavy fines, loss of employment, and a permanent criminal record. In most countries, accessing a system without authorization is a crime, even if no damage is done.

---

## Project Overview

This is my Week 2 report for the ongoing internship at Networkwalks. It covers two modules:

- **W2-PM1:** Footprinting the `networkwalks.com` domain using a range of built-in Kali Linux tools.
- **W2-PM5:** Scanning my own local network with Zenmap.

Together, the two modules show how an attacker typically works: first gathering publicly available information, then finding out which hosts on a network are live. Footprinting was done in **Kali Linux**, and scanning was done on a **Windows PC** with Zenmap installed. Each step includes the command run, the result produced, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.

### Tools Used

| Tool | Purpose |
|---|---|
| **Kali Linux & Windows** | Operating systems used for reconnaissance. Kali handled footprinting; Windows hosted scanning and local network checks. |
| **whois** | Retrieves public domain registration details (owner, registration/expiry dates, name servers). |
| **nslookup** | Queries DNS to resolve a domain name into its IP address. |
| **dnsrecon** | Enumerates DNS records (NS, MX, SPF, TXT, SRV) to map the domain's infrastructure. |
| **curl** | Sends a request to the website and displays the HTTP response headers. |
| **whatweb** | Fingerprints the technologies running on a website (web server, CMS, plugins, IP). |
| **wafw00f** | Detects whether a Web Application Firewall protects the site, and which product it is. |
| **Zenmap (Nmap GUI)** | Graphical Nmap interface used to scan the local subnet for live hosts, IPs, and MAC addresses. |
| **Windows CMD** | Used to look up the local machine's IP and subnet via `ipconfig`. |

### Key Findings & Risk Summary

| # | Finding | Tool | Risk Level |
|---|---|---|---|
| 1 | Public domain registration data retrievable (registrant is privacy-protected) | whois | Low to Medium |
| 2 | Domain resolves directly to a server name and IP address | nslookup | Medium |
| 3 | DNS records enumerated (name servers, mail server, SPF/TXT, SRV) | dnsrecon | Medium |
| 4 | HTTP headers disclose server and platform details | curl | Medium |
| 5 | Software stack fingerprinted (Apache, WordPress, jQuery, Bootstrap, WP Download Manager) | whatweb | Medium to High |
| 6 | Site detected behind ModSecurity (SpiderLabs) WAF | wafw00f | Medium |
| 7 | Local network hosts, IPs, and MAC addresses exposed by scan | Zenmap | Medium to High |

---

## Footprinting with Kali Linux Tools: Procedure

**Objective:** Footprint the live website `networkwalks.com` using six built-in Kali Linux tools, each uncovering a different aspect of the target. All commands are run in Kali's terminal emulator.

### Step 1: whois (Registration & Ownership)

```bash
whois networkwalks.com
```

Returned the registry domain ID, registrar (GoDaddy), creation and expiry dates, domain status flags, and name servers. The registrant details are masked behind a privacy service.


![whois screenshot](images/01-whois.png)

### Step 2: nslookup (Name to IP Resolution)

```bash
nslookup networkwalks.com
```

Resolved the domain to its server name and IP address (`192.232.216.135`).


![nslookup screenshot](images/02-nslookup.png)

### Step 3: dnsrecon (DNS Enumeration)

```bash
dnsrecon -d networkwalks.com
```

Enumerated SOA, NS, MX, A, TXT (including SPF), and SRV records, revealing the domain's DNS footprint and mail infrastructure.


![dnsrecon screenshot](images/03-dnsrecon.png)

### Step 4: curl (HTTP Headers)

```bash
curl -I https://networkwalks.com
```

Displayed the HTTP response headers, including the server type, cookies, and caching/security-related headers.


![curl screenshot](images/04-curl.png)

### Step 5: whatweb (Technology Fingerprinting)

```bash
whatweb networkwalks.com
```

Revealed the general software stack, including the Apache web server, WordPress with WP Download Manager (via the MetaGenerator tag), jQuery, and Bootstrap.


![whatweb screenshot](images/05-whatweb.png)

### Step 6: wafw00f (WAF Detection)

```bash
wafw00f https://networkwalks.com
```

Confirmed the site is protected by **ModSecurity (SpiderLabs) WAF**.


![wafw00f screenshot](images/06-wafw00f.png)

> Taken together, these results allow a full profile of the target to be compiled.

---

## Network Scanning with Zenmap: Procedure

### Step 1: Install Zenmap

Download Zenmap directly from the official website of its developers to ensure a legitimate installation.


![Zenmap GUI](images/07-zenmap-gui.png)

### Step 2: Get the IP address and subnet

Open Command Prompt on the Windows machine and run:

```cmd
ipconfig
```

This identifies the machine's **IP address** and **LAN subnet**.


![ipconfig screenshot](images/08-ipconfig.png)

### Step 3: Enter the target and run the scan

Put the subnet (for example, `192.***.*.*/24`) in Zenmap's **Target** field so the scan covers the entire local network instead of a single device. Run the scan and review the results:

- Total number of active hosts in the subnet
- IP address and MAC address of each host

Note: Screen capture for this is omitted for privacy.

### Step 4: Generate the network topology

Open the **Topology** tab to visually represent how the discovered devices are connected, then save it as both **PNG** and **PDF**.
Note: IP addresses are blurred for privacy.

![Zenmap topology](images/10-zenmap-topology.png)

### Step 5: Compare results

Compare the `ipconfig` output with the Zenmap results to confirm the local machine's details.

---

## Issues Identified and Solution

### Issue: `nslookup` failed with DNS communication errors

**Output received:**

```
nslookup networkwalks.com
;; communications error to 8.8.8.8#53: host unreachable
;; communications error to 8.8.8.8#53: host unreachable
;; communications error to 8.8.8.8#53: host unreachable
Server:   fd17:625c:f037:2::3
Address:  fd17:625c:f037:2::3#53
```

**Potential root cause:** Kali Linux was accidentally moved into another folder, making it inaccessible in VirtualBox. It was fixed by removing and re-adding the machine, but doing so reset the VM's network settings to default.

**Resolution:**

1. Changed Kali's network adapter in VirtualBox from **NAT** to **NAT Network** (as per the Week One instructions), and adjusted the other necessary settings.
2. Re-opened Kali Linux and ran:

```bash
ip a               # displays network interfaces and IP address
sudo dhcpcd eth0   # requests network configuration through DHCP for eth0
```

**Result:** After the reconfiguration and forced lease request, `nslookup` ran successfully and returned the expected output.

---

## Lessons Learned

- **Think like an attacker to defend better.** Looking at things from an attacker's perspective helps in understanding their possible motives and moves, which makes it easier to prevent cyberattacks, provided everything is done within legal boundaries.
- **Public information adds up.** Individually harmless details (WHOIS data, DNS records, HTTP headers, software versions) combine into a detailed profile of a target.
- **Reduce what you expose.** Domain privacy, placing the site behind a proxy, removing outdated DNS entries, hiding server version identifiers, applying standard security headers, and keeping software updated all limit what recon tools can reveal.
- **A WAF helps, but is detectable.** A web application firewall provides a front line of defense that `wafw00f` may detect but that cannot be easily bypassed.
- **Segment and monitor the network.** Dividing a LAN into smaller segments and monitoring for unexpected scans prevents a tool like Zenmap from viewing the entire environment at once. Saved topology files should be treated as sensitive.
- **Environment configuration matters.** VM network settings (NAT vs. NAT Network) directly affect tool results, so troubleshooting starts with the basics.
- **Authorization and documentation are essential.** Scanning must only be done with proper permission, and documenting each step (commands, outputs, screenshots) serves as future reference and as evidence that the work was lawful.

---

## Resources

- **Kali Linux:** [https://kali.org/get-kali](https://kali.org/get-kali)
- **Zenmap:** [https://nmap.org/zenmap/](https://nmap.org/zenmap/)

---

<p align="center">
  <b>Benisse Villasoto</b> | Batch B084 | Networkwalks Cybersecurity Internship | Week 02
</p>
