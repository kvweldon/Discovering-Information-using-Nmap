# Discovering-Information-using-Nmap
Utilizing Kali Linux 2022.2 to further the discovery of information about the client's network using Nmap.
<h1>Phase 3</h1>
I will use Nmap to discover active systems, open ports, identify services and determine operating systems, on the 203.0.113.0/24 subnet as part of the Pentest for structureality.

**<p style="font-size: 15px;">Step 1: Discovery using Nmap.</p>**

I started my discovery by elevating the termnial privileges using "sudo su" to ensure I had access to all of the features and capabilites of Nmap. I also input "nmap" to read through some of the syntax in Nmap as a refresher. 

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/29850569-934e-49ab-9159-2ecff52c738a)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/d80dd45e-fd40-453b-bf91-c96e079d546f)

I ran a ping sweep of the network to see which systems would respond to the discovery and saved it to the file, client_pingsweep.nmap, using "203.0.113.0/24 -sn -oN client_pingsweep.nmap". I was able to discover a number of hosts on the subnet.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/e8c5a43a-3556-4bf8-b79a-7f7a984a94fb)

Next I ran a SYN scan of the top 100 common ports and captured the output in all available formats to a file with the basename client_scan1 using "nmap 203.0.113.0/24 -F -sS -oA client_scan1". The SYN scan(-sS) typically has the best chance of finding open ports because it simulates the initial communication attempt from a valid client.   

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/57ab8a64-e479-4aec-b8c3-cf5ecd0d451f)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/b71b4485-44b2-4242-a6a0-c5b531dd9657)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/2281ed56-e57d-4181-83a8-a469e7340919)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/9d161aa9-441a-4b78-80d5-b044329c2204)

Below, the "ls -l" command is used to list the files of the two previous scans. There are three cleint_scan1 files, normal output, grepable optimized version, and an XML format due to the '-oA' flag which utilizes all available formats to create files.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/6d213558-b371-4116-80fb-4f59e09500b2)

I then used the "cat client_scan1.gnmap" command to view the output of the grepable file. This file will be used to search for various key factors as grepable files are typically easier to search through and present the information in a more efficient manner.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/e1b0bcaf-df3f-4967-a4ad-22310d64c9b1)

Next, I located the hosts with open ports, highlighted in red, with the command "cat client_scan1.gnmap | grep open". There are several Border Gateway Protocol, BGP, supporting routers in the subnet which are not of interest currently. I will perform a scan to remove them and display information that is more relevant to my goal.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/942573fc-af9d-4a52-be06-5da5b0ef7a0b)

Here I use the command "client_scan1.gnmap | grep -v bgp | grep open" to remove the BGP ports and then refined my search further with "cat client_scan1.gnmap | grep -v bgp | grep '80/open\|443/open'". This last scan highlights the systems that are likely hosting websites or web interfaces which I will explore in greater detail in a future lab.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/78773c05-d2df-477a-9c39-321eb620d776)

After identifying the systems I would like to investigate further, I performed the scan "nmap 203.0.113.1,225,227-229,231 -F -sS -sV -oN client_versions.nmap". This ran a version scan on the top 100 ports(-F) against the previously highlighted systems likely hosting websites and saved the output to the file client_versions.nmap. This scan shows the targeted systems are running web services, secure shell, DNS and multiple email servers as well as an insecure version of Debian, Debian 5.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/8a41f102-f3b7-4680-9070-32df37d2938f)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/e18500cc-36aa-4954-ba70-ccee72a0066b)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/5ecf4e3c-7de8-491c-a284-22a7ad8b55cc)

Next I identified the operating systems of the target systems and caputred the scan to the file client_OSes.nmap using the scan "nmap 203.0.113.1,225,227-229,231 -F -sS -O -oN client_OSes.nmap". The scan reveals that the systems are running Linux between versions 4.15-5.6.

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/fafb8787-0272-4b45-9d00-a4e01231e16e)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/eca9e7f6-3c86-430f-9c35-197a7ef3789e)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/2878dc91-c5c0-4856-af87-4475342ed21c)

I then ran a "default" script scan on the targeted systems with the command "nmap 203.0.113.1,225,227-229,231 -sC -oN client_scriptscan.nmap". This scan discovered several important factors showing the systems are vulnerable including duplicate SSH host keys(225/227/228), alternate DNS names(229/231) and supported SMTP commands(1/225/228). Additionally, several systems are hosting websites which be explored in later labs. 

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/b8f071a5-1089-4061-a4e3-f080371a8ca1)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/304e0dbf-8e15-4f80-8155-3cbe9d666a5f)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/42dc43c1-0121-42a9-bdc1-7238eb4c4494)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/011a6efe-3fda-4703-afcc-1524191c770b)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/b5909d2f-208b-49c5-b3b5-87ce48649212)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/593a5ac5-86e5-4ac0-91a5-d38323408957)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/1fcc54b7-e21c-48d1-820f-2e2873b87fc9)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/0d85d394-44c0-4f4f-915b-31aa53088a96)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/84ec8c3d-6005-486c-b233-2603a4c864f3)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/4f6c3330-9299-4eb4-9c40-868bc0f0f17a)

AA

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/8caa739a-e494-4e10-bb82-6b7457b8d0be)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/f0a0520a-8c87-4b5a-951f-1dc2c45e22bd)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/2250209c-5a92-4c9d-9b44-0344f3e35d54)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/cb7a9b4a-cab2-426a-8724-8432ebdfdd5f)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/6ccbd6bf-c9b4-419b-90bd-fb17603654aa)

![image](https://github.com/kvweldon/Discovering-Information-using-Nmap/assets/141193154/403e2330-b263-4cd4-8a17-dbd78aa0e455)






