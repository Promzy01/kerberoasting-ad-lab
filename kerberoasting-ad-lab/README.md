Kerberoasting Attack & Detection in Active Directory Lab
This project demonstrates a full Kerberoasting attack lifecycle from configuring the environment, executing the attack, detecting it with Splunk, and exporting results for reporting and analysis. It's part of my ongoing cybersecurity learning and lab experience.

Objective
To simulate a Kerberoasting attack in an Active Directory lab, detect the malicious activity using Splunk, and export the evidence for reporting.

Tools Used
Kali Linux (Attacker VM)
Windows Server 2019 (Domain Controller and splunk forwarder)
Windows 10 (for Splunk enterprice)
Splunk 10.0.0 (SIEM)
Hashcat (Offline password cracking)
Impacket (SPN extraction)
VirtualBox (Lab environment)

Lab Environment

The Windows Server was assigned IP address 10.0.3.16 and configured to communicate with the internal network (see Figure 1).
The Windows 10 machine was assigned IP address 10.0.3.15 (see Figure 2).
![Figure 1 - Windows Server IP config](images/screenshot_1.png)

Windows 10	Splunk Enterprice	10.0.3.15
![Figure 2 - Windows 10 IP config](images/screenshot_2.png)

Domain Controller Configuration
The Windows Server was promoted to a Domain Controller, with Active Directory Domain Services (AD DS) and DNS installed (see Figure 3).
User accounts such as User1, Admin1, and ServiceAccount1 were created to simulate a realistic domain setup (see Figure 4).

![Figure 3 - Windows Server was promoted to a Domain Controller, with Active Directory Domain Services (AD DS) and DNS installed.](images/screenshot_3.png)

Created a domain: lab.local.
Created users: User accounts were added to simulate real domain activity.
![Figure 4 - User creation in AD](images/screenshot_4.png)


Splunk forwarder was installed (see Figure 5), Installed Sysmon to generate advanced event logs(see figure 6).  Splunk Enterprise was installed on the Windows 10 machine, where we would later aggregate and analyze logs (see Figure 7).
The inputs.conf file was configured to receive logs from the forwarder over port 9997 (see Figure 8).
Windows 10 was then successfully joined to the lab.local domain (see Figure 9), confirming domain connectivity and credential use.Figure 5 - Splunk Forwarder install
Installed Sysmon to generate advanced event logs.
![Figure 6 - Sysmon install](images/screenshot_5.png)



2. Windows 10 (Log Source)
Installed Splunk Enterprise
![Figure 7 - Splunk Enterprise installed](images/screenshot_6.png)
Installed and configured the input.conf to receive Forwards from default port 9997 to send logs to Splunk.
![Figure 8 – Default receiving  port](images/screenshot_7.png)

Joined Windows 10 to the domain lab.local.
Confirmed domain login success.
![Figure 9 - Windows 10 joined domain](images/screenshot_8.png)
The attack began with Impacket's GetUserSPNs.py tool, used to enumerate Kerberos Service Principal Names (SPNs) and extract a TGS hash for ServiceAccount1 (see Figure 10).
The resulting hash was saved to a file named hash.txt for offline cracking (see Figure 11).
We then used Hashcat with the rockyou.txt wordlist to crack the TGS ticket offline (see Figure 12).
The password password123! was successfully recovered from the TGS hash, completing the Kerberoasting attack (see Figure 13).


Phase 2: Simulating Kerberos Attack (Kali)
The attack began with Impacket's GetUserSPNs.py tool, used to enumerate Kerberos Service Principal Names (SPNs) and extract a TGS hash for ServiceAccount1 (see Figure 10).
The resulting hash was saved to a file named hash.txt for offline cracking (see Figure 11).
We then used Hashcat with the rockyou.txt wordlist to crack the TGS ticket offline (see Figure 12).
The password password123! was successfully recovered from the TGS hash, completing the Kerberoasting attack (see Figure 13).
![Figure 10 - GetUserSPNs.py output](images/screenshot_9.png)



![Figure 11 - Saved extracted hash to file](images/screenshot_10.png)

Cracked TGS hash using Hashcat:
hashcat -m 13100 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
![Figure 12 - Hashcat cracking](images/screenshot_11.png)


Cracked TGS Hash with Hashcat
hashcat -m 13100 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
Result: Password cracked — password123!
Figure 12 shows Successful crack output
![Figure 13 - Cracked output](images/screenshot_12.png)



Phase 3: Event Detection in Splunk
We searched for Kerberos Ticket Granting Service (TGS) events by filtering EventCode=4769 in Splunk (see Figure 14 and 15).
Logged into Splunk web interface on Windows 10.
Searched for Kerberos TGT activity using:
index=* EventCode=4769
![Figure 14 - Splunk query](images/screenshot_13.png)

![Figure 15 - Splunk field breakdown](images/screenshot_14.png)


Report Export
Exported Splunk results as CSV report for documentation and analysis.
Attach this CSV file to your GitHub repo: 1754574121_48.csv

Summary of Attack Chain

Final Notes
This lab demonstrates not only the Red Team technique of Kerberoasting, but also the Blue Team response, showcasing:
Active Directory familiarity
SIEM (Splunk) log analysis
Cyber attack detection
Practical reporting skills


Let’s Connect
You can find this project on GitHub and more on my  (feel free to add your link).

End of Project – Feel free to fork, clone, or reach out with suggestions.

