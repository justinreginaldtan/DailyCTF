# 🧠 Shush Protocol
> *Hack The Box*  
> *Date Solved: 2025-05-01*  
> *Difficulty: Easy*
> *Category: ICS*

---

## 🧩 Overview

The goal of this challenge was to extract the password a control device uses to authenticate with a PLC inside an abandoned fertilizer plant. We were provided with a PCAP file containing captured network traffic and tasked with locating the packet containing this sensitive information.

---

## 🛠️ Tools & Techniques

    Wireshark

    Protocol Filters: tcp || udp, frame.len

    Follow TCP/UDP Stream

    Manual payload inspection (hex and ASCII)

---

## 🔍 Approach

Upon loading the .pcapng file in Wireshark, I began by identifying the most active communication pairs. I filtered the traffic down to TCP and UDP packets to remove non-essential protocols and focused on packets exchanged between key hosts, likely representing the PLC and the control system.

To pinpoint suspicious traffic, I sorted packets by length, reasoning that the most data-heavy packets would likely contain diagnostic or configuration details. One packet stood out: it originated from a common IP pair seen using an "Unknown Function" in the protocol field, but had a significantly longer payload than the rest. This anomaly suggested that it might carry more meaningful content than the standard periodic messages.

I followed the stream associated with this packet and examined the hex/ASCII data in the bottom pane of Wireshark. This revealed a readable password string embedded within the payload—transmitted in plaintext, likely due to the use of a custom or insecure protocol lacking encryption.


## 🧠 Takeaways

The password was successfully extracted from this diagnostic packet, completing the challenge. This process mirrors how professionals in ICS cybersecurity analyze anomalous protocol behavior in unencrypted industrial environments, especially when dealing with legacy systems that lack basic authentication safeguards.
