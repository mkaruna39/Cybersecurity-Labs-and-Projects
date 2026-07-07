# Cybersecurity Executive Dashboard 

*Project Overview:*

Developed an interactive cybersecurity executive dashboard in Microsoft Power BI to monitor an organization's overall security posture through real-time security metrics and visual analytics. The dashboard consolidates data from multiple cybersecurity domains, including asset inventory, vulnerability management, incident response, risk management, and compliance reporting, enabling executives to quickly identify security trends and prioritize remediation efforts.
The project demonstrates practical experience with data modeling, dashboard design, interactive filtering, KPI development, and business intelligence reporting.

*AI-Assisted Dataset Generation*

To simulate a realistic enterprise cybersecurity environment, AI was used to generate synthetic cybersecurity datasets and a fictional company profile (TechFusion Inc.). The generated data included assets, vulnerabilities, security incidents, compliance controls, and enterprise risk records representative of those encountered in corporate cybersecurity operations.

Using AI to create synthetic datasets has some advantages:

- Generates realistic enterprise-scale cybersecurity data without exposing confidential or proprietary information.
- Produces diverse incident scenarios and vulnerability distributions for dashboard development.
- Enables rapid prototyping of cybersecurity analytics solutions.
- Simulates executive reporting environments commonly used in Security Operations Centers (SOCs) and Governance, Risk, and Compliance (GRC) teams.
- Supports the development of cybersecurity intelligence dashboards while maintaining data privacy and ethical AI practices.

After generating the datasets, they were imported into Power BI, where they were cleaned, modeled, and transformed into an interactive business intelligence solution.


***Objectives:***

The dashboard was designed to answer key cybersecurity questions such as:

How many security incidents are currently open?

Which departments have the highest security exposure?

How many critical vulnerabilities require immediate remediation?

What is the organization's current compliance score?

Which vulnerability severity levels occur most frequently?

What types of cyber incidents are most common?

What organizational risks currently require attention?


***Dataset***

The dashboard was built using multiple CSV datasets representing different areas of a fictional enterprise security program.

1) Assets- Contains organizational asset inventory including:

- Asset ID
- Asset Type
- Department
- Operating System
- Encryption Status
- Multi-Factor Authentication Status
- Patch Status

2) Vulnerabilities- Contains vulnerability scan results including:

- Vulnerability ID
- Asset ID
- CVE Identifier
- CVSS Score
- Severity
- Days Open
- Status

3) Security Incidents- Contains incident response data including:

- Incident ID
- Department
- Incident Type
- Severity
- Status
- Date
- Hours to Resolve

4) Compliance Controls- Contains security control assessment scores including:

- Control Name
- Compliance Score

5) Enterprise Risks- Contains organizational cybersecurity risks including:

- Risk
- Impact
- Likelihood
- Risk Score
- Status


*Relationships* (This allows filters to apply correctly across the dashboard while maintaining a clean and scalable data model)

Assets → Vulnerabilities (AssetID)

Departments → Assets

Departments → Incidents


Dashboard Features (The dashboard displays key security metrics at the top for quick executive reporting)

*Executive KPI Cards*

![GRC Dashboard Screenshot](https://github.com/user-attachments/assets/0cfdae55-76e5-4c84-a22e-9ff3a00e9431)
*Figure 1 — Executive dashboard screenshot showing KPI cards and vulnerability overview.*



Metrics include:

- Open Incidents
- Compliance Score
- Open Risks
- Critical Vulnerabilities
- Total Assets
- Total Vulnerabilities

These KPIs provide an immediate overview of the organization's cybersecurity posture.

*Vulnerabilities by Severity*

A column chart summarizes vulnerabilities by severity level:

- Critical
- High
- Medium
- Low

![Incident Breakdown Chart](https://github.com/user-attachments/assets/363fb1bd-a3d2-4beb-b305-6cbf84301d3f)

*Figure 2 — Incident breakdown visualization showing distribution across departments and severity levels.*




This visualization helps security teams prioritize remediation efforts based on risk

*Incidents by Type*

![Risk Matrix Visualization](https://github.com/user-attachments/assets/f677ffa5-741b-4c5a-87b6-1e55af968324)

*Figure 3 — Enterprise risk matrix showing impact vs. likelihood distribution.*



A donut chart categorizes security incidents into:

- Ransomware
- Phishing
- Lost Device
- Insider Threat
- Unauthorized Access
- Malware

This enables quick identification of the most common attack vectors affecting the organization.

*Compliance Control Scores*

A horizontal bar chart displays compliance scores across major cybersecurity controls

![Compliance Score Visualization](https://github.com/user-attachments/assets/2f2fc6d9-7599-48e2-abfa-935ca24e23fe)

*Figure 4— Compliance score distribution showing control effectiveness across the organization.*


This allows management to compare control effectiveness across the organization.

*Enterprise Risk Register*

A detailed table summarizes organizational cybersecurity risks including:

- Risk
- Impact
- Status
- Risk Score
- Likelihood

![Asset Inventory Visualization](https://github.com/user-attachments/assets/87d5dc80-2c73-4058-b2ff-968f4da0ac26)

*Figure 5 — Asset inventory overview showing distribution by department and asset type.*


The table enables analysts to review current enterprise risks and prioritize mitigation activities.

***Interactive Dashboard Features***

One of the primary goals of this project was to create a fully interactive executive dashboard.

*Department Filter*- Users can filter the dashboard by organizational department.

Available departments include:

- Finance
- HR
- IT
- Sales

Selecting a department dynamically updates connected visualizations, allowing users to analyze security metrics for individual business units.

![Dashboard Overview Screenshot](https://github.com/user-attachments/assets/0569d330-e321-4adf-888a-93e79161271d)
*Figure 6— Full dashboard overview showing KPIs, charts, and security posture analytics.*


Examples include:

- Total Assets
- Total Vulnerabilities
- Critical Vulnerabilities
- Vulnerabilities by Severity
- Incidents by Type
- Open Incidents

This provides department-level visibility into security posture and operational risk. In addition, filtering by department in the visualisations tab updates the charts to show the vulnerabilities by severity as well as the incidents by type within that department.
![Full Dashboard Screenshot](https://github.com/user-attachments/assets/0495c4aa-25ba-43fb-afd7-71b56228abc9)
*Figure 7 — Full Power BI dashboard view showing updated visualizations, filtered by the HR department*


*Vulnerability Severity Filter*

Users can also filter the dashboard using vulnerability severity.
![Dashboard Screenshot](https://github.com/user-attachments/assets/c316837d-a53e-4845-8328-8805f5d31949)
*Figure 8 — Detailed dashboard view showing incident trends, vulnerability metrics, and compliance analytics.*



Available options include:

- Critical
- High
- Medium
- Low

Selecting a severity level updates vulnerability-related visualizations, allowing security teams to focus on high-risk findings or investigate specific categories of vulnerabilities.

Example Interactive Analysis

For example, selecting the IT department filters the dashboard to display only IT assets, vulnerabilities, and incidents.

Security analysts can immediately observe:

- Number of IT assets
- Total vulnerabilities affecting IT systems
- Distribution of vulnerability severity
- Most common incident types occurring within IT
- Number of critical vulnerabilities requiring remediation

Similarly, selecting Critical in the severity slicer narrows the dashboard to only critical vulnerabilities, making it easier to prioritize urgent security issues.

Power BI Development Process

The project followed a structured BI development workflow:

1) Data Preparation
2) Imported multiple CSV datasets into Power BI Desktop.
3) Verified data types for dates, numeric values, and categorical fields.
4) Cleaned and standardized fields used for filtering and relationships.
5) Data Modeling
6) Created relationships between datasets using primary and foreign keys.
7) Built a centralized Department dimension to support interactive filtering.
8) Configured relationship directions to enable filter propagation across visuals.
9) Dashboard Design

Designed an executive-friendly layout emphasizing readability and rapid decision-making through:

1) KPI cards
2) Interactive slicers
3) Column charts
4) Donut charts
5) Horizontal bar charts
6) Tabular risk reporting
7) Interactive Filtering

Configured slicers and visual interactions so users can explore cybersecurity metrics by:

- Department
- Vulnerability Severity

allowing multiple views of the organization's security posture without changing reports.


Business Impact- This dashboard enables security leadership to quickly monitor organizational cybersecurity performance, identify high-risk departments, analyze incident trends, evaluate compliance, and prioritize remediation efforts through an intuitive, interactive reporting interface.

*AI in Cyber Threat Intelligence*

This project demonstrates how AI can accelerate cybersecurity intelligence workflows by assisting in the creation of realistic security data that can be analyzed through business intelligence platforms.

Potential real-world applications include:

- Security Operations Center (SOC) dashboards
- Vulnerability management reporting
- Executive cyber risk reporting
- Incident trend analysis
- Compliance monitoring
- Threat intelligence visualization
- Security Key Performance Indicator (KPI) reporting

By combining AI-generated synthetic security data with Power BI analytics, organizations can prototype dashboards, test detection strategies, and train analysts without relying on sensitive production data.
