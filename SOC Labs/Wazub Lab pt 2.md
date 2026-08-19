

# Wazuh Custom Detection Engineering & Threat Investigation Lab

## Project Overview

This project focused on building and testing **custom Wazuh detection capabilities** using decoders, detection rules, CDB threat-intelligence lists, frequency-based rules, and rule exclusions.

The lab built on foundational Wazuh concepts by moving beyond preconfigured detections and demonstrating how a SOC team can create and tune its own detection logic for organization-specific threats.

The hands-on work involved:

* Creating a custom Wazuh decoder
* Extracting structured fields from raw web logs
* Testing decoder functionality
* Creating custom Wazuh rules
* Detecting suspicious web activity
* Mapping detections to MITRE ATT&CK
* Using CDB lists for threat intelligence enrichment
* Creating frequency-based brute-force detections
* Investigating Windows authentication activity
* Investigating adversary behavior through Wazuh Discover
* Identifying false positives
* Creating rule exclusions
* Fine-tuning existing Wazuh rules
* Testing detection logic against simulated attack activity

The project provided practical experience with **detection engineering, SIEM rule development, threat investigation, threat intelligence, alert tuning, and SOC operations**.

---

# Learning Objectives

The main objectives of the lab were to understand how Wazuh transforms raw security data into actionable alerts.

The project focused on:

1. Understanding how Wazuh decoders extract security-relevant information from raw logs.
2. Creating custom detection rules using decoded fields.
3. Extending Wazuh's detection capabilities beyond the default ruleset.
4. Using CDB lists to detect known indicators of compromise.
5. Creating frequency-based detections for repeated malicious activity.
6. Simulating attack behavior to validate detection rules.
7. Investigating real security events through Wazuh Discover.
8. Understanding how false positives can be excluded.
9. Learning how to safely fine-tune built-in Wazuh rules.

---

# Lab Environment

The lab used a virtualized Wazuh environment containing the Wazuh management server and monitored endpoints.

| Component               | Purpose                                                       |
| ----------------------- | ------------------------------------------------------------- |
| Wazuh Management Server | Centralized security monitoring and detection platform        |
| Wazuh Dashboard         | Interface for rule testing, investigation, and alert analysis |
| Windows Endpoint        | Source of Windows security and Sysmon events                  |
| Linux Endpoint          | Source of Linux process and audit events                      |
| Wazuh Agents            | Collect endpoint telemetry                                    |
| Custom Web Logs         | Used to develop and test custom decoders                      |
| CDB Lists               | Used to identify known malicious indicators                   |

---

# 1. Understanding Wazuh Decoders

Wazuh can ingest logs from many different sources and formats.

Common formats include:

* JSON
* CSV
* EVTX
* Syslog

However, organizations may also have custom applications that generate logs in formats Wazuh does not understand by default.

For this project, I worked with a custom web access log format:

```text
29/Mar/2025 13:36:36 WEB tryhackme.com 203.45.12.88 "GET /index.php HTTP/1.1" 200 1024 "-" "Mozilla/5.0"
```

Without a decoder, Wazuh cannot reliably extract individual fields from this event.

The objective was therefore to create a custom decoder capable of identifying and extracting useful security fields.

---

# 2. Building a Custom Web Log Decoder

I created a custom `thm-web` decoder in the Wazuh `local_decoder.xml` configuration.

The decoder was divided into multiple sections.

```xml
<decoder name="thm-web">
    <prematch>^\d+/\w+/\d+ \d+:\d+:\d+ WEB</prematch>
</decoder>

<decoder name="thm-web">
    <parent>thm-web</parent>
    <regex>^\S+ \S+ WEB (\S+) (\S+)</regex>
    <order>website, srcip</order>
</decoder>

<decoder name="thm-web">
    <parent>thm-web</parent>
    <regex offset="after_regex">"(\w+) (\S+) \S+" (\d+) (\d+) "(\S+)" "(\S+)"</regex>
    <order>method, url, status, bytes, referer, useragent</order>
</decoder>
```

## Decoder Components

| Component            | Purpose                                         |
| -------------------- | ----------------------------------------------- |
| `<decoder name="x">` | Defines the decoder name                        |
| `<parent>`           | Establishes a parent-child decoder relationship |
| `<prematch>`         | Performs an initial regex check                 |
| `<regex>`            | Extracts specific values from the event         |
| `<order>`            | Maps captured regex groups to field names       |

---

# 3. Understanding the Decoder Pipeline

The decoder processes the event in multiple stages.

## Step 1: Validate the Event Format

The first decoder block uses:

```regex
^\d+/\w+/\d+ \d+:\d+:\d+ WEB
```

This verifies that the event begins with the expected date, time, and `WEB` identifier.

For example:

```text
29/Mar/2025 13:36:36 WEB
```

matches the expected pattern.

---

## Step 2: Extract the Website and Source IP

The second decoder uses:

```regex
^\S+ \S+ WEB (\S+) (\S+)
```

This extracts:

```text
website=tryhackme.com
srcip=203.45.12.88
```

---

## Step 3: Extract Web Request Information

The final decoder extracts additional fields such as:

* HTTP method
* Requested URL
* HTTP status
* Response size
* Referer
* User agent

The resulting structured event becomes:

```json
{
    "website": "tryhackme.com",
    "srcip": "203.45.12.88",
    "method": "GET",
    "status": "200",
    "bytes": "1024",
    "referer": "-",
    "useragent": "Mozilla/5.0",
    "url": "/index.php"
}
```

---

# 4. Testing the Custom Decoder

I used Wazuh's **Ruleset Test** functionality to validate the decoder.

Before the custom decoder was installed, the raw web log did not match an appropriate decoder.

I then:

1. Opened **Server Management → Decoders**.
2. Located `local_decoder.xml`.
3. Added the custom `thm-web` decoder.
4. Saved the configuration.
5. Reloaded the Wazuh configuration.
6. Returned to **Ruleset Test**.
7. Cleared the previous test session.
8. Submitted the raw web log again.
9. Verified that the custom decoder successfully parsed the event.

## What I Learned

This demonstrated a critical SIEM concept:

**Detection rules depend on structured event data.**

The overall process is:

```text
Raw Log
   |
   v
Decoder
   |
   v
Structured Fields
   |
   v
Detection Rule
   |
   v
Alert
```

Without the decoder, the rule would not have reliable fields such as `srcip`, `url`, `status`, or `method` to evaluate.

---

# 5. Creating Custom Wazuh Rules

After creating the decoder, I created custom Wazuh rules to detect specific security events.

Wazuh includes a large number of prebuilt rules, but organizations often need additional detections for:

* Custom applications
* Internal infrastructure
* Organization-specific threats
* Threat intelligence
* Security policies
* Detection gaps
* Custom attack patterns

---

# 6. Understanding Wazuh Rule Structure

I examined custom rules designed to detect Sysmon events and suspicious commands.

Example:

```xml
<rule id="184680" level="0">
    <match>Microsoft-Windows-Sysmon/Operational: INFORMATION(1)</match>
    <description>Sysmon - Event 1</description>
    <group>sysmon_event1,</group>
</rule>

<rule id="184690" level="12">
    <if_group>sysmon_event1</if_group>
    <field name="sysmon.image">whoami.exe</field>
    <description>Sysmon - Whoami Command</description>
    <mitre><id>T1033</id></mitre>
    <group>pci_dss_10.6.1,pci_dss_11.4,...</group>
</rule>
```

## Important Rule Properties

| Property     | Purpose                                                    |
| ------------ | ---------------------------------------------------------- |
| `rule.id`    | Unique identifier for the rule                             |
| `rule.level` | Severity level from 0–15                                   |
| `if_group`   | Requires a previous rule group to match                    |
| `if_sid`     | Requires a specific previous rule ID to match              |
| `match`      | Matches event content                                      |
| `field.name` | Matches a specific decoded field                           |
| `mitre`      | Associates the rule with a MITRE ATT&CK technique          |
| `group`      | Associates the rule with security or compliance categories |

---

# 7. Wazuh Rule Severity

Wazuh uses severity levels from **0 through 15**.

| Level | General Meaning               |
| ----: | ----------------------------- |
|     0 | Ignored / no alert            |
|   1–6 | Regular system events         |
|  7–11 | Notable security events       |
| 12–14 | High-severity security events |
|    15 | Critical security event       |

A level 0 rule can act as a base or parent rule without generating a visible alert.

Higher-level child rules can then use more specific conditions to identify suspicious activity.

---

# 8. Hierarchical Detection Rules

Wazuh rules are hierarchical.

A parent rule establishes a broad condition, while a child rule adds more specific detection logic.

For example:

```text
Sysmon Event 1
      |
      v
Process Creation
      |
      v
Specific Process
      |
      v
whoami.exe
      |
      v
Level 12 Alert
```

This approach allows detection engineers to build layered detections instead of creating completely independent rules for every event.

## What I Learned

This was one of the most important detection-engineering concepts in the lab.

Rather than treating every event independently, Wazuh can use previous rule matches as the foundation for more specific detections.

---

# 9. Creating Custom Web Detection Rules

I then created rules that used the fields extracted by the custom `thm-web` decoder.

The base rule was:

```xml
<rule id="200000" level="0">
    <decoded_as>thm-web</decoded_as>
    <description>Custom - THM Web Request</description>
    <group>web</group>
</rule>
```

This rule identifies events decoded by `thm-web`.

I then created a child rule to detect successful administrative logins:

```xml
<rule id="200001" level="12">
    <if_sid>200000</if_sid>
    <status>200</status>
    <url>/admin/login.php</url>
    <field name="method">POST</field>
    <description>Custom - THM Admin Page Login</description>
    <mitre><id>T1078</id></mitre>
    <group>alert,web</group>
</rule>
```

The detection logic looks for:

```text
Status = 200
AND
URL = /admin/login.php
AND
Method = POST
```

---

# 10. Testing the Web Detection Rule

I tested the rules against multiple web events:

```text
29/Mar/2025 13:36:36 WEB tryhackme.com 203.45.12.88 "GET /index.php HTTP/1.1" 200 1024 "-" "Mozilla/5.0"

29/Mar/2025 13:37:12 WEB tryhackme.com 91.108.4.201 "POST /admin/login.php HTTP/1.1" 200 1854 "-" "curl/7.68.0"

29/Mar/2025 13:40:18 WEB tryhackme.com 77.83.142.33 "GET /contact.php HTTP/1.1" 200 512 "-" "Mozilla/5.0"
```

The second event matched the custom administrative login rule because it satisfied all three conditions.

## What I Learned

This demonstrated how detection rules can combine multiple fields to produce more precise detections.

Instead of alerting on every HTTP request, the rule identifies a specific combination of:

* HTTP method
* URL
* Status code

This reduces unnecessary alerts and makes the detection more meaningful to a SOC analyst.

---

# 11. CDB Lists and Threat Intelligence

I investigated Wazuh **CDB lists**, which can be used for enrichment and threat intelligence.

CDB lists can contain information such as:

* Malicious IP addresses
* Malicious domains
* File hashes
* Asset mappings
* Event-code descriptions
* Other indicators of compromise

I tested a rule that compared the decoded `srcip` against a malicious-IP list.

Example:

```xml
<rule id="200002" level="15">
    <if_sid>200000</if_sid>
    <list field="srcip" lookup="address_match_key">etc/lists/malicious-ioc/malicious-ip</list>
    <description>SOC Alert - THM Web Request From Bad IP</description>
    <mitre><id>T1071</id></mitre>
    <group>alert,web,cti</group>
</rule>
```

The detection process is:

```text
Web Event
   |
   v
thm-web Decoder
   |
   v
Extract srcip
   |
   v
CDB Lookup
   |
   v
Known Malicious IP?
   |
  YES
   |
   v
Level 15 Critical Alert
```

## What I Learned

CDB lists provide a simple way to combine **threat intelligence with event detection**.

Instead of hard-coding every malicious IP into an individual rule, the detection rule can reference an external indicator list.

This makes threat intelligence easier to maintain.

---

# 12. Frequency-Based Detection

Single-event detection is not always sufficient.

Some attacks are characterized by repeated events over a short period.

Examples include:

* Brute-force attacks
* Password spraying
* Denial-of-service activity
* Repeated authentication failures
* Automated scanning

I created a frequency-based detection for repeated failed administrator logins.

The base rule was:

```xml
<rule id="200003" level="12">
    <if_sid>200000</if_sid>
    <status>401</status>
    <url>/admin/login.php</url>
    <field name="method">POST</field>
    <description>Custom - THM Admin Page Failure</description>
    <group>alert,web</group>
</rule>
```

The frequency rule was:

```xml
<rule id="200004" level="15" frequency="3" timeframe="60">
    <if_matched_sid>200003</if_matched_sid>
    <same_srcip />
    <description>SOC Alert - THM Admin Page Bruteforce</description>
    <mitre><id>T1110</id></mitre>
    <group>alert,web,freq</group>
</rule>
```

---

# 13. Understanding Frequency and Timeframe

The frequency rule triggers when:

* The previous failed-login rule matches
* The events come from the same source IP
* Three matching events occur
* The events occur within 60 seconds

The logic can be represented as:

```text
Failed Login #1
       |
Failed Login #2
       |
Failed Login #3
       |
       v
3 Events Within 60 Seconds
       |
       v
Brute-Force Detection
       |
       v
Level 15 Alert
```

The `same_srcip` condition ensures that events are grouped based on the source IP address.

---

# 14. Understanding Wazuh Frequency Rule Behavior

One important lesson from the lab was how Wazuh handles repeated events that match multiple rules.

For eight failed login attempts from the same IP within 60 seconds, the expected behavior demonstrated in the lab was:

**6 regular failure alerts + 2 brute-force alerts**

The reason is that Wazuh generates at most one alert per event.

The frequency counter works in groups:

```text
Events 1–2
   |
   +--> 2 failure alerts
   +--> Counter = 2

Event 3
   |
   +--> Brute-force threshold reached
   +--> Level 15 alert
   +--> Counter resets

Events 4–5
   |
   +--> 2 failure alerts
   +--> Counter = 2

Event 6
   |
   +--> Brute-force threshold reached
   +--> Level 15 alert
   +--> Counter resets

Events 7–8
   |
   +--> 2 failure alerts
```

## What I Learned

Frequency-based detections require careful testing because their behavior can differ from other SIEM platforms.

Understanding how counters reset and how multiple rules interact is important when designing reliable detections.

---

# 15. Testing Advanced Detection Rules

I tested the custom CDB and frequency-based rules against simulated attack activity.

The test data included:

```text
29/Mar/2025 13:37:13 WEB tryhackme.com 91.108.4.201 "POST /admin/login.php HTTP/1.1" 401 1854 "-" "curl/7.68.0"

29/Mar/2025 13:37:14 WEB tryhackme.com 91.108.4.201 "POST /admin/login.php HTTP/1.1" 401 1854 "-" "curl/7.68.0"

29/Mar/2025 13:37:15 WEB tryhackme.com 91.108.4.201 "POST /admin/login.php HTTP/1.1" 401 1854 "-" "curl/7.68.0"

29/Mar/2025 13:37:16 WEB tryhackme.com 45.227.253.11 "GET /index.php HTTP/1.1" 200 2738 "-" "Mozilla/5.0"

29/Mar/2025 13:37:17 WEB tryhackme.com 62.182.85.144 "GET /.env HTTP/1.1" 404 348 "-" "Mozilla/5.0"

29/Mar/2025 13:37:18 WEB tryhackme.com 158.51.96.38 "POST /utils/exec HTTP/1.1" 200 3212 "-" "python-requests/2.28.0"
```

I used the **Ruleset Test** functionality to validate the detection behavior.

## What This Simulated

The test data represented multiple suspicious behaviors:

* Repeated failed administrator authentication
* Potential brute-force activity
* Requests for sensitive `.env` configuration files
* Automated HTTP requests using scripting tools

This demonstrated how multiple individual events can be investigated as part of a larger attack pattern.

---

# 16. SOC Threat Investigation

The lab then transitioned from detection engineering into a more realistic SOC investigation scenario.

The scenario involved a **level 15 brute-force alert** targeting:

* User: `rick.brown`
* Server: `SRV-JMP01`

I investigated the activity using the Wazuh **Discover** interface.

The investigation query was:

```text
rule.id: (160001 or 60122)
```

The time range was expanded to include the relevant April 2026 events.

I then created an investigation table using fields including:

```text
data.win.eventdata.targetUserName
data.win.system.eventRecordID
rule.description
```

## Investigation Workflow

```text
Level 15 Alert
      |
      v
Identify Target User
      |
      v
Identify Target Server
      |
      v
Review Authentication Events
      |
      v
Count Failed Attempts
      |
      v
Review Event Record IDs
      |
      v
Correlate Detection Rules
      |
      v
Investigate Related Activity
```

---

# 17. Investigating Adversary Activity

The SOC investigation demonstrated how an analyst can move from a high-severity alert to the underlying events.

Important investigation questions included:

* Who was targeted?
* Which server was targeted?
* How many login attempts occurred?
* Which detection rules triggered?
* What source IP was involved?
* Was the activity isolated or part of a larger attack?
* Did other malicious activity occur?
* What process initiated network connections?
* Was there evidence of command execution?

This investigation process reinforced the importance of **alert context and event correlation**.

---

# 18. Investigating CDB-Based Detections

The investigation also involved a CDB rule from the Wazuh ruleset that identified another adversarial action.

The analyst needed to identify:

* The IP address that matched the CDB list
* The resulting Wazuh alert
* The program that initiated the connection

This demonstrated how threat intelligence can provide additional context during an investigation.

The investigative workflow was:

```text
Security Alert
      |
      v
Identify Source IP
      |
      v
CDB Threat Intelligence Match
      |
      v
Correlate Endpoint Activity
      |
      v
Identify Initiating Process
      |
      v
Determine Attack Context
```

---

# 19. Alert Exclusions and False Positives

I investigated how Wazuh handles false positives.

Unlike some SIEM platforms where analysts may simply add `NOT` filters to queries, Wazuh can use child rules with:

```xml
level="0"
```

to suppress specific events.

Example:

```xml
<group name="audit,">
   <rule id="100005" level="12">
        <if_sid>100003</if_sid>
        <field name="audit.file.name">malware|shell</field>
        <description>Audit: $(audit.exe) created a suspicious file: $(audit.file.name).</description>
        <group>audit_watch_write,</group>
    </rule>

   <rule id="100006" level="0">
        <if_sid>100005</if_sid>
        <field name="audit.file.name">shell-checker-thm.sh</field>
        <description>False positive. The script is used by our red team for testing.</description>
        <group>audit_watch_write,</group>
    </rule>
</group>
```

The logic is:

```text
File Creation
     |
     v
Rule 100005
     |
     v
Suspicious Filename?
     |
    YES
     |
     v
Check Child Rule 100006
     |
     +----> shell-checker-thm.sh
     |             |
     |             v
     |       Level 0 / Suppressed
     |
     +----> Other File
                   |
                   v
              Level 12 Alert
```

## What I Learned

False-positive management is an important part of detection engineering.

A detection that produces too many false positives can cause:

* Alert fatigue
* Analyst inefficiency
* Missed threats
* Reduced trust in the SIEM

Therefore, tuning detections is just as important as creating them.

---

# 20. Fine-Tuning Existing Rules

I also learned how to safely modify built-in Wazuh detections.

Instead of directly editing the original Wazuh ruleset, the recommended approach is to create an **override** in a local rule file.

For example:

```xml
<group name="audit,">
   <rule id="100005" level="12" overwrite="yes">
        <field name="audit.file.name">malware|shell|dropper|linpeas</field>
        <description>Audit: $(audit.exe) created a suspicious file: $(audit.file.name).</description>
        <group>audit_watch_write,</group>
    </rule>
</group>
```

The original detection can therefore be extended to include:

* `malware`
* `shell`
* `dropper`
* `linpeas`

## Why This Matters

Directly modifying vendor-provided rules can create problems when Wazuh updates its ruleset.

Using local overrides provides a safer method for maintaining custom detection logic.

---

# Detection Engineering Workflow

The entire project demonstrated a repeatable detection engineering process:

```text
Identify Detection Requirement
          |
          v
Understand Raw Log Format
          |
          v
Create / Modify Decoder
          |
          v
Extract Relevant Fields
          |
          v
Test Decoder
          |
          v
Create Base Rule
          |
          v
Create Specific Detection Rule
          |
          v
Add MITRE / Compliance Mapping
          |
          v
Test Against Simulated Activity
          |
          v
Review False Positives
          |
          v
Tune Detection
          |
          v
Deploy to Production
          |
          v
Monitor Detection Performance
```

---

# Hands-On Work Completed

The following activities were performed or investigated during the virtual lab:

* [x] Reviewed Wazuh decoder functionality
* [x] Analyzed a custom web access log format
* [x] Created a custom `thm-web` decoder
* [x] Used regular expressions to extract event fields
* [x] Mapped regex capture groups to Wazuh fields
* [x] Tested the custom decoder using Ruleset Test
* [x] Reloaded Wazuh configuration after decoder changes
* [x] Created custom Wazuh rules
* [x] Created hierarchical parent and child rules
* [x] Used decoded fields in detection rules
* [x] Created a custom web login detection
* [x] Tested successful administrative login detection
* [x] Investigated Wazuh rule severity levels
* [x] Mapped detections to MITRE ATT&CK
* [x] Investigated CDB threat-intelligence lists
* [x] Created a malicious-IP detection
* [x] Created frequency-based brute-force detection
* [x] Tested repeated authentication failures
* [x] Investigated Wazuh frequency counter behavior
* [x] Investigated a level 15 brute-force scenario
* [x] Investigated Windows authentication events
* [x] Used Discover for threat investigation
* [x] Investigated CDB-based alerts
* [x] Investigated initiating processes
* [x] Studied false-positive exclusions
* [x] Created level 0 exclusion logic
* [x] Investigated Wazuh rule tuning
* [x] Used rule overwrites to extend detection logic

---

# Key Skills Demonstrated

## Detection Engineering

* Custom Wazuh decoders
* Regular expressions
* Detection rule development
* Hierarchical rules
* Rule severity
* Rule dependencies
* Frequency-based detection
* Detection tuning
* False-positive management

## SIEM

* Log parsing
* Structured event data
* Alert generation
* Event correlation
* Security dashboards
* Threat investigation
* Alert triage

## Threat Intelligence

* CDB lists
* IOC matching
* Malicious IP detection
* Threat intelligence enrichment
* Indicator-based alerting

## SOC Operations

* Brute-force investigation
* Authentication monitoring
* Alert triage
* Event correlation
* Process investigation
* Source IP analysis
* Threat investigation

## Compliance

* MITRE ATT&CK mapping
* PCI DSS rule groups
* Security monitoring evidence
* Detection control validation

---

# Key Lessons Learned

## 1. Decoders Are the Foundation of Custom Detection

A detection rule cannot effectively evaluate fields that Wazuh has not successfully extracted.

The decoder transforms:

```text
Raw Log
```

into:

```text
Structured Security Event
```

which can then be used by detection rules.

---

## 2. Good Detection Rules Are Specific

A detection such as:

```text
POST /admin/login.php
```

combined with:

```text
HTTP 200
```

is more useful than generating an alert for every HTTP request.

Multiple conditions can reduce false positives and increase detection quality.

---

## 3. Threat Intelligence Can Strengthen Detection

CDB lists allow Wazuh to compare event data against known indicators.

The workflow becomes:

```text
Event
 ↓
Extract IOC
 ↓
Compare Against Threat Intelligence
 ↓
Match
 ↓
High-Severity Alert
```

This allows security teams to integrate external or internally maintained indicators into their detection pipeline.

---

## 4. Frequency Detection Is Useful for Behavioral Attacks

Some attacks cannot be identified reliably from a single event.

Brute-force attacks are a good example.

A single failed login may be completely normal.

Hundreds of failed logins from one source over a short period are much more suspicious.

Frequency-based detection allows Wazuh to identify this behavior.

---

## 5. Detection Engineering Includes Tuning

Creating a detection is only the beginning.

A production detection lifecycle should include:

```text
Create
  ↓
Test
  ↓
Validate
  ↓
Deploy
  ↓
Monitor
  ↓
Tune
  ↓
Retest
```

This is necessary to maintain useful detections while minimizing false positives.

---

## 6. False Positives Must Be Managed Carefully

The lab demonstrated how level 0 child rules can suppress known benign activity.

This is particularly useful for environments where:

* Red-team activity is authorized
* Security testing is routine
* Administrative scripts trigger detections
* Certain applications legitimately perform suspicious-looking actions

The goal is not simply to generate as many alerts as possible.

The goal is to generate **high-quality alerts that analysts can act on**.

---

# SOC Analyst Perspective

This project closely simulated several activities performed by SOC analysts.

An analyst receiving a level 15 brute-force alert could investigate:

```text
High-Severity Alert
        |
        v
Target User
        |
        v
Target Server
        |
        v
Authentication Attempts
        |
        v
Source IP
        |
        v
Related Alerts
        |
        v
Process / Network Activity
        |
        v
Determine Attack Scope
        |
        v
Escalate / Contain / Respond
```

The lab demonstrated that detection engineering and SOC investigation are closely connected.

A detection engineer creates the logic that identifies suspicious behavior, while the SOC analyst uses those detections to investigate and respond to real events.

---

# Detection Engineering Perspective

The most valuable technical takeaway from this lab was understanding that detection engineering is a lifecycle rather than simply writing XML rules.

A detection engineer must understand:

* What data is available
* How the data is structured
* Which fields are important
* How to parse the data
* What behavior should trigger an alert
* How severe the behavior is
* Which ATT&CK technique applies
* How to test the detection
* How to handle false positives
* How to maintain the detection after platform updates

This project provided hands-on exposure to each of these stages.

---

# Project Outcome

This lab expanded my Wazuh knowledge from basic monitoring into **custom detection engineering and threat investigation**.

The project demonstrated how to move from an unsupported raw log format to a fully functional security detection:

```text
Custom Log
    |
    v
Custom Decoder
    |
    v
Structured Fields
    |
    v
Base Rule
    |
    v
Specific Detection
    |
    v
Threat Intelligence / Frequency Logic
    |
    v
Alert
    |
    v
SOC Investigation
    |
    v
Tuning
```

The most important practical skills developed were:

* Building custom decoders
* Writing regex-based field extraction
* Creating hierarchical Wazuh rules
* Using CDB lists for IOC matching
* Creating brute-force detections
* Testing simulated attacks
* Investigating Windows authentication events
* Mapping detections to MITRE ATT&CK
* Managing false positives
* Safely overriding built-in rules

---



**Wazuh Custom Detection Engineering & Threat Investigation Lab**

* Developed and tested custom Wazuh decoders using regular expressions to parse previously unsupported web access logs into structured security fields.
* Engineered hierarchical Wazuh detection rules for administrative login activity, suspicious process execution, malicious IP indicators, and brute-force authentication behavior using frequency and timeframe conditions.
* Conducted SOC-style threat investigations in Wazuh Discover by correlating authentication events, source IPs, rule IDs, targeted accounts, and initiating processes.
* Implemented CDB-based threat intelligence detection and investigated MITRE ATT&CK mappings for credential access, discovery, and command-and-control-related activity.
* Practiced false-positive reduction and detection tuning using level 0 exclusions and local rule overrides to improve alert quality without modifying vendor-provided rules.

---

# Technologies & Concepts

**Tools:** Wazuh, Wazuh Dashboard, Wazuh Ruleset Test, Wazuh Discover

**Technical Skills:** XML, Regular Expressions, Log Parsing, Detection Engineering, SIEM, Threat Intelligence, IOC Matching

**Security Concepts:** Brute-Force Detection, Authentication Monitoring, Alert Triage, Threat Investigation, False-Positive Management, Detection Tuning

**Frameworks:** MITRE ATT&CK, PCI DSS

**Detection Techniques:** Custom Decoders, Hierarchical Rules, CDB Lists, Frequency-Based Rules, Rule Overrides, Alert Exclusions

---

# Final Takeaway

This project demonstrated practical experience building security detections rather than simply consuming prebuilt alerts.

The lab showed how a SOC can take an unfamiliar log format, build the necessary parsing logic, create detection rules, enrich events with threat intelligence, identify repeated attack behavior, investigate resulting alerts, and tune the detection to reduce false positives.

The overall workflow was:

**Raw Security Data → Parsing → Detection Logic → Threat Intelligence → Alert → Investigation → Tuning**

This provided hands-on experience with a core SOC and detection-engineering responsibility: **turning raw security telemetry into reliable, actionable detections.**
