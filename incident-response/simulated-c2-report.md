CYBER SECURITY INCIDENT RESPONSE REPORT
1. Incident Overview
Simulating and Blocking Outbound C2 Traffic in Linux
Analyst: Johny Figueroa

Environment: Linux / Kali Local Lab

Status: Completed & Verified

Overview & Objective
For this mini-project, I wanted to walk through a complete end-to-end Incident Response (IR) cycle in a Linux environment. Instead of just reading about threat hunting, I simulated an attacker executing outbound beaconing, tracked the traffic down using Wireshark, and then used kernel-level firewall rules to cut off the connection.

Step 1: Simulating the Threat (curl)
To get things moving, I used curl to generate custom outbound HTTP requests with modified headers and user-agent strings. This mimics how a basic command-and-control (C2) beacon or script would check in with an external server.

Step 2: Triangulating the Traffic (Wireshark)
Once the traffic was moving, I fired up packet capture and dug into the logs to find the indicator of compromise (IOC).

The Filter: Using http.request.method == "GET" let me instantly strip away background noise and isolate the exact outbound request.

The Finding: Caught the host reaching out to the target external IP (100.31.16.17) clear as day in the packet details.

Step 3: Containment & Remediation (iptables)
Finding the bad IP is only half the battle—next, I needed to shut down the communication channel immediately.

Instead of just appending a rule to the bottom, I used iptables to inject a high-priority drop rule directly at the top of the output chain so it would take effect instantly:

Bash
sudo iptables -I OUTPUT 1 -d 100.31.16.17 -j DROP
After dropping the rule, I tested it out and verified via packet logs that any further attempts to talk to that IP were successfully blocked cold by the kernel.

Key Takeaways & Lessons Learned
What clicked: Wireshark filters made isolating the HTTP traffic super fast, and iptables gave me total, instant control right at the host level without needing a bulky third-party tool.

Room for growth: Doing this all manually via CLI is great for understanding the fundamentals, but in a real production setup, you'd want automated alerting (like Snort or a basic SIEM rule) to catch those outbound spikes before you have to hunt them down by hand.
