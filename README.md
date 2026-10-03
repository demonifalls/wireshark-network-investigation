# Wireshark Network Investigation

## Project Overview

This project demonstrates a simulated network traffic investigation using Wireshark concepts and display filters to analyze potentially suspicious activity. The investigation focused on identifying unusual communication patterns involving SMB traffic, authentication activity, executable distribution, and outbound network connections.

The purpose of the investigation was to determine whether the observed activity could indicate lateral movement or automated command-and-control behavior and whether the incident required escalation.

## Scenario

During the simulated investigation, I identified unusual activity involving a source host communicating with multiple internal systems.

The same user account successfully authenticated to several internal systems within seconds of one another. The source host also generated multiple SMB connections to internal systems over TCP port 445.

The timing and repetitive nature of the activity warranted further investigation.

## Investigation & Findings

### 1. Suspicious SMB Activity

The source IP `192.168.1.25` communicated with multiple internal destination IP addresses over SMB port 445 at approximately two-second intervals.

SMB is commonly used in Windows environments for legitimate file and resource sharing, so the presence of SMB traffic alone was not considered malicious.

However, the rapid and repetitive communication pattern across multiple internal systems was unusual and required additional investigation to determine whether the activity could be associated with lateral movement.

### 2. Rapid Successful Authentication

Further investigation identified successful authentication activity involving the same user account, `jsmith`, across multiple internal hosts within approximately eight seconds.

Successful authentication alone would not necessarily indicate malicious activity. However, the speed and repetitive pattern were inconsistent with typical manual user behavior.

This increased concern that the account credentials could potentially be compromised or being used by an automated process.

### 3. Suspicious Executable Distribution

The investigation identified `security_update.exe` being copied to the same hosts involved in the earlier SMB and authentication activity.

This increased the severity of the investigation because the executable distribution correlated with the rapid SMB connections and successful authentications across the same systems.

The correlation between these events raised concern for possible lateral movement and indicated that the executable should be investigated further.

### 4. Coordinated External Communication

The same hosts that received `security_update.exe` subsequently began communicating with the external IP address `198.51.100.80` over HTTPS port 443 at approximately two-second intervals.

HTTPS traffic over port 443 is common and is not inherently malicious. However, the timing and correlation between the executable distribution and the subsequent outbound connections increased suspicion.

The recurring communication pattern could represent automated network activity or possible command-and-control behavior. Additional investigation would be necessary to determine the actual purpose and contents of the communication.

## Example Wireshark Display Filters

The investigation incorporated Wireshark filtering concepts for isolating relevant network traffic.

### Filter Traffic Involving a Specific IP

```text
ip.addr == 192.168.1.25
```

### Filter Traffic Originating From a Specific Host

```text
ip.src == 192.168.1.25
```

### Filter Traffic Sent to a Specific Destination

```text
ip.dst == 198.51.100.80
```

### Filter SMB Traffic

```text
tcp.port == 445
```

### Filter HTTPS Traffic

```text
tcp.port == 443
```

### Filter SMB Traffic Involving the Source Host

```text
ip.addr == 192.168.1.25 && tcp.port == 445
```

### Filter DNS Traffic

```text
dns
```

These filters demonstrate how network traffic can be narrowed to specific hosts, ports, and protocols during an investigation.

## Analyst Assessment

Based on the correlated evidence, I would escalate the incident to Tier 2 for additional investigation.

The escalation would be based on the combination of:

- Rapid SMB communication across multiple internal systems
- Successful authentication of the same account across multiple hosts
- Distribution of `security_update.exe` to those systems
- Subsequent recurring HTTPS communication with the same external IP address

Together, these events are consistent with possible lateral movement and automated external communication.

However, the available evidence does not definitively establish that lateral movement or command-and-control activity occurred. Additional endpoint, authentication, process, and network evidence would be necessary to confirm the nature of the activity.

As an initial containment measure, I would recommend isolating affected systems from the network while preserving evidence for further investigation.

## Skills Demonstrated

- Network traffic analysis fundamentals
- Wireshark display-filter syntax
- IP address analysis using `ip.src`, `ip.dst`, and `ip.addr`
- TCP port filtering using `tcp.srcport`, `tcp.dstport`, and `tcp.port`
- SMB traffic analysis over TCP port 445
- HTTPS traffic analysis over TCP port 443
- DNS traffic analysis
- TCP stream analysis concepts
- Network communication pattern analysis
- Identification of possible automated beaconing
- Investigation of possible lateral movement
- Correlation of network, authentication, and endpoint activity
- Incident timeline development
- Differentiating suspicious activity from confirmed malicious activity
- Incident escalation and containment decision-making

## Conclusion

This simulated investigation demonstrates how network traffic can be analyzed to identify unusual communication patterns and determine whether additional investigation or escalation is necessary.

By correlating IP addresses, ports, protocols, authentication activity, executable distribution, and recurring network connections, I identified evidence consistent with possible lateral movement and automated external communication.

The investigation also demonstrates the importance of distinguishing suspicious indicators from confirmed malicious activity and gathering additional evidence before reaching a definitive conclusion.

## Disclaimer

This project is an educational simulation designed to demonstrate network traffic analysis, Wireshark filtering concepts, and Tier-1 SOC investigation methodology.

The network traffic, IP addresses, usernames, hosts, executable names, and incident scenario are simulated and do not represent a real production security incident.