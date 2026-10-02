

# 🏥 Mediroza Hospital — Web Application Penetration Test

## 📌 Project Overview

In this project, I conducted an authorized penetration test and exploitation assessment on the Mediroza Hospital website using tools available in Kali Linux.
The purpose of the project was to demonstrate the different stages of a web application penetration test, beginning with reconnaissance and enumeration, followed by vulnerability identification and exploitation.



## 🎯 Objectives

The main objectives of this project were to:

- Perform reconnaissance against the Mediroza Hospital website.

- Identify the technologies used by the web application.

- Identify the presence of a Web Application Firewall.

- Identify open ports and services.

- Enumerate hidden directories and files.

- Identify weaknesses within the Patient Portal.

- Test the Patient Portal for SQL Injection.

- Demonstrate the possible impact of an authentication vulnerability.

- Locate protected patient PDF documents.

- Assess the strength of the passwords protecting the PDF documents using John the Ripper.

⚙️ Tools Used

1. WHOIS

2. WhatWeb

3. Wafw00f

4. Nmap / Zenmap

5. Gobuster

6. John the Ripper


🔍 Penetration Testing Procedure

1. 🌐 WHOIS Reconnaissance

The first stage of the assessment involved gathering publicly available information about the Mediroza Hospital domain.

The following command was entered into the Kali Linux terminal:

whois medirozahospital.com

WHOIS returned information about the domain registration including the registrar, registration dates and name servers.

The results showed that the domain was registered through NameCheap, Inc.

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/mediroza%20whois.png)

Figure 1: WHOIS reconnaissance results.

This information formed part of the initial reconnaissance stage of the penetration test.


2. 🖥️ Website Technology Identification Using WhatWeb

The next stage was to identify technologies being used by the Mediroza Hospital website.

The following command was used:  whatweb medirozahospital.com

The results identified information including:

HTTP response codes,LiteSpeed web server technology, the target IP address, HTTP redirects and Web server headers.

The IP address identified during the assessment was: 199.188.201.16



![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/whatweb%20report.png)


Figure 2: WhatWeb reconnaissance results.

The information gathered helped provide a better understanding of the technologies supporting the application.



3. 🛡️ Web Application Firewall Detection

The Wafw00f tool was used to determine whether the website was protected by a Web Application Firewall.

The command used was: wafw00f medirozahospital.com

The scan reported that the website was protected by the LiteSpeed Web Application Firewall.

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/Wafw00f%20report.png)

Figure 3: Wafw00f results showing LiteSpeed WAF detection.


Identifying a Web Application Firewall is important during reconnaissance because it helps the penetration tester understand some of the security measures already protecting the application.


4. 🔎 Network and Port Enumeration

Network enumeration was performed using Zenmap/Nmap and Legion.

An intensive Nmap scan was performed against the IP address identified during reconnaissance.

The scan used:

nmap -T4 -A -v 199.188.201.16

The results identified web-related services including:

Port 80 — HTTP

Port 443 — HTTPS

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/zenmap%20report.png)

Figure 4: Zenmap/Nmap scan results.

These results provided additional information about the network services exposed by the target.



5. 📂 Directory and File Enumeration Using Gobuster

The next stage involved searching the website for directories and files that were not directly visible through normal browsing.

Gobuster was used for this process.

The command used during the assessment was similar to: gobuster dir -u https://medirozahospital.com -w /usr/share/wordlists/dirb/common.txt -x php,txt,pdf -k

Gobuster tested filenames and directories using the supplied wordlist.

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/gobuster%20scan.png)

Figure 6: Gobuster directory and file enumeration.

During enumeration, an old backup/database-related file was identified that had not been properly secured.

A patient-related directory was also discovered.

The patient directory eventually led to the hospital's Patient Portal.

This demonstrated why sensitive backups and application directories should not be left publicly accessible on a production web server.


6. ## 🔐 Patient Portal and SQL Injection Testing

The directory enumeration stage led to the discovery of the Patient Portal located at: medirozahospital.com/patient/login.php

The portal required a username and password to access patient laboratory results.

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/login.png)

!=[image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/login%202.png)

Figure 7: Mediroza Hospital Patient Portal.

Normal authentication attempts produced an Incorrect password response.

Further testing was then conducted on the login form to determine whether user input was properly validated.



Figure 8: Patient login testing.

An SQL Injection test was performed against the username field.

One of the test strings used during the authorized assessment was:

admin' --

![image alt}(https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/login3.png)

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/pdf%20files.png)

Figure 9: SQL Injection test entered in the Patient Portal.

The testing demonstrated that the application's authentication mechanism was vulnerable to SQL Injection.

According to the results of the authorized assessment, the vulnerable input made it possible to bypass the intended authentication controls and reach protected patient resources.

This was one of the most serious vulnerabilities identified during the project.

7. ## 📄 Discovery of Patient PDF Reports

After accessing the patient section of the application, three patient pathology reports were identified.

The reports were stored as PDF documents and were protected with passwords.

One of the documents is shown below.

![image alt ](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/Screenshot%202026-10-02%20162115.png)

Figure 10: Example pathology laboratory report accessed during the authorized test.

The ability to reach these documents demonstrated the potential security impact of the SQL Injection vulnerability.

In a real hospital environment, unauthorized access to medical records could lead to a serious confidentiality and privacy breach.



8. ## 🔑 PDF Password Testing Using John the Ripper

The patient PDF documents were password protected.

To assess the strength of the document passwords, the PDF password hashes were extracted using pdf2john.

The basic process used was:

pdf2john patient_report_2.pdf > patient_report_2.hash.txt

John the Ripper was then used with the RockYou password wordlist:

john --format=PDF --wordlist=/usr/share/wordlists/rockyou.txt patient_report_2.hash.txt

The screenshot below shows John the Ripper successfully loading the PDF hash and performing the password test.

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/password%20cracking%202.png) 

Figure 11: John the Ripper PDF password test for patient_report_2.

The same procedure was carried out against another protected PDF document.

john --format=PDF --wordlist=/usr/share/wordlists/rockyou.txt patient_report_3.txt

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/password%20cracking%203.png) 

Figure 12: John the Ripper PDF password test for patient_report_3.

The testing demonstrated that weak passwords protecting sensitive documents can be recovered through dictionary-based password testing.

An online password-testing tool provided by Networkwalks was also used to demonstrate another method of testing the password hash.

The screenshot below shows a successful password match.

![image alt](https://github.com/Chims79/Networwalks-B083B-Penetration-Testing-Project-Mediroza-General-Hospital/blob/75c2749588f6446ff6beadbd79754d8289f25064/password%20cracking.png)

Figure 13: Networkwalks password cracker showing a successful password match.

This stage demonstrated that encrypting a document does not provide sufficient protection when the password itself is weak or predictable.


## 🚨Vulnerabilities Identified

- Publicly discoverable sensitive directories and files

- Exposed backup/database-related file

- SQL Injection vulnerability

- Weak authentication controls

 -Authentication bypass

- Exposure of confidential patient PDF documents

- Weak PDF passwords

## 🔗 Attack Chain

Reconnaissance

↓

Technology Identification

↓

Network Enumeration

↓

Directory and File Enumeration

↓

Discovery of Patient Portal

↓

SQL Injection Testing

↓

Authentication Bypass

↓

Access to Patient PDF Reports

↓

PDF Hash Extraction

↓

Password Testing with John the Ripper

The assessment showed how several security weaknesses can be combined to create a much greater security risk.

## 🛠️ Recommendations

- Remove backup files from publicly accessible web directories.

- Store sensitive files outside the public web root.

- Use prepared statements and parameterized SQL queries.

- Properly validate all user-supplied input.

- Implement strong authentication controls.

- Implement authorization checks before providing access to patient records.

- Use strong and unique passwords for encrypted documents.

- Monitor failed login attempts.

- Maintain application and web server logs.

- Regularly update the operating system, web server and application components.

- Conduct regular vulnerability assessments and penetration tests.

💡 Key Takeaways

- Reconnaissance is an important part of penetration testing because it provides information that may assist later stages of the assessment.
  
- Directory enumeration can reveal resources that were not intended to be publicly accessible.
  
- Backup files should never be stored inside publicly accessible directories.
  
- SQL Injection can have a serious impact when user input is passed directly to a database without proper protection.
  
- Authentication alone is not sufficient; applications should also enforce authorization for individual resources.

- Medical and other confidential documents should be protected using strong access controls.

- Password-protected documents should use strong and unpredictable passwords.

- John the Ripper can be used during authorized security assessments to evaluate password strength.

- Several relatively small security weaknesses can sometimes be chained together to produce a serious security breach.
- 
 ---

## ⚖️ Disclaimer

The information provided here is meant solely for learning and legitimate and sanctioned research purposes.

The Mediroza Hospital website used during this project was created for educational purposes, and permission to conduct the penetration test was granted by my lecturer.

Accessing or testing computer systems without proper authorization is illegal in most jurisdictions.

Every activity documented in this repository was performed within an authorized educational environment.

----

## 👤 Author

