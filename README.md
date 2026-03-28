# Wireshark-Qakbot-packet-analysis


Title: Wireshark Qakbot packet analysis

Analyst: Savon Masters 

Date: March 3rd, 2026


                                                        Summary 


Qakbot is a trojan malware that has different purposes, like hiding on a network, interacting with devices like windows and androids, and injecting malware. It is able to interrupt a whole system's workability if left unsolved. To update my analysis and investigation approach to packets I went through this malware. I wanted to see how malware hides, attacks, and is obtained in a normal environment. 


                                                        Investigation  


![image alt](https://github.com/SavonMasters/Wireshark-Qakbot-packet-analysis/blob/169c97489f4f6e8b9b502acddd8aeec97465f2f6/Screenshot%20from%202026-03-11%2018-24-56.png)
I first start by looking at the capture file properties to get the data about the packet. In this packet it started “2023-02-03 12:02:36”, had a duration of “02:52:06” and delivered “55207” packets. 


![image alt](https://github.com/SavonMasters/Wireshark-Qakbot-packet-analysis/blob/0c58d789459cc76665e9f16ae29dfa4fbf8d3908/Screenshot%20from%202026-03-11%2018-34-01.png)
I go over to the protocol hierarchy statistics to understand the most active protocol used in the packet. I am curious on the SMB, HTTP, and ARP (Address resolution protocol) traffic that was generated. 


![image alt](https://github.com/SavonMasters/Wireshark-Qakbot-packet-analysis/blob/961d036c328356753b30b9166fb3ebac8f1b78c4/Screenshot%20from%202026-03-11%2018-41-43.png)
I select the conversations feature, it will show who’s most likely is our client IP address and different IP addresses talking with them. In this session you can see “10[.]0[.]0[.]149” is likely the clients IP addresses and “10[.]0[.]0[.]6” and “208[.]187[.]122[.]74” talking the most to them. 



I chose to break down the HTTP packets first. This first packet contain a GET request for “/86607.dat” from a unsecure website “hxxp[://]128[.]254[.]287[.]55/86607.dat” by the user agent “curl/7.83.1”. The curl user agent is used to transfer data to a server and the “MZ” in the binary is a message for an executable in the payload. 




With the data that “86607.dat” is an executable I exported it to the command line, could be able to get the SHA256 hash “713207d9d9875ec88d2f3a53377bf8c2d620147a4199eb183c13a7e957056432”. I sent the hash to virus total and received the output that it was a malicious trojan malware in relation to the Qaktbot malware. 



Qakbot is a multi-layered trojan malware used to steal information, deliver ransomware and infect network systems. It’s been noticed to list tactics of system network configuration discovery and internet connection discovery using ARP and ICMP. I connected that an ARP scan had taken place because of the multiple ARP requests to decreasing IP addresses, in order in a specific time window from the same MAC address “IntelCor_9e:42:fb”. Another ICMP requests were made to see if a network connection could be allowed to the IP addresses. 



I went ahead and was taught on MITRE that Qakbot can move laterally like a worm on SMB. The SMB traffic had dropped DLLs “efweioirfbtk.dll”, “umtqqzkklrgp.dll”, and “ltoawuimupfxvg.dll” to presumably take actions upon the network. 



I needed to export the SMB objects to the command-line to keep the SHA256 hash when on virustotal I looked and saw the hash was the same as the “86607.dat” executable from earlier. At that point I knew the attacker had connected “10[.]0[.]0[.]6” with the malware being allowed to laterally move on the network. 


                                                        Conclusion 

The attack happened when our client “10[.]0[.]0[.]9” requested a HTTP GET request from the online source “hxxp[://]128[.]254[.]287[.]55” for the “86607.dat” file. A whole reputation check revealed it was a trojan malware the same as the Qakbot. The attack went on and left evidence on the SMB traffic an executable had laterally moved and affected “10[.]0[.]0[.]6”  having left DLLs “efweioirfbtk.dll”, “umtqqzkklrgp.dll”, and “ltoawuimupfxvg.dll” on them. 


                                                        IOCs

The attackers IP address and website “128[.]254[.]287[.]55” “hxxp[://]128[.]254[.]287[.]55”
The infected link from the website “86607.dat”.
The link’s SHA256 hash “713207d9d9875ec88d2f3a53377bf8c2d620147a4199eb183c13a7e957056432”.
The hash’s record being together with the Qakbot. 
A visible ARP scan being displayed.
The DLLs being messaged to “10[.]0[.]0[.]6” “efweioirfbtk.dll”, “umtqqzkklrgp.dll”, and “ltoawuimupfxvg.dll”.
The DLL’s hash being the same as the “86607.dat” 
The MITRE techniques performed by the malware “T1071.001 Application’s layer protocol:web protocols”, “T1210 Exploitation of remote services”, “T1016 System network configuration discovery", “T1049 System network connections discovery”. and “T1204.001 User execution:malicious links”. 


                                                          Recommendations 

Block the attacker’s IP address and site “128[.]254[.]287[.]55” “hxxp[://]128[.]254[.]287[.]55”.
Delete the malware from the server. 
Add the executable to the antivirus software firewall. 
Start to get alerts for ARP scans transmitted on the server.
Insert all the DLLs in the antivirus software firewall with the other executable. 
Do not use HTTP port for anything because it is open for security events. 
Compare the server to see if IOCs are on the server right now.
Decide whether “10[.]0[.]0[.]6” post attack has moved laterally to deeper in the server. 


                                                        Things I learned 

Wiresharks query language and layout of the tool.
How the malware finds information and moves on the server. 
Research known IOCs of the malware see related events happening on the server.
Investigate each of the communications channels on a server for new levels to attack.
Pulling MITRE backstory and days of the attacks taking place before.
