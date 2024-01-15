# Wireshark-Packet-Sniffer-Project

In this Wireshark project, I aim to demonstrate proficiency in network analysis and packet inspection. The guide will cover basic Wireshark functionalities along with more advanced techniques such as applying filters and evaluating encrypted traffic. I will showcase ethical hacking practices by including packet sniffing from an intentionally vulnerable website. I will also decrypt HTTPS and TLS data

## Part 1: Setup Kali Linux, Starting Wireshark

- Downloaded Kali Linux ISO, validated checksum.
- Created VirtualBox VM (2GB RAM, 20GB disk).
- Installed Kali with default options.
- Set username/password.
- Completed graphical install.
- Kali Linux VM ready with preinstalled Wireshark.

<img src="Wireshark%20Project%20images/setup.png" alt="Setup Image" width="1000"/>


## Start Capture on eth0 and Implementing filters 
I select the Ethernet adapter on my wireshark in my vm to capture traffic on google.com
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/0757f772-1656-4bbe-ad73-4ae3c9ab8444)

I use `'ip.dst == 10.0.2.15'` , ip.dst filter is used to filter packets based on their destination IP address. 
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/7684208f-ef20-458a-8d3e-9b7573a9247f)

I want to see the first handshake when visiting google, so I apply the filter `tcp.port == 443 && frame matches "google"`

![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/b6e23d29-254f-4fe5-98fc-4e29927303b6)
The filter `tcp.port == 443 && frame matches "google"` in Wireshark selects packets from port 443(https traffic) containing the string "google" in their payload, effectively filtering for HTTPS traffic with occurrences of the term "google." I know it uses TLSv1.3 protocol and it is the first packetcapture

Now I want to see a TCP conversation filter I will go up to Stats ---> Conversations:
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/84a880aa-6c7a-4628-93a2-74f18a427245)
Then at the Wireshark Converstation window TCP category I'll select a conversation im interested which is the packets associated with googles ip which I know is `142.251.32.110`
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/f41c344f-5112-42d2-8047-bb238e9aa22b)


