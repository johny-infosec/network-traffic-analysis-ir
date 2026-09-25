CYBER SECURITY INCIDENT RESPONSE REPORT
1. Incident Overview
Incident Title: Simulated Outbound Malicious Beaconing & C2 Traffic Investigation

Date/Time Detected: September 25, 2026

Reported By: Johny Figueroa (Security Operations / Analyst)

Incident Handler: Johny Figueroa

Severity Level: High

Status: Resolved & Contained

2. Executive Summary
A simulated security incident involving unauthorized outbound HTTP beaconing was detected from a local Linux workstation environment. Threat actor behavior was replicated using custom user-agent strings and HTTP curl requests. Packet capture analysis via Wireshark successfully identified the anomalous outbound traffic patterns targeting external IOCs. Immediate host-level containment was executed using kernel-level firewall rules (iptables), successfully neutralizing the outbound communication channel.

3. Incident Details
Type of Incident: Simulated Command & Control (C2) / Unauthorized Outbound Beaconing

Affected Systems: Local Linux Development / Security Workstation (WSL/Kali Environment)

Affected Users: flying-the-black

Indicators of Compromise (IOCs):

Suspicious IPs: 100.31.16.17

File hashes: N/A (Simulation artifact)

Domains/URLs: Outbound HTTP endpoints mapped via target IP

Processes: curl (utilized for simulation)

Registry changes: N/A (Linux-based host)

4. Timeline of Events
T+00:00: Threat simulation initiated; custom headers and beaconing user-agents executed via curl.

T+00:05: Packet capture initiated to monitor network interfaces.

T+00:12: Wireshark filter http.request.method == "GET" applied, isolating outbound HTTP traffic to external target.

T+00:18: Indicator of Compromise identified (100.31.16.17).

T+00:25: Containment executed via iptables rule injection.

5. Detection & Analysis
Detection Method: Manual packet capture review and targeted display filtering.

Tools Used: Wireshark, curl, Linux CLI, iptables.

Findings:

Observed cleartext HTTP GET requests traversing the network interface toward the external IOC.

Wireshark filter isolation confirmed the precise destination IP and request headers.

6. Containment Actions
Immediate Containment:

Deployed a high-priority blocking rule at the top of the kernel firewall OUTPUT chain to immediately drop all traffic headed to the threat IP:

Bash
sudo iptables -I OUTPUT 1 -d 100.31.16.17 -j DROP
Short‑Term Containment: Verified rule enforcement using firewall status queries to ensure zero packet leakage.

7. Eradication & Recovery
Eradication Steps: Terminated simulation scripts and ensured no persistence mechanisms or cron jobs were left behind.

Recovery Steps:

Validated normal network stack functionality for legitimate services.

Confirmed subsequent outbound connection attempts to 100.31.16.17 were successfully dropped by the firewall.

8. Root Cause Analysis (RCA)
Root Cause: Controlled simulation of outbound beaconing to test incident response workflows.

How the attacker gained access: N/A (Intentional simulation payload via curl).

Why controls failed or succeeded: Manual rule insertion succeeded instantly due to root-level host access and correct iptables syntax placement (-I OUTPUT 1).

9. Impact Assessment
Data Impact: None (Simulated exercise; no production data exposed).

Operational Impact: None (Contained to local lab environment).

Financial Impact: $0.00

Regulatory Impact: None.

10. Lessons Learned
What worked well: Wireshark display filters (http.request.method == "GET") quickly isolated the exact HTTP request, and iptables provided instant, reliable host-level blocking.

What needs improvement: Automating detection alerts via script or log monitors rather than manual packet inspection.

Gaps in detection: Absence of automated SIEM/IDS alerts for anomalous outbound traffic in the baseline environment.

11. Preventive Measures
Policy updates: Establish stricter outbound firewall default-deny policies for sensitive environments.

Additional monitoring: Integrate persistent packet logging or Snort/Suricata rules for known C2 patterns.

Hardening steps: Regularly audit active outbound connections and review firewall rule chains.

12. Final Status
Incident Closed On: September 25, 2026

Verified By: Johny Figueroa

Follow‑Up Actions Scheduled: Document script steps into the main GitHub repository README.md.
