# Penetration Testing Report — Footprinting & Network Scanning Phases

**W2-PM-FINAL | Cybersecurity | Networkwalks**

| Field | Details |
|---|---|
| **Pentester Name (Cybersecurity Professional)** | Solomon Alaba |
| **Program/Batch** | B082-Networkwalks |
| **Date** | 21 August 2026 |
| **Modules completed** | W2-PM1 (Footprinting with Multiple Kali Tools v1)<br>W2-PM2 (Footprinting with GHDB v1)<br>W2-PM3 (Footprinting with Maltego v1)<br>W2-PM4 (Footprinting with theHarvester v1)<br>W2-PM5 (Network Scan with Zenmap v1) |
| **Client/Target** | 1. Networkwalks (secured written permission already)<br>2. Security camera and parent directory for mathematics textbooks exposed/public on the internet<br>3. Networkwalks.com (secured written permission already)<br>4. Microsoft.com (educational purpose only, with permission)<br>5. Personal local LAN network |
| **Permission secured from client?** | Yes |
| **Phases covered** | **Phase 1:** Reconnaissance & Footprinting<br>**Phase 2:** Google Hacking Database / Exploit Database<br>**Phase 3:** Maltego Footprinting<br>**Phase 4:** theHarvester (passive recon)<br>**Phase 5:** Scanning & Network Discovery with Zenmap |

---

## Table of Contents

1. [Liability Disclaimer](#1-liability-disclaimer)
2. [Introduction](#2-introduction)
3. [Tools Used](#3-tools-used)
4. [Activities Performed](#4-activities-performed)
5. [Network Scanning with Zenmap](#5-network-scanning-with-zenmap)
6. [Analysis / Impact](#5-analysis--impact)
7. [Recommendations](#6-recommendations)
8. [Conclusion](#7-conclusion)
9. [Evidence Collected](#8-evidence-collected)

---

## 1. Liability Disclaimer

I have performed these activities only on the systems and devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purposes only. Do not use anything from here to break the law. The instructor, the authors, and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job, and a permanent record. In most countries, unauthorised access is a crime even when nothing is damaged.

## 2. Introduction

Reconnaissance (footprinting) is the **first stage of ethical hacking**. It involves gathering publicly available information about a target before any direct interaction. This week's modules demonstrate multiple tools and approaches to footprinting and scanning, including:

- **WHOIS, WhatWeb, nslookup, curl, wafw00f, dnsrecon** (Module 1)
- **Google Hacking Database (GHDB)** (Module 2 — blocked content, not included here)
- **Maltego** (Module 3)
- **theHarvester** (Module 4)
- **Zenmap/Nmap** (Module 5)

Together, these tools provide a complete profile of the target's infrastructure, technologies, and network exposure.

### 🛠 Module 1 — Footprinting with Multiple Tools (Kali Linux)

**Tools used:** WHOIS, WhatWeb, nslookup, curl, wafw00f, dnsrecon

## 3. Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux & Windows | Operating systems used for reconnaissance/footprinting |
| WHOIS | Revealed registrar (GoDaddy), creation date (2019), expiry (2027), and HostGator name servers |
| whatweb | Fingerprinted Apache server, WordPress 7.0.4, WP Download Manager 3.3.58, Bootstrap 7.0.4, JQuery 3.7.1, and exposed email info@networkwalks.com |
| nslookup | Resolved domain to IP 192.232.216.135 |
| curl -I | Showed HTTP headers, Apache server banner, WordPress REST API endpoint `/wp-json/`, and secure HttpOnly cookies |
| wafw00f | Detected ModSecurity (SpiderLabs) WAF protection |
| dnsrecon | Enumerated DNS records including MX (mail.networkwalks.com), SPF policy, TXT verification, and SRV records pointing to cPanel mail discovery |

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

I performed reconnaissance against the `networkwalks.com` domain using six Kali Linux tools: **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon**. Each tool was used to collect a different type of information about the target.

First, I used **WHOIS** to obtain publicly available domain registration information and identify the domain's name servers. The results provided information about the domain registration and hosting infrastructure.

I then used **WhatWeb** to identify technologies used by the website. The results identified **WordPress 7.0.4** and **WP Download Manager 3.3.58**, along with other information exposed by the website.

Using **Nslookup**, I resolved the domain name to its IP address. The result identified **192.232.216.135**.

I used **Curl** with the `-I` option to inspect the HTTP response headers. This provided additional information about the web application and exposed the WordPress REST API endpoint `/wp-json/`.

Next, I used **Wafw00f** to determine whether a Web Application Firewall was protecting the website. The result identified **ModSecurity (SpiderLabs)**.

Finally, I used **DNSRecon** to enumerate DNS records. The results provided information relating to name servers, mail servers, SPF/TXT records, service records, and DNS software information.

➡️ **Findings:** Attackers can map hosting provider, CMS versions, DNS footprint, and firewall presence. Defenders must minimize exposure and patch outdated versions.

### Module 2 — Footprinting with GHDB

- GHDB (Google Hacking Database) is used to find sensitive information indexed by search engines (e.g., exposed log files, admin panels, error messages).
- The methodology involves crafting Google queries to uncover misconfigurations.

**Task 1 — Find 10 live, vulnerable security camera links exposed and accessible from the internet**

| # | Link | Relevant Dork |
|---|---|---|
| 1 | http://122.116.41.8:8080/ | `intitle:"webcamXP" inurl:8080` |
| 2 | http://219.111.32.218:8088/viewer/live/index.html | `inurl:viewash.html` |
| 3 | http://82.127.206.236/axiscgi/mjpg/video.cgi?resolution=704x480&camera=1&dummy=1620387728755 | `inurl:axis-cgi/jpg` |
| 4 | http://85.93.53.175:8080/gallery.html?page=6 | `intitle:"webcamXP 5" inurl:admin.html` |
| 5 | http://www.insecam.org/en/view/946861/ | `intitle:"Yawcam" inurl:8081` |
| 6 | http://219.111.32.218:8088/viewer/live/index.html | `inurl:viewash.html` |
| 7 | https://www.trendnet.com/emulators/TV-IP100_C1/top7599.htm?Currenttime= | `inurl:top.htm inurl:currenttime` |
| 8 | http://www.insecam.org/en/bytype/axis/ | `inurl:/view.shtml` |
| 9 | http://www.insecam.org/en/bytype/webcamxp/ | `inurl:"lvappl.htm"` |
| 10 | http://webcam.hunderdorf.de/ | `intitle:"live view" intitle:axis` |

**Task 2 — Find 10 listings containing downloadable mathematics ebooks**

| # | Link | Relevant Dork |
|---|---|---|
| 1 | https://www.skylineuniversity.ac.ae/pdf/math/ | `intitle:index.of "parent directory" mathematics pdf` |
| 2 | http://195.91.100.187:8080/index.htm?redirect=/home.htm | `inurl:home.htm intitle:1766` |
| 3 | http://182.72.188.195/cgi-bin/koha/opac-library.pl | `inurl:"cgi-bin/koha"` |
| 4 | https://assets.openstax.org/oscms-prodcms/media/documents/calculus-volume-1_-_WEB.pdf | `allintext:"mathematics textbook"` |
| 5 | https://judsonbooks.org/aata-files/aata-20260811.pdf | `inurl:"mathematics textbook"` |
| 6 | https://www.mohest.jg.gov.ng/wp-content/uploads/2024/01/NCM-SS1-TXT-FINAL.pdf | `intitle:"mathematics textbook"` |
| 7 | https://www.physics.muni.cz/~jancely/NM/Texty/Numerika/P_Olver_LectNotes/ApplMathLectNotes.pdf | `intext:"keyword"` |
| 8 | http://erewhon.superkuh.com/library/Math/ | `intitle:"index of parent directory" inurl:"mathematics pdf"` |
| 9 | https://ochicken.net/library/Mathematics/ | `allintext:"index.of parent directory" inurl:"mathematics pdf"` |
| 10 | https://ss.du.ac.in/pdf/ | `intitle:"index of parent directory mathematics"` |

**Findings:** GHDB demonstrates how attackers leverage search engines to discover hidden or misconfigured resources like cameras, textbooks, classified documents, usernames, and passwords, etc.

### Module 3 — Footprinting with Maltego

Maltego, an advanced OSINT (Open Source Intelligence) and link analysis tool, was used to harvest email addresses and other publicly available information related to the target domain `networkwalks.com`.

**Tool used:** Maltego (Community Edition)

- Installed Maltego Graph on Windows.
- Configured free Maltego ID and transforms.
- Ran domain transforms on `networkwalks.com`.
- Harvested email addresses including `info@networkwalks.com`.
- Visualized relationships between domain, DNS records, and email entities.

![Maltego graph](images/maltego-graph.png)

➡️ **Findings:** Maltego provides powerful OSINT capabilities, mapping relationships and exposing organizational emails that attackers can use for phishing.

### Module 4 — Footprinting with theHarvester

**Tool used:** theHarvester (Kali Linux)

- **Task 1:** Queried `microsoft.com` via Baidu, limit 1000 → harvested email `viva-noreply@microsoft.com`.
- **Task 2:** Queried `microsoft.com` via all sources, limit 50 → multiple API key errors but still returned emails and subdomains.

![theHarvester Task 1](images/theharvester-task1.png)
![theHarvester Task 2](images/theharvester-task2.png)

**Findings:** theHarvester collects emails, subdomains, and hosts from public sources (search engines, PGP servers, Shodan). Each harvested email is a phishing target; each subdomain expands the attack surface.

## 5. Network Scanning with Zenmap

For the second activity, I used **Zenmap** to perform network discovery on my local network. The practical required me to identify my local IP address and subnet, discover live hosts, identify their IP and MAC addresses, and generate a network topology.

I first used the Windows `ipconfig` command to identify my local IP address and LAN subnet. I then entered the subnet into Zenmap and selected **Ping Scan** to identify active hosts.

The example results provided in the practical identified twelve (12) live hosts:

```
192.168.100.1
192.168.100.5
192.168.100.6
192.168.100.8
192.168.100.11
192.168.100.18
192.168.100.60
192.168.100.93
192.168.100.146
192.168.100.178
192.168.100.201
192.168.100.180
```

The example results also included MAC addresses.

After completing the scan, I opened the **Topology** section in Zenmap, enabled the legend, and saved the network topology in PDF format as required by the practical task.

![Zenmap topology](images/zenmap-topology.png)
![Zenmap topology legend](images/zenmap-topology-legend.png)

## 5. Analysis / Impact

**Attack Perspective:** These tools provide a roadmap of the target's infrastructure, technologies, and network exposure. Emails feed phishing campaigns; outdated CMS versions invite exploits; DNS records reveal mail servers and SPF policies; network scans expose live hosts.

**Defense Perspective:** Running the same tools internally helps organizations see what attackers see, reduce leaks, patch vulnerabilities, and harden defenses.

**Key Insight:** Reconnaissance is passive, hard to detect, and foundational to every later stage of attack.

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
|---|---|---|---|---|
| 1 | Web technology information exposed | WhatWeb identified WordPress and WP Download Manager | Attackers may use exposed technology/version information to identify software requiring further security review | 🟠 Medium |
| 2 | Server IP address identifiable | Nslookup resolved the domain to `192.232.216.135` | Provides information about the network location of the web service | 🟢 Low |
| 3 | HTTP technical information exposed | Curl returned HTTP response headers and exposed `/wp-json/` | May assist technology fingerprinting and further enumeration | 🟢 Low |
| 4 | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals information about the web application's security architecture | 🟢 Low |
| 5 | DNS infrastructure information exposed | DNSRecon identified DNS, mail, and service-related records | DNS information can help build a broader infrastructure profile | 🟠 Medium |
| 6 | Multiple live hosts visible on local network | Zenmap identified four live hosts in the example network | Unknown or unauthorized devices may potentially be present on a network | 🟠 Medium |

**Risk level key:** 🔴 Critical · 🟠 Medium · 🟢 Low

The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities.

The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these two modules.

Therefore, the presence of information such as a software version, IP address, or DNS record does not by itself mean that the system is vulnerable. Further authorized security testing would be required to confirm any actual vulnerability.

## 6. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

1. **Review publicly exposed technology information** — Organizations should regularly review what information about their web technologies, CMS, and plugins is publicly visible.
2. **Keep software updated** — CMS platforms, plugins, and other web technologies should be regularly updated and reviewed against current security advisories.
3. **Review HTTP headers** — HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed.
4. **Review DNS records regularly** — DNS records should be checked periodically to ensure that only required information and services are publicly exposed.
5. **Properly configure and monitor the WAF** — Keep the WAF (ModSecurity) enabled and tuned, since it already blocks naive attacks.
6. **Perform regular internal network discovery** — Organizations should periodically scan their own networks to identify active devices.
7. **Investigate unknown devices** — Any unexpected device discovered during network scanning should be investigated and verified.
8. **Maintain network documentation** — Network topology and device information should be documented and updated regularly.
9. **Perform security testing with authorization** — Reconnaissance and scanning should only be performed against systems and networks where appropriate authorization has been provided.

## 7. Conclusion

Working through Modules 1 to 5 has given me a clear understanding of how **reconnaissance and scanning form the foundation of ethical hacking**. Each tool revealed a different angle of the target's footprint:

- **Module 1 (Kali tools)** showed me how simple commands like whois, whatweb, nslookup, curl, wafw00f, and dnsrecon can expose critical details such as registrar information, CMS versions, DNS records, and firewall presence.
- **Module 2 (GHDB)** highlighted the power of search engines in uncovering sensitive data, reminding me that misconfigurations can be exploited without any direct attack.
- **Module 3 (Maltego)** demonstrated the strength of OSINT visualization, where relationships between domains, emails, and infrastructure become clear in a graph. This made me realize how attackers can pivot from one piece of data to another.
- **Module 4 (theHarvester)** reinforced the importance of email and subdomain harvesting, showing how attackers build phishing campaigns and expand their attack surface.
- **Module 5 (Zenmap/Nmap)** gave me hands-on experience with active network scanning, teaching me how to identify live hosts, IP addresses, and MAC addresses in a subnet, and how defenders can use this to monitor unauthorized devices.

The biggest lesson I've taken away is that **footprinting is passive, powerful, and often invisible to the target**. It's the stage where attackers quietly build a roadmap, but it's also the stage where defenders can see themselves through the attacker's eyes.

Personally, I now appreciate that cybersecurity is not just about firewalls and antivirus software — it starts with awareness of what information is already exposed. By practicing these modules, I've strengthened my ability to think like both an attacker and a defender, which is the essence of ethical hacking.

## 8. Evidence Collected

![1_whois](images/1_whois.png)
![2_whatweb](images/2_whatweb.png)
![3_nslookup](images/3_nslookup.png)
![4_curl](images/4_curl.png)
![5_wafw00f](images/5_wafw00f.png)
![Zenmap topology](images/zenmap-topology.png)
![Zenmap topology evidence](images/zenmap-topology-evidence.png)

---

*-End-*

## 👤 Author

**Solomon Alaba**
Cybersecurity Professional, B082

LinkedIn: [linkedin.com/in/solomon-alaba-391b0228b](https://www.linkedin.com/in/solomon-alaba-391b0228b/)

## 📌 Project Information

**Program:** Cybersecurity program at Networkwalks | **Week:** 02 | **Repository:** GitHub
