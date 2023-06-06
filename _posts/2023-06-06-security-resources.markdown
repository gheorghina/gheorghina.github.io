---
layout: post
title:  "Getting up to speed in Cybersecurity"
date:   2023-06-02 00:18:23 +0700
categories: [distributed systems, cybersecurity, best practices]
---


### NIST 800-61 - Computer Security Incident Handling Guide

NIST stands for the National Institute of Standards and Technology. It is a physical sciences laboratory and a non-regulatory agency of the United States Department of Commerce. NIST's mission is to promote U.S. innovation and industrial competitiveness by advancing measurement science, standards, and technology.

NIST is responsible for developing and maintaining a wide range of standards and guidelines that cover various areas, including cybersecurity, information technology, physical measurements, engineering, manufacturing, and more. These standards and guidelines are designed to promote accuracy, consistency, and reliability in measurements, as well as to enhance cybersecurity practices and promote interoperability among different systems.

NIST is known for its work in developing and promoting the use of cryptographic standards, such as the Advanced Encryption Standard (AES) and the Secure Hash Algorithm (SHA). It also provides testing and certification services for various industries, including technology and manufacturing sectors.

### NIST Cybersecurity Framework

[The NIST (National Institute of Standards and Technology) Cybersecurity Framework](https://www.nist.gov/cyberframework) is a set of guidelines, best practices, and standards developed by the United States government to help organizations manage and improve their cybersecurity posture. The framework provides a flexible and risk-based approach to cybersecurity, focusing on five key functions: Identify, Protect, Detect, Respond, and Recover.

1. Identify: This function involves understanding and managing cybersecurity risks by identifying and documenting the assets, systems, and data that need protection. It includes activities such as conducting risk assessments, establishing governance processes, and creating an inventory of critical resources.

2. Protect: The Protect function aims to implement safeguards to ensure the security of assets and prevent or mitigate potential cyber threats. It covers activities such as access control, data encryption, security awareness training, secure configuration management, and implementing measures to protect against malware and unauthorized access.

3. Detect: This function involves implementing measures to identify and detect cybersecurity events and incidents promptly. It includes activities such as implementing monitoring systems, logging and analysis of network traffic, and establishing incident response capabilities to detect and respond to security breaches.

4. Respond: The Respond function focuses on developing and implementing response plans to effectively manage and mitigate cybersecurity incidents. It includes activities such as establishing an incident response team, defining incident response procedures, conducting forensic investigations, and communicating with stakeholders during incidents.

5. Recover: The Recover function aims to restore normal operations and recover from cybersecurity incidents. It involves activities such as developing and implementing recovery plans, conducting post-incident reviews, and updating security measures to prevent similar incidents in the future.

The NIST Cybersecurity Framework is designed to be scalable and adaptable to different organizations, regardless of their size, industry, or cybersecurity maturity level. It serves as a common language and reference point for organizations to assess their cybersecurity capabilities, prioritize investments, and improve their overall cybersecurity resilience.

By following the NIST Cybersecurity Framework, organizations can establish a systematic and risk-based approach to cybersecurity, enhance their ability to prevent, detect, and respond to cyber threats, and ultimately protect their critical assets and information from evolving cybersecurity risks.

#### Design Principles

- Implement Strong identify foundation

- Enable traceability

- Apply security at all layers

- Automate security best practices

- Protect data in transit and at rest

- Prepare for security events

### AWS Security Support

**Identity and Access Management (IAM)**: IAM enables you to manage user access to AWS services and resources. It allows you to create and manage user accounts, assign permissions using policies, enforce multi-factor authentication (MFA), and integrate with external identity systems.

**Virtual Private Cloud (VPC)**: VPC provides a logically isolated virtual network within AWS. It allows you to define and control network configurations, including IP addressing, subnets, routing tables, and security groups. VPC enables you to establish secure communication between resources and connect to on-premises networks using Virtual Private Network (VPN) or AWS Direct Connect.

**Security Groups and Network Access Control Lists (ACLs)**: Security groups and ACLs are used to control inbound and outbound traffic at the network level. They act as virtual firewalls, allowing you to define rules to permit or deny specific types of traffic to your AWS resources.

**Encryption**: AWS offers various encryption options to protect data at rest and in transit. This includes server-side encryption for storage services like Amazon S3, Amazon EBS, and Amazon RDS. AWS Key Management Service (KMS) provides centralized key management for encryption keys, and AWS Certificate Manager (ACM) helps provision, manage, and deploy SSL/TLS certificates for secure communication.

**Logging and Monitoring**: AWS provides services like AWS CloudTrail and Amazon CloudWatch to monitor and log activities and events within your AWS account. CloudTrail tracks API activity and allows you to audit and review actions, while CloudWatch offers metrics, logs, and alarms for monitoring resource utilization and performance.

**DDoS Mitigation**: AWS Shield is a managed Distributed Denial of Service (DDoS) protection service that helps safeguard AWS resources from volumetric, state-exhaustion, and application layer DDoS attacks. AWS Shield Standard is automatically included with all AWS accounts, providing basic DDoS protection, while AWS Shield Advanced offers enhanced protection and DDoS response support.

**Compliance and Security Certifications**: AWS has numerous certifications and compliance programs, including ISO 27001, PCI DSS, HIPAA, SOC 1/2/3, and more. These certifications demonstrate AWS's commitment to maintaining robust security controls and help customers meet their compliance requirements.

#### AWS Resources

- [AWS WA](https://aws.amazon.com/well-architected-tool/) is designed to help review the state of applications and workloads against architectural best practices. It identifies opportunities for improvement, and tracks progress over time.

- [AWS Well Architected](https://aws.amazon.com/architecture/well-architected/) 

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/2023-04-10/framework/sec_appsec_perform_regular_penetration_testing.html)

- [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html)

- [AWS Security Incident Response Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html)

- [Ransomware mitigation: Top 5 protections and recovery preparation actions](https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)

- [Organizing Your AWS Environment Using Multiple Accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/advanced-organization.html)

- [VPC sharing: A new approach to multiple accounts and VPC management](https://aws.amazon.com/blogs/networking-and-content-delivery/vpc-sharing-a-new-approach-to-multiple-accounts-and-vpc-management/)

### GCP Security Support

**Identity and Access Management (IAM)**: IAM allows you to manage user identities, control access to GCP resources, and enforce fine-grained permissions. It provides centralized identity management, supports multi-factor authentication (MFA), and integrates with external identity systems.

**Virtual Private Cloud (VPC)**: GCP VPC allows you to create isolated virtual networks in the cloud. It provides network-level security controls, including firewall rules, network segmentation, and subnet-level access controls. VPC Network Peering and VPN tunnels enable secure connectivity between GCP and on-premises networks.

**Encryption**: GCP provides encryption options to protect data at rest and in transit. Google Cloud Storage encrypts data by default, and you can also use customer-managed encryption keys (CMEK) for additional control. Google Cloud Key Management Service (KMS) offers centralized key management for encryption keys used across GCP services.

**Cloud Identity-Aware Proxy (IAP)**: IAP provides secure access to GCP resources through context-aware access controls. It allows you to grant access based on user identity and context, rather than relying solely on IP-based controls.

**Google Cloud Armor**: Cloud Armor is a web application firewall (WAF) service that helps protect applications from web-based attacks. It provides a set of security policies to filter and block malicious traffic based on IP addresses, geolocation, and other criteria.

**Google Cloud Security Command Center (Cloud SCC)**: Cloud SCC offers centralized visibility and control over security-related data and resources in GCP. It provides security and risk insights, helps identify vulnerabilities, and offers recommendations to improve security posture.

**Compliance and Security Certifications**: GCP has obtained various industry certifications and compliance attestations, such as ISO 27001, SOC 1/2/3, HIPAA, PCI DSS, and more. These certifications demonstrate GCP's commitment to implementing and maintaining robust security controls.

Additionally, GCP offers security services like Cloud DLP for data loss prevention, Cloud HSM for hardware-based key storage and cryptographic operations, and Google Cloud Security Scanner for web application vulnerability scanning.

### Azure Security Support

**Azure Active Directory (Azure AD)**: Azure AD is Microsoft's cloud-based identity and access management service. It enables you to manage user identities, control access to Azure resources, enforce multi-factor authentication (MFA), and integrate with on-premises Active Directory environments.

**Azure Virtual Network (VNet)**: Azure VNet allows you to create isolated virtual networks in the cloud. It provides network-level security controls, including network security groups (NSGs) and Azure Firewall, to filter and control inbound and outbound traffic. Virtual Private Network (VPN) and Azure ExpressRoute allow secure connectivity between Azure and on-premises networks.

**Azure Security Center**: Azure Security Center provides centralized security management and threat protection for Azure resources. It offers security recommendations, vulnerability assessments, threat detection, and incident response capabilities. It helps identify and remediate security issues across your Azure environment.

**Encryption**: Azure provides various encryption options to protect data at rest and in transit. Azure Storage Service Encryption helps encrypt data stored in Azure Blob storage, Azure Files, and Azure Queue storage. Azure Disk Encryption enables encryption for virtual machine disks. Azure Key Vault provides centralized key management and secure storage for encryption keys.

**Azure DDoS Protection**: Azure DDoS Protection defends against Distributed Denial of Service (DDoS) attacks. It automatically detects and mitigates volumetric and application layer DDoS attacks, helping ensure the availability of your applications and services.

**Logging and Monitoring**: Azure provides services like Azure Monitor and Azure Sentinel for logging, monitoring, and threat detection. Azure Monitor collects and analyzes metrics and logs from Azure resources, while Azure Sentinel is a cloud-native security information and event management (SIEM) solution for intelligent security analytics and response.

**Compliance and Security Certifications**: Azure has obtained numerous certifications and compliance attestations, such as ISO 27001, SOC 1/2/3, HIPAA, PCI DSS, and more. These certifications demonstrate Azure's adherence to robust security controls and help customers meet their compliance requirements.

Additionally, Azure offers a variety of security-related services, including Azure Firewall, Azure Web Application Firewall (WAF), Azure Security Center for IoT, Azure Information Protection, and more. These services provide additional layers of security and protection for specific use cases and scenarios.


On all three AWS, GCP and Azure it's crucial to follow security best practices, implement proper configurations, and regularly update and patch your resources to maintain a secure environment. All provide documentation, security guidelines, and the Well-Architected Frameworks to help users design and deploy secure architectures on the platform.

### The most common used tools in cybersecurity


### Wireshark


[Wireshark](https://www.wireshark.org/) is a popular open-source network protocol analyzer. It is a software tool used for capturing, analyzing, and troubleshooting network traffic. Wireshark allows you to inspect and dissect network packets in real-time or from saved capture files.


Some key features and functionalities of Wireshark:


**Packet Capture**: Wireshark captures network packets from a live network interface or from pre-recorded capture files. It supports various capture formats, including Ethernet, Wi-Fi, USB, and more.


**Protocol Analysis**: Wireshark decodes and analyzes network protocols at different layers of the network stack, including Ethernet, IP, TCP, UDP, HTTP, DNS, SSL/TLS, and many others. It provides detailed information about each packet and the corresponding protocol headers.


**Filtering and Search**: Wireshark allows you to apply filters to focus on specific network traffic based on criteria such as source/destination IP addresses, ports, protocols, or packet contents. It also provides search capabilities to locate specific packets or packet patterns within a capture file.


**Packet Inspection**: Wireshark provides a comprehensive view of network packets, allowing you to inspect the contents of individual packets, including payload data, protocol-specific fields, and metadata.


**Statistics and Analysis**: Wireshark offers various statistics and analysis tools to help you understand network behavior and identify anomalies. This includes graphical representations of packet flow, conversation analysis, and protocol-specific statistics.


**Protocol Dissection and Decoding**: Wireshark understands numerous network protocols and can dissect and decode their data structures, providing meaningful information about each protocol layer.


**Export and Reporting**: Wireshark allows you to export captured packets or analysis results in various formats, such as plain text, CSV, XML, or even as an image for reporting or further analysis.


Wireshark is widely used by network administrators, security professionals, developers, and researchers for a range of purposes, including network troubleshooting, network monitoring, security analysis, protocol debugging, and educational purposes. Its rich set of features and extensive protocol support make it a powerful tool for network analysis and understanding network behavior.

### Honeypot


[Honeypot](https://honeypot.is/) is a cybersecurity technique or tool that is used to deceive and gather information about attackers or malicious activities. It is essentially a trap or decoy system designed to attract and divert unauthorized users or potential attackers.

A honeypot is typically a computer, network, or application that appears to be a legitimate target but is actually isolated and closely monitored. It is intentionally made vulnerable or attractive to exploit in order to lure attackers. The honeypot can mimic real systems or contain enticing data or resources to entice attackers.

The primary purpose of a honeypot is to observe and analyze the tactics, techniques, and tools used by attackers. By studying their behavior, security professionals can gain insights into their methods and motivations, identify new attack patterns, and develop better defenses to protect real systems.

There are different types of honeypots, including low-interaction honeypots, which simulate only limited services or protocols, and high-interaction honeypots, which provide a more comprehensive emulation of real systems. Honeypots can be deployed at various levels, such as network-level honeypots, which capture network-based attacks, and application-level honeypots, which focus on specific applications or services.

It's important to note that honeypots should be used with caution and deployed by experienced cybersecurity professionals. Improperly configured or unmonitored honeypots can potentially be used against an organization or cause unintended consequences.


### Additional cybersecurity tools


**Nmap**: A powerful and widely used network scanning tool that helps identify open ports, services running on systems, and potential vulnerabilities in a network.


**Metasploit**: A penetration testing framework that provides a wide range of exploits, payloads, and tools to test the security of systems and identify vulnerabilities.


**Nessus**: A popular vulnerability scanning tool that identifies vulnerabilities in systems and provides recommendations for remediation. It helps organizations assess and manage their security posture.


**Snort**: An open-source intrusion detection and prevention system (IDS/IPS) that monitors network traffic for suspicious patterns or known attack signatures and alerts administrators.


**Burp Suite**: A comprehensive web application security testing tool that allows for manual and automated scanning of web applications, identifying security flaws and vulnerabilities.


**Splunk**: A log management and analysis platform that helps collect, monitor, and analyze logs from various sources, enabling organizations to detect security incidents and investigate anomalies.


**OpenVAS**: An open-source vulnerability assessment tool that scans systems for known vulnerabilities and provides detailed reports for remediation.


**GnuPG**: A free and open-source implementation of the Pretty Good Privacy (PGP) encryption standard, providing cryptographic privacy and authentication for communication and file encryption.


**Security Information and Event Management (SIEM) Tools**: These include tools like Splunk Enterprise Security, IBM QRadar, and LogRhythm, which collect and analyze security event logs from various sources to identify and respond to security incidents.


**Password Managers**: Tools like LastPass, KeePass, and Dashlane help generate and securely store complex passwords, ensuring strong password management practices.



### A learning framework for getting up to speed in cybersecurity 

Becoming an expert in cybersecurity requires a combination of theoretical knowledge and practical experience. Here's a general outline for a training program that can help you develop the necessary skills and knowledge in this field:

1. **Foundational Knowledge**: 


    Start by gaining a solid understanding of computer networks, operating systems, and programming languages like Python and C/C++. Familiarize yourself with TCP/IP protocols, network architecture, and common vulnerabilities.

    Study cryptography principles, including encryption algorithms, digital signatures, and secure key management.


2. **Cybersecurity Fundamentals**: 

    Learn the basics of cybersecurity, including threat landscape, types of attacks (e.g., malware, social engineering, phishing), risk management, and security frameworks (e.g., NIST Cybersecurity Framework).

    Familiarize yourself with legal and ethical considerations in cybersecurity, such as privacy laws, compliance regulations, and ethical hacking guidelines.


3. Network Security:

    Dive deeper into network security concepts, including firewalls, intrusion detection/prevention systems (IDS/IPS), virtual private networks (VPNs), and secure protocols (e.g., SSL/TLS).

    Understand network monitoring and analysis tools, such as Wireshark, and learn how to analyze network traffic for detecting and investigating security incidents.


    Recommended books for beginners:

    **"Computer Networks: A Systems Approach" by Larry L. Peterson and Bruce S. Davie**: This book provides a comprehensive introduction to computer networks and covers both the fundamentals and advanced concepts. It offers a practical, hands-on approach to understanding networks and includes discussions on network protocols, network architecture, network security, and more. The book also includes real-world examples and case studies to reinforce the concepts.

    **"Network Security Bible" by Eric Cole**: This book serves as a beginner's guide to network security, covering a wide range of topics. It explains fundamental concepts, such as network vulnerabilities, threats, and defense mechanisms. It also delves into practical aspects of network security, including firewalls, intrusion detection systems, VPNs, wireless security, and incident response. The book provides clear explanations, real-world examples, and practical tips for securing networks.

    Both of these books offer a solid foundation for beginners in the field of networking and network security. They are highly regarded for their clarity, depth of coverage, and practical approach. It's worth noting that the field of network security is continually evolving, so supplementing your learning with up-to-date online resources, tutorials, and practical exercises will also be beneficial.

4. System and Application Security:

    Explore secure system configuration, hardening techniques, and secure coding practices.

    Study web application security, including common vulnerabilities (e.g., SQL injection, cross-site scripting), secure development methodologies (e.g., OWASP Top Ten), and tools like Burp Suite or OWASP ZAP.

5. Threat Intelligence and Incident Response:

    Gain knowledge of threat intelligence concepts, including threat modeling, threat hunting, and security information and event management (SIEM) systems.

    Learn incident response procedures, including incident detection, containment, eradication, and recovery. Understand forensic analysis techniques and incident reporting.

6. Secure Infrastructure and Cloud Security:

    Familiarize yourself with secure infrastructure design principles, including secure network architecture, access control, and identity management.

    Gain knowledge of cloud security concepts, such as secure cloud deployment models, data encryption, access controls, and security monitoring in cloud environments.

7. Continuous Learning and Professional Certifications:

    Stay updated with the latest trends, vulnerabilities, and technologies in cybersecurity through continuous learning, attending conferences, and reading industry publications.

    Pursue relevant certifications, such as CompTIA Security+, Certified Information Systems Security Professional (CISSP), Certified Ethical Hacker (CEH), or Offensive Security Certified Professional (OSCP).


The practical experience is crucial for becoming an expert in cybersecurity. Seek opportunities for hands-on experience through internships, Capture the Flag (CTF) competitions, bug bounty programs, or by participating in open-source security projects. Additionally, joining cybersecurity communities and engaging with professionals in the field can provide valuable networking opportunities and mentorship.

### Cybersecurity Certifications

There are several highly regarded cybersecurity certifications that can help professionals enhance their skills, demonstrate their expertise, and advance their careers in the field. Here are some of the best-known cybersecurity certifications:

**Certified Information Systems Security Professional (CISSP)**: Offered by (ISC)², the CISSP certification is widely recognized and covers various domains of cybersecurity, including security and risk management, asset security, security engineering, communication and network security, and more. It is ideal for experienced professionals in cybersecurity management and leadership roles.

**Certified Ethical Hacker (CEH)**: The CEH certification, offered by the EC-Council, validates the skills and knowledge required to identify vulnerabilities and assess the security posture of systems through ethical hacking and penetration testing. It is focused on offensive security techniques and is suitable for professionals interested in ethical hacking and vulnerability assessment.

**CompTIA Security+: Security+** is a widely recognized entry-level certification that covers foundational knowledge in cybersecurity. It covers various topics such as network security, cryptography, identity management, risk management, and incident response. It is vendor-neutral and a good starting point for individuals beginning their career in cybersecurity.

**Certified Information Security Manager (CISM)**: Offered by ISACA, the CISM certification is designed for professionals involved in managing, designing, and assessing an enterprise's information security program. It covers domains such as information security governance, risk management, incident management, and program development and management.

**Certified Information Systems Auditor (CISA)**: Also offered by ISACA, the CISA certification is focused on auditing, control, and security of information systems. It is designed for professionals involved in IT auditing, control, and security roles and covers domains such as information system audit processes, governance and management, and information system acquisition, development, and implementation.

**Offensive Security Certified Professional (OSCP)**: Offered by Offensive Security, the OSCP certification emphasizes hands-on practical skills in penetration testing. It requires passing a challenging 24-hour practical exam, demonstrating real-world knowledge of attacking systems and networks.

**Certified Cloud Security Professional (CCSP)**: The CCSP certification, offered by (ISC)², focuses on cloud security principles, concepts, and best practices. It is designed for professionals responsible for managing and securing cloud environments and covers areas such as cloud architecture, data security, identity and access management, and compliance.

### Some of the best resources to stay up to date

**Websites and Blogs**:    

- The Hacker News
- Krebs on Security
- Dark Reading
- SecurityWeek
- Threatpost
- SANS Internet Storm Center (ISC)
- Schneier on Security
- The CyberWire

**Online Security Communities and Forums**:  

- Reddit's /r/netsec and /r/cybersecurity subreddits
- Stack Exchange's Information Security community
- OWASP (Open Web Application Security Project) community
- ISC2 Community forums
- DEF CON Forums

**Security Conferences and Events**:  

- Black Hat
- DEF CON
- RSA Conference
- DerbyCon
- Hack In The Box (HITB)
- InfoSec World
  

**Webinars and Online Courses**: 

- Coursera, 
- Udemy, 
- edX, 
- Cybrary

**Podcasts**:

- Security Now!
- Darknet Diaries
- Risky Business
- The CyberWire
- Smashing Security

**Research Papers and Whitepapers:** Read research papers and whitepapers published by reputable organizations, security vendors, and academic institutions. These often provide in-depth analysis of vulnerabilities, techniques, and emerging technologies.


### Additional Resources

- [NIST 0 Computer Security Resource Center](https://csrc.nist.gov/)

- [Automating Incident Response and Forensics](https://www.youtube.com/watch?v=f_EcwmmXkXk&ab_channel=AmazonWebServices)

- https://www.udemy.com/course/cissp-domain-1-2/ 

- https://www.isc2.org/Certifications/CISSP   

- https://www.forbes.com/sites/stevemorgan/2016/01/02/one-million-cybersecurity-job-openings-in-2016/2/#6874bcd17ca9   

- https://www.isc2.org/Certifications/Associate   

- https://www.isc2.org/Ethics   

- https://downloads.isc2.org/credentials/cissp/CISSP-Detailed-Content-Outline.pdf 

- https://www.wiz.io/blog/compliance-made-easy-with-wiz

- https://www.nccoe.nist.gov/publication/1800-25/

- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.1800-25.pdf
