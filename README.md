# Network Traffic Analysis & Incident Response Simulation

A hands-on cybersecurity portfolio project demonstrating traffic inspection, OSINT reconnaissance, threat containment, and firewall remediation within a Linux environment.

---

## Project Overview
This project simulates an investigation into suspicious outbound network activity. Using command-line utilities, packet capture tools, and kernel-level firewall controls, this exercise walks through the complete Incident Response (IR) lifecycle: identifying anomalous traffic, analyzing packet structures, gathering OSINT intel, executing containment measures, and verifying remediation effectiveness.

---

## 1. Threat Simulation & Custom Headers
Before analyzing traffic, custom threat actor headers and user-agent strings were simulated using `curl` to mimic malicious beaconing behavior[cite: 1].

<img width="995" height="306" alt="Screenshot 2026-09-23 084217" src="https://github.com/user-attachments/assets/2268aa54-764c-407a-8672-40b26f814dba" />

---

## 2. Traffic Capture & Wireshark Analysis
Packet captures were reviewed using targeted display filters to isolate anomalous HTTP traffic and identify indicators of compromise (IOCs).

* **Key Display Filters Used:**
  * Filtering by HTTP GET requests: `http.request.method == "GET"`[cite: 3]
* **Observation:** The host successfully established outbound HTTP connections to suspicious target IPs[cite: 3].


<img width="1352" height="227" alt="using wireshark command request method get  for IOCS header" src="https://github.com/user-attachments/assets/601432e5-e899-40fb-9ae9-7ee1e4c16258" />

---

## 3. Perimeter Containment & Firewall Remediation
To neutralize the threat, host-level network traffic to the malicious IP address was blocked using Linux `iptables`. The rule was inserted at the top of the `OUTPUT` chain to ensure immediate priority:

```bash
sudo iptables -I OUTPUT 1 -d 100.31.16.17 -j DROP
```
<img width="877" height="305" alt="send packet request and verify  drop firewall" src="https://github.com/user-attachments/assets/1335b0f0-b4f7-431e-aa19-341abaaf2a9a" />
