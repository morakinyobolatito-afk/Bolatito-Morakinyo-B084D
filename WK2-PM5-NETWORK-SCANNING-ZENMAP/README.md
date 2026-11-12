# W2-PM5: Network Scanning with Zenmap

**Program:** Networkwalks Cybersecurity & Ethical Hacking | **Batch:** B084D | **Week:** 2
**Author:** Morakinyo | **Date:** 12 November 2026 | **Environment:** Windows PC with Zenmap (Nmap GUI)

> **For educational purposes only.** This scan was run only on my own local lab network. Only scan networks you own or have written permission to test.

## 1. Objective

Use Zenmap to discover live hosts on the local lab subnet, record their IP and MAC addresses, and display and save the network topology as a PDF.

## 2. Tools Used

| Tool | Purpose |
|---|---|
| Zenmap (Nmap GUI) | Run the ping scan, view hosts and draw the topology |
| Windows `ipconfig` | Identify the local IP address, subnet and MAC address |

## 3. Tasks and Results

### Task 1: Install Zenmap

Zenmap was installed on my Windows PC from the official Nmap download page (https://nmap.org/download.html).

![Zenmap installed](screenshots/01-zenmap-installed.jpg)

### Task 2: Find the local IP address and LAN subnet

Ran `ipconfig` in Command Prompt to identify the local IP address and the subnet used for the scan.

![ipconfig output](screenshots/02-ipconfig.jpg)

| Item | Result |
|---|---|
| Subnet scanned | `192.168.56.1/24` (the 192.168.56.0 network) |

### Task 3: Find the live hosts in the subnet

In Zenmap, I entered the subnet as the target and selected the **Ping scan** profile.

**Command:** `nmap -sn 192.168.56.1/24`

![Nmap output](screenshots/03-nmap-output.jpg)

| Item | Result |
|---|---|
| Scan date | 12 November 2026 |
| IP addresses scanned | 256 |
| Live hosts | 1 |
| Scan time | 11.97 seconds |

An earlier scan of the single address `192.168.56.1` (`nmap -sn 192.168.56.1`) also reported the host as up, in 1.06 seconds.

### Task 4: How many hosts are live?

**Answer:** 1 host is live.

### Task 5: IP addresses of the live hosts

**Answer:** `192.168.56.1`

### Task 6: MAC addresses of the live hosts

**Answer:** Nmap did not report a MAC address for the live host (Host Details shows MAC as "Not available"). A ping scan only reports a MAC address for hosts it discovers on the same Ethernet segment. A computer's own MAC address can be read with `ipconfig /all`.

![Host details](screenshots/04-host-details.jpg)

| Host Details field | Value |
|---|---|
| State | up |
| Open / filtered / closed ports | 0 / 0 / 0 (a ping scan does not scan ports) |
| IPv4 | 192.168.56.1 |
| IPv6 | Not available |
| MAC | Not available |

### Task 7: Display and save the topology as PDF

Opened the **Topology** tab, turned on the legend, and used **Save Graphic** to export the topology as a PDF.

![Network topology](screenshots/05-topology.png)

The topology shows **localhost** (the scanning machine) connected to **192.168.56.1**.

Saved file: [`topology/Nmap_-_Zenmap_GUI.pdf`](topology/Nmap_-_Zenmap_GUI.pdf)

## 4. Risk Analysis

This is an observation from host discovery, not a confirmed vulnerability. No port scanning or exploitation was performed.

| # | Finding | Evidence | Potential impact | Level |
|---|---|---|---|---|
| 1 | Live host visible on the lab network | Zenmap identified 192.168.56.1 on the lab subnet | Unknown or unauthorized devices could be present on a network if not tracked | Low |

## 5. Recommendations

1. **Perform regular internal network discovery:** periodically scan your own networks to identify active devices.
2. **Investigate unknown devices:** any unexpected device found during scanning should be verified.
3. **Maintain network documentation:** keep the topology and device information documented and up to date.
4. **Scan only with authorization:** run discovery only on networks you own or have written permission to test.

## 6. Lessons Learned

- A ping scan (`-sn`) is a quick, low-impact way to find live hosts because it does not scan ports.
- Scanning a whole `/24` checks 256 addresses, which took about 12 seconds here compared with about 1 second for one address.
- Small virtual or host-only lab networks usually contain very few hosts, so a single live host is a normal result.
- MAC addresses are not always shown, depending on where the host sits on the network.
- Zenmap's Topology tab makes the scan results easier to visualize and report.

## 7. Conclusion

Zenmap was used to run a ping scan across the `192.168.56.0/24` lab subnet. The scan found one live host, `192.168.56.1`, and the topology was exported as a PDF. This host discovery step is the foundation for later port scanning and service enumeration.

## Disclaimer

These materials are for education and research only. Do not scan or test networks you do not own or lack explicit written authorization to test.
