# NIST CSF 2.0 Cybersecurity Risk Assessment & Enterprise Risk Register

**Project Overview:**
This project demonstrates an end-to-end enterprise cybersecurity risk assessment conducted for a fictional organization, TechFusion Inc., using the NIST Cybersecurity Framework (CSF) 2.0 as the primary risk management framework. The objective was to identify critical organizational assets, evaluate cybersecurity threats and vulnerabilities, calculate organizational risk, recommend mitigation strategies, and communicate findings through executive reporting and data visualization.
The assessment simulates the responsibilities of a Cybersecurity Risk Analyst or GRC Analyst by following a structured risk management methodology commonly used in enterprise environments. Deliverables include an asset inventory, vulnerability assessment, enterprise risk register, NIST CSF mapping matrix, risk treatment plan, executive report, and Tableau dashboard.
Although the organization and data are fictional, the methodology, documentation, and reporting approach reflect industry practices used by cybersecurity consulting firms and internal security teams.

**Objectives**
The primary goals of this project were to:
- Conduct a cybersecurity risk assessment aligned with the NIST Cybersecurity Framework (CSF) 2.0.
- Develop a comprehensive enterprise asset inventory.
- Identify realistic cybersecurity threats and vulnerabilities.
- Evaluate organizational risk using a qualitative likelihood and impact model.
- Prioritize cybersecurity risks using a structured risk register.
- Map identified risks to the six NIST CSF 2.0 Functions.
- Recommend practical risk treatment strategies
- Create executive-level reporting suitable for business stakeholders.
- Visualize organizational cyber risk using Tableau

**Project Deliverables**
- Executive Cybersecurity Risk Assessment Report
- Asset Inventory
- Threat Catalog
- Vulnerability Assessment
- Enterprise Risk Register
- Business Impact Assessment
- Risk Treatment Plan
- NIST CSF 2.0 Mapping Matrix
- Risk Scoring Matrix
- Control Inventory
- Interactive Tableau Dashboard

***Enterprise Environment***

**Organization Profile**
Industry: Technology Consulting

Employees: Approximately 500

Hybrid Workforce

Three Office Locations

Cloud Infrastructure

**Technology Environment**

- Microsoft 365
- Microsoft Azure
- Active Directory
- Windows 11 Endpoints
- VPN
- Endpoint Detection & Response (EDR)
- Email Security Gateway
- Customer Database
- Financial ERP
- Human Resources Information System

This environment was intentionally designed to resemble the infrastructure commonly found within mid-sized organizations.

**Methodology**
The assessment followed a strict risk management process.

1) Asset Identification
   
Critical organizational assets were identified and documented.

Examples included:

- Active Directory
- Microsoft 365
- Customer Database
- Employee Workstations
- VPN Infrastructure
- Firewalls
- Email Systems
- Backup Servers

Each asset was assigned:

- Asset ID
- Department
- Owner
- Criticality
- Operating System
- Data Classification

2) Threat Identification

Potential threats capable of impacting organizational operations were identified.

Examples included:

- Phishing
- Business Email Compromise
- Credential Theft
- Ransomware
- Insider Threat
- Malware
- SQL Injection
- DDoS
- Cloud Misconfiguration
- Supply Chain Attacks

Threat selection was based on common attack techniques affecting modern enterprise environments.

3) Vulnerability Assessment

Each threat was paired with realistic organizational vulnerabilities.

Examples included:

- Weak password policies
- Missing security patches
- Lack of security awareness training
- Excessive user permissions
- Cloud configuration errors
- Legacy software
- Unencrypted sensitive information
- Limited monitoring capabilities

The relationship between assets, threats, and vulnerabilities formed the basis for the enterprise risk assessment.

4) Risk Analysis

Each identified cybersecurity risk was evaluated using a qualitative likelihood and impact methodology.

*Likelihood Scale*

Score	Description

1- Rare
2- Unlikely
3- Possible
4- Likely
5- Almost Certain

*Impact Scale*

Score	Description

1- Negligible
2- Minor
3- Moderate
4	Major
5- Critical

Risk scores were calculated using:

Risk Score = Likelihood × Impact

This methodology allowed risks to be prioritized according to potential business impact.

5) Risk Register Development
A centralized Enterprise Risk Register was developed to document identified cybersecurity risks.

Each record included:

- Risk ID
- Asset
- Threat
- Vulnerability
- Likelihood
- Impact
- Risk Score
- Risk Owner
- Treatment Strategy
- Status

The Risk Register provides a prioritized view of cybersecurity risks requiring remediation.

6) NIST CSF 2.0 Alignment

Every identified risk was mapped to one or more NIST Cybersecurity Framework Functions.

- Govern
- Identify
- Protect
- Detect
- Respond
- Recover

This mapping demonstrates how enterprise risks align with industry-recognized cybersecurity practices and supports prioritization of security initiatives.

7) Risk Treatment Planning

Each risk was assigned one of four treatment strategies:

- Mitigate
- Transfer
- Accept
- Avoid

Recommendations included implementing technical, administrative, and procedural controls designed to reduce organizational risk while supporting business objectives.

**How AI Was Used**

Artificial intelligence was used to accelerate project development, generate realistic synthetic data, and support cybersecurity analysis. AI served as a productivity tool rather than replacing the risk assessment process.

AI-assisted tasks included:

- Designing a realistic fictional enterprise environment.
- Generating synthetic asset inventories, threat catalogs, vulnerabilities, and risk records.
- Creating internally consistent datasets suitable for analysis.
- Drafting executive report sections and documentation.
- Brainstorming mitigation strategies aligned with cybersecurity best practices.
- Structuring project documentation and GitHub content.

All AI-generated information was reviewed for consistency and adapted to align with the project's objectives and the NIST Cybersecurity Framework.

Using AI to create synthetic datasets enabled the development of a realistic portfolio project without relying on proprietary or sensitive organizational data. This reflects a growing trend in cybersecurity where AI assists analysts with documentation, data generation, reporting, and cyber threat intelligence while requiring human oversight for validation and decision-making.


**Key Findings**

The cybersecurity risk assessment evaluated 50 enterprise assets supporting core business operations across the Information Technology, Finance, Human Resources, and Sales departments. The asset inventory included servers, databases, cloud applications, network infrastructure, and business-critical systems. Based on the asset inventory:

- 50 enterprise assets were assessed.
- 16 assets (32%) belonged to the Information Technology department.
- 13 assets (26%) supported Finance.
- 11 assets (22%) supported Sales.
- 10 assets (20%) supported Human Resources.

Asset criticality was also assessed to prioritize systems requiring the greatest protection:

- 20 High Criticality assets (40%)
- 15 Critical assets (30%)
- 15 Medium Criticality assets (30%)

This distribution indicates that 70% of organizational assets were classified as High or Critical, emphasizing the importance of implementing strong preventive and detective security controls.

*Vulnerability Assessment Results*

The assessment identified 60 vulnerabilities across the enterprise environment.

Severity analysis showed:

- 16 Critical vulnerabilities (26.7%)
- 9 High vulnerabilities (15.0%)
- 22 Medium vulnerabilities (36.7%)
- 13 Low vulnerabilities (21.6%)

Overall, 25 of the 60 vulnerabilities (41.7%) were classified as High or Critical, indicating several issues that should be prioritized for remediation because of their potential impact on business operations.

All identified vulnerabilities remained in an Open status within the assessment dataset, representing opportunities for future mitigation activities and demonstrating the importance of continuous vulnerability management.

*Enterprise Risk Assessment Results*

Using the identified assets, threats, and vulnerabilities, the assessment produced an Enterprise Risk Register containing 40 cybersecurity risks.

Each risk was evaluated using a qualitative Likelihood × Impact methodology.

Risk Prioritization:

- 10 Critical Risks (25%)
- 7 High Risks (17.5%)
- 17 Medium Risks (42.5%)
- 6 Low Risks (15%)

The average enterprise risk score was 13.3 out of 25, indicating an overall moderate risk posture with several high-priority risks requiring immediate attention.

The highest calculated risk score was 25, representing risks with both a very high likelihood of occurrence and a critical business impact.

NIST CSF 2.0 Alignment

Each identified risk was mapped to one of the six NIST Cybersecurity Framework (CSF) 2.0 Functions.

NIST CSF Function	Risks Mapped
Protect	8

Respond	8

Identify	8

Recover	7

Govern	5

Detect	4

The results show that the majority of identified risks fall within the Protect, Respond, and Identify functions. This suggests that strengthening preventive security controls, improving incident response capabilities, and enhancing asset and risk identification processes would have the greatest impact on reducing organizational cyber risk.

*Overall Assessment*

The assessment identified a combination of high-value assets, open vulnerabilities, and prioritized enterprise risks that could significantly affect business operations if left unaddressed.

Key observations include:

- 50 enterprise assets were assessed across four business departments.
- 60 vulnerabilities were identified, with 41.7% rated High or Critical.
- 40 enterprise cybersecurity risks were documented.
- 17 risks (42.5%) were classified as High or Critical.
- The average enterprise risk score was 13.3/25.
- Risks were successfully mapped across all six NIST CSF 2.0 Functions, supporting a structured and standards-based approach to cybersecurity risk management.
