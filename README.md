# Zombie IP and TCP Idle Scan (Nmap) — Cybersecurity Research

> A research-based technical assessment of Zombie IPs and TCP Idle Scanning, prepared as part of the Cyber Security & Ethical Hacking program at Arena Web Security (Alpha 60).

## Overview

This project documents the concepts and network behavior behind **TCP Idle Scan**, also known as **Zombie Scan**, using Nmap. It explores how a third-party idle host can be used as an indirect vantage point when assessing TCP port states, and how changes in the host's IP Identification (IPID) values can help infer scan results.

The work is presented as a technical research paper and assessment. It is not a newly developed scanning tool.

## Research objectives

- Understand the role of TCP and the TCP three-way handshake in network communication.
- Explain what a Zombie IP / Idle Host is in the context of an Idle Scan.
- Study the basic packet flow between the scanner, zombie host, and target.
- Understand how IPID behavior can provide clues about port states.
- Review the Nmap Idle Scan option and its prerequisites.
- Discuss the security implications of indirect scanning and network trust relationships.

## Topics covered

1. **Network architecture and TCP** — OSI model context and TCP three-way handshake.
2. **Zombie IP / Idle Host** — the intermediary host used by the scan technique.
3. **TCP Idle Scan workflow** — the roles of the scanner, zombie host, and target.
4. **IPID behavior** — how IPID changes may be observed and interpreted.
5. **Nmap** — the `-sI` Idle Scan option and an example command.
6. **Security implications** — network exposure, filtering, trusted relationships, and defensive considerations.

## How TCP Idle Scan works — high-level summary

A traditional TCP scan sends probes directly from the scanner to the target. In an Idle Scan, the scanner uses a suitable third-party host (the *zombie*) as an indirect source of probes.

At a high level:

1. The scanner checks whether the candidate zombie host has suitable, predictable IPID behavior.
2. The scanner causes a probe to be sent toward the target with the zombie's address represented as the source.
3. The target's response can affect the zombie's IPID behavior.
4. The scanner probes the zombie again and compares the observed IPID change.
5. The difference may help infer whether the target port is open or closed.

The technique is highly dependent on network conditions and the zombie host's IPID implementation. It will not work reliably with every host or network.

## Nmap command reference

The paper discusses the following Nmap syntax:

```bash
nmap -Pn -p 80 -sI <ZOMBIE_IP> <AUTHORIZED_TARGET_IP>
```

Example using documentation-only placeholder addresses:

```bash
nmap -Pn -p 80 -sI 192.0.2.20 198.51.100.30
```

- `-Pn` — skip host discovery and treat the target as online.
- `-p 80` — scan TCP port 80.
- `-sI` — perform an Idle Scan using the specified zombie host.
- `<ZOMBIE_IP>` — IP address of a suitable, authorized idle host.
- `<AUTHORIZED_TARGET_IP>` — target system for which you have explicit permission to test.

**Important:** The option is `-sI` (lowercase letter `i`), not `-sl` (lowercase letter `L`). The addresses in the example are reserved documentation ranges, not real assessment targets.

## Requirements and limitations

- Use only systems you own or have explicit written authorization to assess.
- A suitable zombie host must be reachable and have IPID behavior that can be used by the technique.
- Modern operating systems and network devices may randomize IPID values, use per-destination counters, or otherwise make the technique unreliable.
- Firewalls, NAT, packet filtering, routing, and rate limiting can affect results.
- An Idle Scan does not guarantee anonymity or prevent attribution. Network telemetry and other evidence may still reveal activity.

## Defensive considerations

Network defenders can reduce exposure and improve detection by:

- Keeping operating systems and network devices updated.
- Applying least-privilege firewall rules and source-address validation where appropriate.
- Monitoring unusual TCP traffic patterns and unexpected probing behavior.
- Reviewing network flows and security logs for suspicious indirect scanning activity.
- Avoiding trust decisions based only on source IP addresses.

## Project material

The original research paper is written in Bangla and includes:

- TCP and OSI model illustrations.
- TCP three-way handshake explanation.
- Zombie Scan workflow diagrams.
- Nmap command reference and scan-output screenshots.
- Discussion of advantages, implications, and conclusion.

If you publish the original PDF in this repository, add it under a folder such as `docs/` and link it here:

- [Original Research Paper (Bangla)](docs/zombie-ip-and-idle-scan-bangla.pdf)

## Author

**Gopal Chandra Paul**  
Former Student — Arena Web Security, Alpha 60  
Research area: Cybersecurity, Network Scanning, Nmap

## Disclaimer

This repository is for educational and defensive cybersecurity learning. TCP Idle Scanning must only be performed in a controlled lab or against systems for which you have explicit authorization. The author does not encourage unauthorized scanning, intrusion, or attempts to evade monitoring.

## References

The original paper lists the following references for further reading:

- [Nmap — Idle Scan](https://nmap.org/book/idlescan.html)
- [Nmap Idle Scan paper / presentation](https://nmap.org/presentations/CanSecWest03/CD_Content/idlescan_paper/idlescan.html)
- [ResearchGate — IPv4 Internet Protocol header](https://www.researchgate.net/figure/Pv4-Internet-Protocol-header-48_fig1_256195912)
- [TCP Three-Way Handshake — GeeksforGeeks](https://www.geeksforgeeks.org/computer-networks/tcp-3-way-handshake-process/)
- [TCP Three-Way Handshake — Network Walks](https://networkwalks.com/tcp-3-way-handshake-process/)
