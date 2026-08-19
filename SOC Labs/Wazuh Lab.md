# Wazuh SIEM, Endpoint Monitoring & Vulnerability Management Lab

## Project Overview

This project involved hands-on investigation of **Wazuh**, an open-source security platform that combines endpoint monitoring, security event management, vulnerability detection, configuration assessment, file integrity monitoring, and compliance visibility.

The lab was completed in a virtualized environment containing a Wazuh management server and two monitored endpoints:

* **Windows Server**
* **Linux Server**

The primary objective was to gain practical experience using Wazuh from both a **SOC analyst** and **security/compliance** perspective.

During the lab, I investigated endpoint telemetry, vulnerabilities, security configurations, authentication logs, detection rules, dashboards, Sysmon events, File Integrity Monitoring, and automated response capabilities.

---

# What Is Wazuh?

Wazuh is an open-source security platform used for:

* Security event monitoring
* Endpoint detection and monitoring
* Vulnerability detection
* Configuration assessment
* File Integrity Monitoring (FIM)
* Compliance monitoring
* Security dashboards
* MITRE ATT&CK mapping
* Cloud security monitoring
* Active Response
* OSQuery integration
* YARA and VirusTotal integration

Wazuh originally focused heavily on host-based intrusion detection and endpoint monitoring but has evolved into a broader security platform that can provide many SIEM and security operations capabilities.

---

# Lab Environment

The virtual lab contained the following components:

| Component               | Purpose                                                                         |
| ----------------------- | ------------------------------------------------------------------------------- |
| Wazuh Management Server | Central security monitoring and management platform                             |
| `WIN-SERVER`            | Windows endpoint monitored by Wazuh                                             |
| `linux-server`          | Linux endpoint monitored by Wazuh                                               |
| Wazuh Agents            | Collect endpoint telemetry and send data to the Wazuh server                    |
| Wazuh Dashboard         | Interface for investigating events, vulnerabilities, configurations, and alerts |

The lab provided a preconfigured Wazuh environment, allowing the focus to remain on **security monitoring, investigation, vulnerability management, and detection concepts**.

---

# 1. Accessing the Wazuh Environment

After starting the Wazuh management server, I allowed the environment several minutes to initialize before accessing the Wazuh web interface.

After authentication, the Wazuh **Overview** page displayed the available agents.

The lab contained:

* One Windows endpoint
* One Linux endpoint

The agents appeared as **disconnected**, which was expected for the provided lab environment.

## What I Learned

This introduced the basic Wazuh architecture:

```text
Endpoint
   |
   | Wazuh Agent
   v
Wazuh Management Server
   |
   v
Wazuh Dashboard
   |
   +--> Events
   +--> Vulnerabilities
   +--> Configuration Assessment
   +--> IT Hygiene
   +--> Alerts
   +--> Dashboards
```

The Wazuh agent acts as the endpoint telemetry collection component, while the Wazuh server centrally processes and analyzes the information.

---

# 2. Investigating Wazuh Agents

From the **Agents** section, I investigated the Windows and Linux endpoints and explored their available security information.

Wazuh agents can monitor:

* Authentication activity
* Operating system activity
* Installed software
* Network configuration
* User accounts
* Security configuration
* File changes
* Vulnerabilities
* Security events

Wazuh supports agents for operating systems including:

* Windows
* Linux
* macOS

## What I Learned

Centralized endpoint monitoring allows security teams to collect information from multiple systems without manually inspecting every endpoint.

This is especially useful in a SOC environment where analysts need a centralized view of endpoint activity.

---

# 3. IT Hygiene Investigation

I selected the `WIN-SERVER` endpoint and investigated the **IT Hygiene** information.

The IT Hygiene functionality provided several categories of endpoint information.

## System

The System section provided information such as:

* Hostname
* Operating system
* Hardware information
* Network interfaces
* System details

## Software

The Software section provided visibility into:

* Installed applications
* Operating system packages
* Software versions
* Browser extensions
* Installed components

## Network

The Network section provided information about network exposure, including listening services and ports.

For example, exposed services such as **RDP** can be important from an attack-surface management perspective.

## Identity

The Identity section provided information about local users and their privileges.

## Security Value

This information can support:

* IT asset management
* Attack-surface management
* Vulnerability management
* Incident response
* Security investigations

A simplified view of endpoint visibility is:

```text
Asset
 |
 +--> Operating System
 +--> Installed Software
 +--> Network Exposure
 +--> Local Accounts
 +--> Privileges
```

## What I Learned

IT Hygiene provides security teams with centralized visibility into the actual state of an endpoint.

This information can help analysts understand **what the asset is, what is installed on it, what services are exposed, and who has access to it**.

---

# 4. CIS Configuration Assessment

I investigated Wazuh's **Configuration Assessment** functionality for the Windows endpoint.

Wazuh can evaluate endpoint configurations against security benchmarks such as **CIS Benchmarks**.

The Windows endpoint performed hundreds of configuration checks, including checks implemented through PowerShell and other system inspection mechanisms.

These checks can evaluate areas such as:

* Password policies
* Account configuration
* Security settings
* Operating system configuration
* Access controls
* System hardening

## Why Configuration Assessment Matters

Configuration weaknesses can create security exposure even when no software vulnerability exists.

For example:

```text
Vulnerable Software
        |
        v
Technical Vulnerability
        |
        v
Potential Exploitation
```

versus:

```text
Weak Configuration
        |
        v
Security Misconfiguration
        |
        v
Potential Attack Path
```

## What I Learned

I learned that **vulnerability management and configuration management are related but separate security disciplines**.

A system can have fully patched software and still be insecure because of weak configurations or missing security controls.

---

# 5. Vulnerability Detection

I investigated Wazuh's **Vulnerability Detection** capability using the Linux endpoint.

The vulnerability inventory identified a large number of vulnerabilities in the environment, including critical-severity findings.

The exact number can vary depending on the state of the lab environment and vulnerability database updates.

The vulnerability interface allowed findings to be investigated and filtered using information such as:

* CVE
* Package
* Description
* Timestamp
* Severity
* Affected agent

I also investigated package-specific vulnerability information, including Linux kernel-related packages.

## Vulnerability Investigation Workflow

```text
Vulnerability Detected
        |
        v
Identify CVE
        |
        v
Identify Affected Package
        |
        v
Determine Severity
        |
        v
Determine Exposure
        |
        v
Prioritize Remediation
        |
        v
Patch / Mitigate
        |
        v
Reassess
```

## Important Observation

One of the most important lessons from the vulnerability module was that **the number of detected vulnerabilities should not automatically be treated as the number of immediately exploitable threats**.

A vulnerability management team should consider:

* Severity
* Exploit availability
* Asset criticality
* Network exposure
* Business impact
* Existing mitigations
* Whether the vulnerable software is actually being used
* Whether the affected service is externally accessible

This demonstrates the difference between **vulnerability identification** and **risk-based vulnerability management**.

---

# 6. Investigating Security Logs

I investigated Wazuh's log analysis capabilities using the **Discover** interface.

The Discover page provides access to events collected and processed by Wazuh.

I investigated Linux authentication activity using the following search query:

```text
decoder.name: sshd
```

I then examined the resulting events and expanded individual records to inspect their available fields.

## Investigation Workflow

```text
Raw Endpoint Activity
        |
        v
Wazuh Agent
        |
        v
Log Collection
        |
        v
Decoder
        |
        v
Rule Processing
        |
        v
Alert
        |
        v
Dashboard / Investigation
```

## What I Learned

This demonstrated how endpoint telemetry can move through a security monitoring pipeline.

A SOC analyst can use centralized logs to investigate authentication activity and identify potentially suspicious behavior.

---

# 7. Logs vs. Rules vs. Alerts

The lab demonstrated an important distinction between **events, rules, and alerts**.

Wazuh receives endpoint data and uses decoders to understand the structure of incoming events.

Rules then determine whether an event should generate a security alert.

Wazuh rules use severity levels ranging from **0 to 15**, with higher levels representing more significant events.

The basic process is:

```text
Endpoint Event
      |
      v
   Decoder
      |
      v
Parsed Event
      |
      v
    Rule
      |
      v
Security Alert
```

## What I Learned

A SIEM is not simply a database of logs.

The security value comes from:

1. Collecting telemetry
2. Parsing telemetry
3. Identifying meaningful patterns
4. Applying detection logic
5. Generating alerts
6. Investigating the resulting activity

This was an important foundation for understanding how security monitoring platforms operate.

---

# 8. Custom Security Visualization

I explored Wazuh's visualization capabilities by creating a visualization using the `wazuh-alerts-*` data source.

The visualization grouped alerts by:

```text
agent.name
```

This provides a way to visualize which endpoints are generating security events.

Multiple visualizations can then be combined into a dashboard.

## Example SOC Dashboard Metrics

A production SOC dashboard could include:

* Total alerts
* Critical alerts
* Alerts by severity
* Alerts by endpoint
* Alerts by detection rule
* Authentication failures
* Vulnerabilities by severity
* Top affected assets
* Configuration assessment results

## What I Learned

Visualization helps analysts identify trends and outliers that may be difficult to recognize when looking only at raw event data.

Dashboards can help identify:

* High-risk endpoints
* Repeated detections
* Alert trends
* Changes over time
* Unusual activity

---

# 9. Agent Groups and Centralized Configuration

I explored the **Agent Groups** functionality.

Agent groups allow organizations to apply common configurations to multiple endpoints.

For example:

```text
CORP
 |
 +--> Corporate Laptops
 +--> Corporate Desktops
 +--> Corporate Servers

BYOD
 |
 +--> Personal Devices
```

Different groups can have different logging and monitoring requirements.

The lab's default group already included custom log collection settings for:

* Linux authentication logs
* Microsoft Defender event logs on Windows

## Why This Matters

Centralized configuration becomes increasingly important as an organization grows.

Instead of manually configuring every endpoint:

```text
Wazuh Group
     |
     +--> Agent 1
     +--> Agent 2
     +--> Agent 3
     +--> Agent 4
     +--> Agent 5
```

security policies can be managed centrally and propagated to the appropriate agents.

---

# 10. Sysmon Monitoring

I investigated how Wazuh can be configured to collect **Microsoft Sysmon** events.

A Windows agent can be configured to monitor the Sysmon operational event channel.

Example configuration:

```xml
<localfile os="Windows">
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

Sysmon provides valuable endpoint telemetry such as:

* Process creation
* Command-line execution
* Parent-child process relationships
* File hashes
* Network connections
* Process metadata

## What I Learned

Sysmon can provide additional endpoint telemetry that improves detection capabilities.

For SOC analysts, process creation and parent-child process relationships are particularly valuable when investigating suspicious execution.

---

# 11. Wazuh Decoders

I investigated the role of **decoders** in the Wazuh detection pipeline.

Different operating systems and applications produce logs in different formats.

A decoder tells Wazuh how to interpret those raw events.

Conceptually:

```text
Raw Log
   |
   v
Decoder
   |
   +--> Event Type
   +--> Username
   +--> Process
   +--> Hash
   +--> Parent Process
   |
   v
Structured Event
```

For Sysmon events, decoders can extract fields such as:

* Process image
* Username
* File hash
* Parent process

## What I Learned

Decoders are important because detection rules become more useful when raw log data is converted into structured fields.

This is a key concept in SIEM and detection engineering.

---

# 12. Wazuh Detection Rules

I investigated Wazuh rules used to turn parsed events into meaningful security alerts.

One example from the lab demonstrated detection of **PowerShell execution**.

A lower-level rule identified Sysmon process creation events, while another rule matched PowerShell execution and generated a higher-severity alert.

Conceptually:

```text
Sysmon Process Creation
        |
        v
Event Detection Rule
        |
        v
PowerShell Process?
        |
       YES
        |
        v
High-Severity Alert
```

## Why This Is Important

PowerShell is a legitimate administrative tool, but it can also be abused by attackers.

Therefore, PowerShell execution can provide valuable telemetry for a SOC.

However, simply detecting `powershell.exe` does not automatically mean malicious activity occurred.

Useful investigation context includes:

* Executing user
* Parent process
* Command line
* Host
* Time
* Script content
* Network activity
* Process ancestry

## What I Learned

Effective detection requires **context**, not simply matching a single event.

This reinforced the importance of balancing detection coverage with false-positive reduction.

---

# 13. Alerting and Notifications

I explored Wazuh's alerting functionality.

A security team can create alert queries based on severity or other event fields.

For example:

```text
rule.level >= 12
```

can be used to identify higher-severity events.

Notifications can then be integrated with external communication channels such as email or collaboration platforms.

## SOC Alert Workflow

```text
Security Event
      |
      v
Detection Rule
      |
      v
Severity Assessment
      |
      v
Alert
      |
      v
Notification
      |
      v
SOC Analyst Investigation
      |
      v
Response
```

## What I Learned

This demonstrated how Wazuh can become part of an operational SOC workflow rather than simply being used as a log viewer.

---

# 14. File Integrity Monitoring

I explored **File Integrity Monitoring (FIM)**.

FIM allows security teams to monitor important files and directories for unauthorized changes.

Potential monitoring targets include:

* SSH configuration
* Application configuration
* Security configuration
* VPN configuration
* Sensitive files
* Windows registry keys

A simplified FIM workflow is:

```text
File / Registry
      |
      v
Baseline
      |
      v
Change Detected
      |
      v
Wazuh Event
      |
      v
Security Alert
      |
      v
Investigation
```

## Security and Compliance Value

FIM can help organizations monitor sensitive system configurations and detect unauthorized modifications.

This makes FIM useful for both:

* Security operations
* Compliance monitoring

---

# 15. Malware Detection with YARA and VirusTotal

I investigated how Wazuh can extend FIM with malware detection capabilities.

When monitored files are created or modified, Wazuh can integrate with:

* **YARA**
* **VirusTotal**

YARA can be used to identify files matching known malware characteristics or custom detection rules.

VirusTotal can provide additional reputation information based on file hashes.

Conceptually:

```text
File Modified
      |
      v
Wazuh FIM
      |
      +---------> YARA
      |
      +---------> VirusTotal
      |
      v
Detection Result
      |
      v
Alert / Active Response
```

## What I Learned

This demonstrated how endpoint monitoring can be combined with malware detection and threat intelligence capabilities.

---

# 16. Active Response

I explored Wazuh's **Active Response** capability.

Depending on the configured response, Wazuh can execute actions on an endpoint after a detection.

Potential responses include:

* Running a script
* Blocking a network connection
* Removing or isolating a file
* Performing automated remediation

A simplified workflow is:

```text
Threat Detected
      |
      v
Wazuh Rule
      |
      v
Alert
      |
      v
Active Response
      |
      v
Endpoint Action
```

## Important Lesson

Active Response can support automated containment, but it should not automatically be treated as a complete replacement for a dedicated EDR platform.

Automated responses also need to be carefully designed because overly aggressive actions can disrupt legitimate business activity.

---

# 17. Additional Wazuh Capabilities

The lab also introduced several capabilities that could be explored further.

## Cloud Security

Wazuh can collect security information from cloud services and platforms including:

* AWS
* Microsoft Azure
* Google Cloud
* Microsoft 365
* GitHub

## Compliance Mapping

Wazuh can map security events and rules to frameworks and standards such as:

* MITRE ATT&CK
* GDPR
* PCI DSS
* Other compliance requirements

## OSQuery

Wazuh can integrate with OSQuery to perform structured endpoint queries.

This can be useful for custom SOC investigations and IT hygiene checks.

## Agentless Monitoring

Wazuh can perform certain monitoring functions without a traditional Wazuh agent, including FIM over SSH.

This can be useful for infrastructure where installing an endpoint agent is impractical.

---

# Hands-On Work Completed

The following activities were completed or investigated inside the Wazuh virtual environment:

* [x] Accessed the Wazuh management dashboard
* [x] Investigated the Windows and Linux agents
* [x] Explored endpoint agent information
* [x] Investigated IT Hygiene data
* [x] Reviewed system information
* [x] Reviewed installed software
* [x] Reviewed network information and exposed services
* [x] Reviewed local identities and privileges
* [x] Investigated CIS Configuration Assessment
* [x] Reviewed endpoint security configuration findings
* [x] Investigated vulnerability detection
* [x] Filtered vulnerability information by package
* [x] Investigated Linux authentication events
* [x] Used the Discover interface to search security telemetry
* [x] Investigated Wazuh rules and alert severity
* [x] Created and explored a custom security visualization
* [x] Investigated agent groups
* [x] Examined custom log collection configuration
* [x] Investigated Sysmon event collection
* [x] Learned how Wazuh decoders process events
* [x] Investigated Wazuh detection rules
* [x] Examined PowerShell detection logic
* [x] Investigated alerting and notification capabilities
* [x] Explored File Integrity Monitoring
* [x] Explored YARA and VirusTotal integrations
* [x] Explored Active Response
* [x] Reviewed Wazuh compliance and cloud-security capabilities

---

# Key Skills Demonstrated

## Security Operations

* SIEM monitoring
* Alert investigation
* Security event analysis
* Endpoint telemetry analysis
* Detection engineering concepts
* Security dashboards

## Vulnerability Management

* Vulnerability identification
* CVE investigation
* Severity analysis
* Package-level vulnerability analysis
* Vulnerability prioritization

## Endpoint Security

* Endpoint monitoring
* IT asset visibility
* Process monitoring
* Sysmon telemetry
* File Integrity Monitoring
* Configuration assessment

## Detection Engineering

* Log collection
* Decoders
* Detection rules
* Rule severity
* PowerShell detection
* Alert generation

## Compliance / GRC

* CIS benchmark assessment
* Configuration compliance
* Security monitoring
* FIM for compliance
* Compliance framework mapping

---

# Key Lessons Learned

## 1. Visibility Comes Before Detection

A SOC cannot effectively detect threats without sufficient endpoint telemetry.

The lab demonstrated the progression:

```text
Asset
  ↓
Telemetry
  ↓
Log Collection
  ↓
Parsing
  ↓
Detection
  ↓
Alert
  ↓
Investigation
  ↓
Response
```

---

## 2. Vulnerability Counts Require Context

A large vulnerability count does not necessarily mean an organization has an equally large number of immediately exploitable threats.

Risk prioritization should consider:

* Severity
* Exploitability
* Asset importance
* Exposure
* Business impact
* Existing controls

This is a key difference between simply **scanning for vulnerabilities** and performing **risk-based vulnerability management**.

---

## 3. Configuration Security Is Different From Vulnerability Management

CIS configuration assessment demonstrated that a system can be insecure even when installed software is patched.

Security teams therefore need both:

```text
Vulnerability Management
+
Configuration Management
```

---

## 4. Raw Logs Are Not Enough

The decoder and rule workflow demonstrated how raw events become useful security detections.

```text
Raw Event
   ↓
Decoder
   ↓
Structured Data
   ↓
Detection Rule
   ↓
Alert
```

This is an important foundation for understanding SIEM and detection engineering.

---

## 5. Detection Requires Context

A PowerShell event alone does not automatically indicate malicious activity.

Effective detection requires additional context such as:

* User
* Host
* Command line
* Parent process
* Process ancestry
* Timing
* Network activity

This reinforced the importance of reducing false positives while maintaining useful detection coverage.

---

## 6. Security Monitoring Supports Compliance

Several Wazuh features have both security and compliance applications.

| Wazuh Capability        | Security Use                   | Compliance Use                    |
| ----------------------- | ------------------------------ | --------------------------------- |
| Vulnerability Detection | Identify vulnerable software   | Vulnerability management evidence |
| CIS Assessment          | Detect insecure configurations | Security control assessment       |
| FIM                     | Detect unauthorized changes    | Change monitoring                 |
| Log Monitoring          | Detect suspicious activity     | Audit logging                     |
| IT Hygiene              | Asset visibility               | Asset inventory                   |
| Alerting                | Incident detection             | Monitoring evidence               |

---

# SOC Analyst Perspective

From a SOC analyst perspective, this lab demonstrated a realistic security monitoring workflow.

An analyst could begin with a high-severity alert and investigate:

```text
Alert
 ↓
Affected Endpoint
 ↓
User
 ↓
Process
 ↓
Command Line
 ↓
Parent Process
 ↓
Network Activity
 ↓
Related Events
 ↓
Determine Benign vs. Suspicious
 ↓
Escalate / Respond
```

This is more representative of SOC work than simply viewing a list of alerts.

The lab demonstrated how analysts can move from an initial detection toward contextual investigation.

---

# GRC / Compliance Perspective

The lab also demonstrated how endpoint security data can support governance and compliance activities.

For example, an organization could use Wazuh to help answer questions such as:

* Which endpoints are being monitored?
* Which systems have vulnerabilities?
* Which systems fail configuration benchmarks?
* Which software is installed?
* Which accounts exist on endpoints?
* Are important files being monitored?
* Are security events being collected?
* Are security controls producing evidence?

This makes Wazuh useful not only for SOC operations but also for **security governance and compliance monitoring**.

---

# Overall Project Outcome

This lab provided hands-on experience with Wazuh as an integrated security monitoring platform.

Rather than only learning Wazuh terminology, I interacted with the platform's major security capabilities and investigated how endpoint telemetry moves through the monitoring pipeline.

The lab strengthened my understanding of:

* SIEM architecture
* Endpoint monitoring
* Vulnerability management
* Configuration assessment
* CIS benchmarks
* Security logging
* Detection rules
* Decoders
* Sysmon
* PowerShell monitoring
* Alerting
* File Integrity Monitoring
* Malware detection integrations
* Automated response
* Security dashboards
* Compliance monitoring

The most valuable takeaway was understanding how these individual capabilities work together to form a practical security monitoring workflow:

```text
             ┌─────────────────────┐
             │      Endpoints      │
             │ Windows + Linux     │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │   Wazuh Agents      │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ Log Collection      │
             │ IT Hygiene          │
             │ Vulnerabilities     │
             │ FIM                 │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ Decoders            │
             │ Parse Events        │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ Detection Rules     │
             │ Severity 0–15       │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ Alerts & Dashboards │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ SOC Investigation   │
             │ & Response          │
             └─────────────────────┘
```

---

# Resume-Ready Project Description

**Wazuh SIEM & Endpoint Security Lab — Security Operations / Vulnerability Management**

* Deployed and investigated a Wazuh security monitoring environment containing Windows and Linux endpoints, analyzing endpoint telemetry, authentication events, vulnerabilities, software inventories, network exposure, and local identities.
* Performed vulnerability and configuration assessments using Wazuh's vulnerability detection and CIS benchmark capabilities, developing an understanding of risk-based vulnerability prioritization and security hardening.
* Investigated Wazuh's SIEM pipeline by analyzing log collection, decoders, detection rules, alert severity, Sysmon telemetry, and PowerShell execution detections.
* Built and explored security visualizations and dashboards to analyze alert activity across monitored endpoints.
* Evaluated File Integrity Monitoring, YARA/VirusTotal integrations, Active Response, centralized agent configuration, and compliance-monitoring capabilities.

---

# Technologies & Concepts

**Tools:** Wazuh, Wazuh Dashboard, Sysmon, PowerShell, YARA, VirusTotal

**Operating Systems:** Windows, Linux

**Security Concepts:** SIEM, HIDS, EDR, Vulnerability Management, Configuration Assessment, CIS Benchmarks, FIM, Detection Engineering, Alert Triage, Endpoint Monitoring, Compliance Monitoring

**SOC Concepts:** Log Collection, Event Parsing, Decoders, Detection Rules, Alert Severity, Threat Investigation, Security Dashboards, Automated Response

**Frameworks / Standards:** CIS Benchmarks, MITRE ATT&CK, GDPR, PCI DSS, NIST

---

# Project Takeaway

This project demonstrated how an open-source security platform can provide centralized visibility across endpoints while supporting both **SOC operations and security compliance**.

The hands-on portion was particularly valuable because it connected individual security concepts—including vulnerability scanning, endpoint telemetry, configuration assessment, log parsing, detection rules, and alerting—into a single operational workflow.

The project demonstrated practical experience moving from:

**Endpoint Data → Security Detection → Alert Investigation → Risk Analysis → Response**
