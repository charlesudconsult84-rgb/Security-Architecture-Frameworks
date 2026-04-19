# Security-Architecture-Frameworks
As a junior cybersecurity professional, you are tasked with ensuring that your organization’s IT infrastructure meets PCI DSS compliance standards. The organization handles sensitive financial data, and it is your responsibility to identify vulnerabilities, implement security controls, and enforce proper network segmentation to safeguard critical assets. You will assess the firewall configurations, conduct security scans using Wazuh and Nessus, and analyze network architecture to optimize compliance and security.

Your objective is to assess a segmented network consisting of a web server, database server, and compliance server that manages security monitoring. The internal application server (192.168.30.10) is currently under forensic investigation following an insider threat incident and is temporarily offline. While you will not interact with this server directly, its network segmentation settings in VyOS must remain intact to ensure readiness for relaunch once the investigation is complete.

Additionally, an automated data transfer process has been configured using cron jobs and scripted automation to securely transfer database backups from the Web Server to the Compliance Server, and then forward them to the Database Server. This process ensures that the Web Server and Database Server, which cannot communicate directly, can still exchange data securely through the Compliance Server acting as a bastion host. The transfer process uses SSH keys and SCP to securely move data between the servers.

  . You should not modify the existing cron jobs or scripts but may analyze their security effectiveness.
  . Any identified potential improvements or risks should be included in the Security Analysis and Best Practices            section of your report.

# Tools and Resources
Tools
  . Preconfigured OVAs for all servers 
       . Web ServerLinks to an external site.
       . Database ServerLinks to an external site.
       . Internal Server (under forensic analysis)
       . Compliance ServerLinks to an external site.
       . VyOS RouterLinks to an external site.
       . Desktop VMLinks to an external site.
Resources
       . PCI DSS Compliance Framework Documentation (PCI DSS v4.0.1Links to an external site.)
       . PCI DSS Implementation Guide (PCI Security Standards Document LibraryLinks to an external site.)
       . VyOS Documentation GuideLinks to an external site.(Chapter 5 will be the most helpful)
       . Credentials:
               . VMs & Servers: vagrant/vagrant
               . VyOS Router: vyos/vyos
               . Database Server MySQL Database User: admin/password
               . Web Server MySQL Database User: admin/admin
               . Wazuh: admin/VXBzATlmeNcFbwDG8cJ?KRDNyI1x*Rsz
               . Nessus: flatiron/flatiron

# Network Environment Details
Incident Context
Recently, a cybersecurity breach occurred due to an insider threat. The previous engineer was found responsible for malicious activities and was terminated. As a result, the Internal Application Server (192.168.30.10) has been taken offline and is currently under forensic investigation. Although you will not interact with this server, the VyOS firewall settings should remain configured to ensure it can be reintegrated into the network securely when necessary.

  . Router (VyOS): Manages firewall rules and traffic segmentation between servers.
         . The Web Server (192.168.10.10) can communicate externally but cannot communicate directly with the Database              Server (192.168.20.10) or Internal Server (192.168.30.10).
         . The Database Server and Internal Server must remain isolated from external access.
         . All servers communicate only with the Compliance Server (192.168.40.10) for monitoring.
  . Server 1: Web Server (192.168.10.10)
         . Role: Hosts customer-facing applications.
         . Vulnerabilities: SSL/TLS settings, insecure configurations (HTTP-only), open ports, logging disabled.
  . Server 2: Database Server (192.168.20.10)
         . Role: Stores sensitive PCI-related data.
         . Vulnerabilities: Weak database security, unencrypted data, improper access controls.
  . Server 3: Internal Application Server (192.168.30.10)
         . Status: Currently offline and under forensic investigation.
         . Context: Previously used for internal operations, this server was compromised in an insider threat incident.             While it remains inactive, its segmentation rules in VyOS must be preserved for reintegration post-                      investigation.
  . Server 4: Compliance Server (192.168.40.10)
         . Role: Runs Wazuh and Nessus for real-time monitoring and PCI DSS compliance scans.
  . Desktop VM (192.168.40.17)
         . Role: Provides a GUI environment for accessing Nessus/Wazuh and testing web server vulnerabilities (XSS, SQL             injection).
Note: If you add static routes using ip route add, be sure to also persist them by editing /etc/netplan/*.yaml and applying with sudo netplan apply. Otherwise, your routes will be lost after reboot.

# Instructions
Task 1: Network Segmentation Enforcement
1. Implement VyOS firewall rules to enforce proper segmentation.
2. Submit a screenshot of your VyOS firewall rules confirming segmentation.
3. Provide a detailed write-up explaining the rules, their necessity, and PCI DSS alignment.

Task 2: Hardening the Web Server and Database Server
Audit and improve SSL/TLS settings, logging, and mitigate common vulnerabilities like SQL injection and XSS on the web server.
Use the Desktop VM (192.168.40.17) to test for vulnerabilities.
  SQL Injection Test (SQLi): Run the following command from the Desktop VM to test if the web server is vulnerable         to SQL injection:

  Demonstrate web server vulnerabilities using the following commands:

  Cross-Site Scripting (XSS) Test: In a form field on the web application, enter the following payload and observe if it   executes: <script>alert('XSS')</script>

 curl -u admin:admin -d "name=Test OR 1=1 -- &email=test@test.com&credit_card=1234&cvv=234"                               http://192.168.10.10/checkout.php

Strengthen access control policies, encryption settings, and logging configurations on the Database Server.

Submit a summary of identified vulnerabilities and applied security enhancements. The number and severity of vulnerabilities reported by Nessus Essentials and Wazuh may differ. This is expected; each tool has its own scanning logic. In your report, compare and explain any major differences in the scan results.

Task 3: Security Analysis and Best Practices
1. Review the use of the Compliance Server as an intermediary for vulnerability assessments.
2. Answer the following:
   . Justify whether using an intermediary server is a best practice for security segmentation.
   . What vulnerabilities or inefficiencies might this design introduce?
   . Propose alternative security strategies that could improve efficiency without compromising PCI DSS compliance.

Automated Data Transfer Considerations:
 The Compliance Server is configured to securely transfer database backups from the Web Server to the Database Server     using SSH keys and SCP. Since direct communication between these two servers is restricted, the Compliance Server acts   as a bastion host for data relay.

 . You should not modify the existing cron jobs or scripts.
 . Instead, analyze their security effectiveness and determine whether any improvements or risks should be addressed.
 . If you believe alternative data transfer strategies would improve security or efficiency, justify your                   recommendations as part of your response.

Task 4: Compliance Scanning and Vulnerability Analysis
1. Run Nessus/Wazuh scans from the Compliance Server. Remember that Wazuh and Nessus may detect different                   vulnerabilities. Consider how each tool contributes to a more complete picture of system health.
2. Submit your Nessus scan results identifying vulnerabilities.
3. Evaluate the role of automated tools vs. manual checklists in PCI DSS compliance.
4. Answer the following:
   . What are the advantages and disadvantages of automated compliance tools compared to manual audits?
   . When would it be more effective to rely on automated scanning versus manual verification?
   . How would a hybrid approach improve vulnerability detection and PCI DSS compliance?

# Submission and Grading Criteria
Security Architecture Summative Assessment Report - link in Tools and Resources section

Grading:
Use the attached rubric in Canvas to understand how your submission will be graded. The rubric provides a detailed map of the criteria for scoring, including essential items to include and performance expectations.

Submission: 
Download and Copy the Report Template: this will prompt you to make a copy.
The link to the Report Template can be found in the Tools and Resources section

Remember: Once you make a copy, it will be saved to your Google Drive. Open this saved copy in your Drive each time you need to access the template. Avoid clicking the link again to prevent creating multiple copies.
Prepare Your Final Risk Register and Risk Assessment Report: Complete the template by following the instructions provided in the document.
Upload as PDF: Export your reports as a PDF and upload them to Canvas via the assignment submission page.

 
