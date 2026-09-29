# Wireshark Network Investigation

## Project Overview

For this project, I used Wireshark to analyze network traffic for possible malicious behavior. After identifying unusual activity between several hosts, I conducted a traffic analysis using IP addresses, ports, protocols, DNS activity, and communication patterns. The investigation revealed suspicious behavior, including recurring automated connections and unusual communication between internal and external systems.

## Scenario

During the initial review, I noticed that the same user account successfully authenticated to several internal systems within seconds of one another. The source host was also making an unusual number of SMB connections to multiple internal systems over port 445. The speed and pattern of this activity appeared unusual and warranted further investigation.

## Investigation & Findings

### 1. Suspicious SMB Activity

I observed the source IP `192.168.1.25` communicating with multiple internal destination IPs over SMB port 445 at approximately two-second intervals. The rapid and repetitive pattern appeared unusual and required further investigation to determine whether the activity was legitimate or potentially associated with lateral movement.

### 2. Rapid Successful Authentication

Upon further investigation, my concern increased after discovering that the same user account, `jsmith`, successfully authenticated to multiple internal hosts within approximately eight seconds. Successful authentication alone would not necessarily be suspicious, but the speed and repetitive pattern were inconsistent with typical manual user activity. This raised concern that the account credentials could be compromised or being used by an automated process.

### 3. Suspicious Executable Distribution

Upon further investigation, I discovered that `security_update.exe` was being copied to each host that the `jsmith` account had rapidly authenticated to. This increased my suspicion because the executable distribution correlated with the earlier rapid SMB connections and successful authentications across the same systems. Together, these events raised concern for possible lateral movement and warranted further investigation of the executable.

### 4. Coordinated External Communication

Upon further investigation, my suspicion increased after discovering that the same hosts that received `security_update.exe` began communicating with the same external IP, `198.51.100.80`, over HTTPS port 443 at approximately two-second intervals. The timing and correlation between the executable distribution and subsequent outbound connections raised concern that the activity could represent command-and-control communication or another form of automated network activity. Further investigation would be required to determine the purpose and contents of the communication.

## Analyst Assessment

Based on my observations, I would escalate this incident to Tier 2 due to indicators of possible lateral movement and command-and-control or beaconing activity. My assessment is supported by the rapid SMB connections, successful authentication of the `jsmith` account across multiple hosts, distribution of `security_update.exe` to those same systems, and subsequent HTTPS communication from the affected hosts to the same external IP. I would recommend isolating the affected hosts to limit additional communication or potential spread while `security_update.exe` and the external network activity are investigated further.

## Wireshark Skills Demonstrated

- Analyzed network traffic using Wireshark to identify unusual communication patterns.
- Filtered traffic by source, destination, and IP address using `ip.src`, `ip.dst`, and `ip.addr`.
- Filtered TCP traffic using `tcp.srcport`, `tcp.dstport`, and `tcp.port`.
- Combined display filters using `&&`, `||`, and parentheses.
- Analyzed DNS traffic and correlated domain lookups with subsequent network connections.
- Followed TCP streams to examine communication between hosts.
- Identified recurring network patterns that could indicate automated beaconing.
- Analyzed SMB traffic over port 445 for indicators of possible lateral movement.
- Correlated network traffic with authentication and endpoint activity to develop an incident timeline.
- Distinguished suspicious activity from confirmed malicious activity and identified when further investigation or escalation was necessary.

## Conclusion

In this project, I demonstrated how I identified unusual network traffic and investigated the activity to determine whether escalation was necessary. I used Wireshark to analyze traffic to and from IP addresses, identify the ports and protocols being used, and correlate communication patterns across multiple hosts. By combining these findings, I was able to identify indicators of possible lateral movement and command-and-control activity while recognizing that additional investigation would be required to confirm malicious behavior.