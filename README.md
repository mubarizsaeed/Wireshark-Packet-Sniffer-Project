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
I select the Ethernet adapter on my wireshark in my VM to capture traffic on google.com
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/0757f772-1656-4bbe-ad73-4ae3c9ab8444)

I use `'ip.dst == 10.0.2.15'` , ip.dst filter is used to filter packets based on their destination IP address. 
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/7684208f-ef20-458a-8d3e-9b7573a9247f)

I want to see the first handshake when visiting Google, so I apply the filter `tcp.port == 443 && frame matches "google"`

![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/b6e23d29-254f-4fe5-98fc-4e29927303b6)
The filter `tcp.port == 443 && frame matches "google"` in Wireshark selects packets from port 443(https traffic) containing the string "google" in their payload, effectively filtering for HTTPS traffic with occurrences of the term "google." I know it uses TLSv1.3 protocol and it is the first packet-capture

Now I want to see a TCP conversation filter I will go up to Stats ---> Conversations:
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/84a880aa-6c7a-4628-93a2-74f18a427245)
Then at the Wireshark Conversation window TCP category, I'll select a conversation I'm interested which is the packets associated with Google IP which I know is `142.251.32.110`
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/f41c344f-5112-42d2-8047-bb238e9aa22b)

The filter gets applied `ip.addr==10.0.2.15 && tcp.port==37106 && ip.addr==142.250.65.195 && tcp.port==443`
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/5ffb9127-9796-45eb-b9c7-7b6ae6e7da92)
This Wireshark filter isolates TCP packets with source IP 10.0.2.15 on port 37106, destined for Google's 142.250.65.195 on HTTPS port 443

I can also use the matches or contains keyword to find a specific string in Wireshark: `ip matches "google"` or `eth matches "google" `
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/07418985-36e7-4d93-a3a4-923331f25312)

### Start capture on Vulnerable website using HTTP protocol  
I will visit the site `http://www.vulnweb.com/` it is a site to help you with manual penetration testing or for educational purposes
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/06985044-b088-4cc3-8d4c-48044b7f144f)
I will choose the security tweets to test the website to log in while having my packet capture running on Wireshark:
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/8655b84d-98f6-43c5-bf9f-27c280416e9a)
Now I will filter through the traffic searching for HTTP traffic and snifffor the login credentials:
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/5614c1d1-86c5-48bf-9a61-9215aa2759e3)
I will use Wiresharks built in follow HTTP stream to find the credentials I'll select the POST response because the form data (such as username and password) is often sent to the server using a POST request.:
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/90a28003-fa1b-4437-a4ab-eb9c92d8a67c)
The stream shows the GET request then the POST request from the serverside which include the username and password:
![image](https://github.com/mubarizsaeed/Wireshark-Packet-Sniffer-Project/assets/98554238/368a5997-2c96-4aba-af74-66efe1323b7b)
In the sequence, an initial GET request fetches popular content, followed by a login attempt (POST request) with plaintext credentials. Subsequently, a redirection occurs after a successful login, and a GET request to the root is made with a cookie indicating the authenticated user. It is crucial for all websites to use encrypted protocols such as HTTPS



