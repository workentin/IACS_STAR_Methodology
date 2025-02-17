# IACS System Testing and Assessment Rating (STAR) Methodology (IACS STAR)
A methodology for scoring implementation vulnerabilities identified when testing and doing cybersecurity assessments of industrial and automation control environments.  

## Contributions

* Author: Don C. Weber
* IACS/OT Contributors: Oscar Delgado, Danielle Jablanski, Tim Conway
* IT/Infosec Contributors: Jeff Williams (Author of original OWASP Risk Rating Methodology)

# Introduction

Security assessments and penetration testing of an Industrial and Automation Control Systems (IACS) / Operational Technology (OT) enviornment are two types of vulnerability assessments that feed information into the [ISA/IEC 62443](https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards) risk assessment process. The Cyber Security Management System (CSMS) process, detailed in ISA/IEC-62443-2-1 standard, requires a detailed risk assessment which is outlined in full within the ISA/IEC-62443-3-2 standard. The detailed risk assessment requires that a vulnerability assessment is conducted to identify unmitigated risk. These vulnerability assessments required that the assessment findings be qualitatively rated according to the threat, likelihood, and concequences should the vulnerability be exploit and threat actor success realized. 

The IACS System Testing and Assessment Rating (STAR) Methodology (IACS STAR) is intended to be a methodology to estimate the severity of identified risks to the IACS/OT environment. This methodology includes the classic qualitative risk calculation elements while adding the consequence considerations necessary for understanding risks to IACS/OT processes and equipment. Having a system in place that addresses IACS/OT concerns for rating risks will save time and eliminate arguing about prioritizations and improve countermeasure selection to quickly reduce risk. 

The authors have tried hard to make this model simple to use, while keeping enough detail for accurate risk estimates to be made. Please reference the section below on customization for more information about tailoring the model for use in a specific organization.

# Overview 

Over the years there has be a lot of debate about how to rate risk within industrial and automation control environments. Rating risk is difficult due to the varying ideas about likelihood, frequency, consequences, and impact ratings. This project is an effort to update the [OWASP Risk Rating Methodology](https://owasp.org/www-community/OWASP_Risk_Rating_Methodology) to be usable when conducting security assessments and pentration tests within IACS/OT enviornments. It is not designed to replace an organization's risk rating methdodology. It is intended for assessment teams to use when a specific methodology has not been defined or when a quicker method is needed to quickly rate and reduce risk.

For background, there are other more mature, popular, or well established Risk Rating Methodologies that can be followed:

- [ISA/IEC 62443 Series of Standards](https://www.isa.org/standards-and-publications/isa-standards/isa-iec-62443-series-of-standards)
- [NIST 800-30 - Guide for Conducting Risk Assessments](https://csrc.nist.gov/publications/detail/sp/800-30/rev-1/final)
- [National Vulnerability Database (NVD) Common Vulnerability Scoring System Version 3 (CVSSv3) Calculator](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)
- [Government of Canada - Harmonized TRA Methodology](https://cyber.gc.ca/en/guidance/harmonized-tra-methodology-tra-1)
- Mozilla resources:
    - [Risk Assessment Summary](https://infosec.mozilla.org/guidelines/assessing_security_risk)
    - [Rapid Risk Assessment (RRA)](https://infosec.mozilla.org/guidelines/risk/rapid_risk_assessment.html)
- [The Blind Spot: How to Simply Calculate Cyber Attack Likelihood Using the Exploitability Assessment](https://www.cybersecureot.info/post/the-blind-spot-how-to-simply-calculate-cyber-attack-likelihood-using-the-exploitability-assessment)

The risk and vulnerability assessment process is augmented by threat modeling to identify and prioritize potential attack vectors and successful exploitations. The following are a list of methods to help with the threat modeling process:

- [First.org: Threat Modeling](https://www.first.org/global/sigs/cti/curriculum/threat-modelling)
- [The Operational esilience Framework](https://www.grf.org/orf)
- [Microsoft Threat Modeling Tool threats](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats) - aka S.T.R.I.D.E.
- [Threat Assessment and Remediation Analysis (TARA)](https://www.mitre.org/news-insights/publication/threat-assessment-and-remediation-analysis-tara)
- [ICS Layered Threat Modeling](https://sansorg.egnyte.com/dl/fztutwiK5J)
- [OWASP Threat Modeling](https://owasp.org/www-community/Threat_Modeling)
- [OWASP Application Threat Modeling](https://owasp.org/www-community/Application_Threat_Modeling)
- [OWASP pytm](https://owasp.org/www-project-pytm/) Pythonic framework for threat modeling
- [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon/) threat modeling tool

The ISA/IEC 62443 CSMS Detailed Risk Assessment process requires that considerations for the criticality of processes, equipment, and procedures are calculated and documented. The following resources provide some details and insight into the considerations for this process. 

- [Critical infrastructure cybersecurity prioritization: A cross-sector methodology for ranking operational technology cyber scenarios and critical entities](https://www.atlanticcouncil.org/in-depth-research-reports/issue-brief/critical-infrastructure-cybersecurity-prioritization/)
- [Common Vulnerability Scoring System Version 4.0](https://www.first.org/cvss/v4-0/) - CVSS version 4.0 is the next generation of the Common Vulnerability Scoring System standard.

# Approach

The ISA/IEC-62443-2-1 standard outlines that risk is calculated by taking the likelihood that an event will occur and scaling it with the concequences should the event be realized. Hence the equation **Risk = Likelihood * Consequence**. The assignment of the likelihood and consequence variables is the typical debate. 

Most IACS/OT likelihood calculations, sometimes referred to as frequency, take into consideration the commonly understood cases of equipment failure. Industrial and automation equipment have specific usage tollerances that, when calculated with known usage, can provide a measurable likelihood that the equipment will experience a problem. This often results in a likelihood table that uses specific time tables for an event to occur.

- High: will occur in the next year
- Moderate: will occur in the next 10 years
- Low: no history of occurance and therefore unlikely

These typcial likelihood ratings are not applicable when considering cybersecurity and the likelihood or frequency that a threat actor will attempt to exploit a vulnerability. In 2008 the Federal Energy Regulatory Commission (FERC) determined that electric utilities required specific guidance to understand how to address likelihood and frequency when calculating risk. In [FERC Order 706 Mandatory Reliability Standards for Critical Infrastructure Protection](https://www.ferc.gov/sites/default/files/2020-04/E-2_11.pdf) the following guidance was provided:

- "Because there is insufficient data available to determine frequency, it should be assumed that an event will occur." 
- "Risk-based assessment methodology should focus on the consequences of an outage, not the likelihood of an outage.“

In the sections below, the factors that make up "likelihood" and "consequences" for IACS/OT environments are broken down as defined in the 'ISA/IEC-62443-3-2 Zone and Conduit Requirements (ZCR) 5: Perform a detailed cyber security risk assessment' section. The assessment team is shown how to leverage these factors to determine the overall severity for risks identified during a vulnerability assessment.

```
  Step 1: ZCR 5.1: Identify Threats
  Step 2: ZCR 5.2: Identify Vulnerabilities
  Step 3: ZCR 5.3: Factors for Estimating Consequences and Impact
  Step 4: ZCR 5.4: Factors for Estimating Likelihood
  Step 5: ZCR 5.5: Calculate Unmitigated Cybersecurity Risk
  Step 6: Reporting Vulnerabilities and Vector Scores
  Step 7: IACS STAR Calculator
```

## Step 1: Identify Threats (ISA/IEC-62443-3-2 ZCR 5.1)

Following the model of the OWASP risk rating system, there are a few factors that aid in the determination of risk. This effort has modified the OWASP model slightly to fit into the IACS/OT modle. These factors are used when modeling attack scenarios to prioritize assessment efforts. The fartors are also used when calculating the risk rating each vulnerability and will vary according to the specifics of the situation. These factors include: 

- a description of the threat actor group
- the capabilities or skill-level of the threat actors
- the possible motivations for the threat actors
- the opportunities provided to the threat actors by the environment's architecture
- the level of access achieved when successfully exploiting the vulnerability

### Threat Actor Factors

It is important to understand threat actor groups when considering the skills, motivation, opportunities, and population of potential attackers. There are many lists that outline specific threat actor groups that are known to attack IACS/OT environments. These include: [Wikipedia: threat actor](https://en.wikipedia.org/wiki/Threat_actor), [MITRE](https://attack.mitre.org/groups/), [Mandiant](https://www.mandiant.com/resources/insights/apt-groups), [Crowdstrike](https://www.crowdstrike.com/adversaries/), [Dragos](https://www.dragos.com/threat-groups/), and more. To perform vulnerabilitie assessments there needs to be an easier list that allows all stakeholders to agree. Each of these groups are more accurately defined by their skills, likelihood of success, and their primary objectives. The following is one possible breakdown of threat actors associated with IACS/OT environments, each with their own levels of skill, motivation, opportunities, and group size.

- **Malware**: Malicious programs that have a specific affect on vulnerable / compromised systems. This includes general malware, IACS/OT malware, and custom malware.
- **Script Kiddies**: Uses tools, techiques, and malware that are known, common, and easily accessible.
- **Cybercriminal**: An advanced group that have the maturity and financial backing to obtain or develop tools, hire technology experts, and time to conduct research and development to achieve goals.
- **Hacker**: Individuals that are mainly comprised of security researchers and sensationalists. Their activities are typcially limited to conducted vulnerability research on products. Select individuals may progress unauthorized access to understand risk and publically disclose information.
- **Competitor**: Direct competitors with team members that will have access to specific knowledge that will apply in the case of specialized software and equipment.
- **Employee**: Disgruntled and reckless employees that have privileged access to systems and devices as well as the knowledge about the technologies and networks.
- **Vendor**: Rogue vendor, integrator, or consultant that is disgruntled, reckless, or malicious. Have knowledge about technologies and may have privileged access to systems, networks, and devices.
- **Cyberwarrior**: Nation-state threat actors that typically operate in groups to achieve specific goals. Trained to live-off-the-land, steal credentials, and exfiltrate information. Has access to custom tools and malware designed to maintain persistence, propogate, and achieve their goals.

To compute the likelihood that a threat actor groups will be successful the following numerical breakdowns will be assigned to skill level, motivation, opportunity, and size categories. The level of each category can be estimated to calculate the Threat Agent Factor which will be used to computer the overall likelihood that an event will be realized.  

- **Skill Level** - How technically skilled is this group of threat actors? 
  - Limited Information Technology (IT), network, and no Operational Technology (OT) skills (1)
  - Moderate IT, limited network, and no OT technical skills (3)
  - Advanced IT, moderate network, and limited OT technical skills (5)
  - Advanced IT, advanced network, and moderate OT technical skills (6)
  - Advanced OT technical skills (8)
  - Security penetration skills and knowledge of OT technologies (9) 
- **Motive** - How motivated of the threat actors once they obtain access to the control environment? An excellent guide for this is the Impact column of the [MITRE ATT&CK ICS Matrix](https://attack.mitre.org/matrices/ics/).
  - No reward or intention to impact control environment (1)
  - Theft of operational data or equipment (2)
  - Create loss of view and control as a result of target-of-opportunity access to assets (3)
  - Limit access to fileshares and prevent view and control using common malware (5)
  - Prevent view and control using specially designed malware (6)
  - Manipulate view and control using privileged remote access and / or specially designed malware (8)
  - Prevent operation of safety equipment or cause a catastriphic failure (9)
- **Opportunity** - When accessing the control environment, how are the threat actors limited by deployed countermeasures? 
  - Physical access and local authentication is required and response time is less than fifteen minutes (0)
  - Physical access and local authentication is required but response time is more than fifteen minutes (1)
  - Physical and remote access is possible, local authentication required, and active monitoring is enabled (3)
  - Limited logging of physical or remote access but administrative privileges are required to access network devices, systems, and applications (5)
  - Undetected physical or remote access but administrative privileges are required to access network devices, systems, and applications (6)
  - Undetected physical or remote access that provides requires authentication to some network devices, systems, and applications (8)
  - Undetected physical or remote access that provides elevated permissions to network devices, systems, and applications (9)
- **Access** - What is the physical or remote access capabilities achieved by successful exploitation within the process? 
  - Physical owner/operator users (1)
  - Physical vendor / integrator users (2)
  - Physical non-malicious civilian users (4)
  - Remote owner/operator users (5)
  - Remote vendor / integrator users (7)
  - Physical malicious users (8)
  - Remote anonymous internet users (9)

## Step 2: Identify Vulnerabilities (ISA/IEC-62443-3-2 ZCR 5.2)

The identification of vulnerabilities depends on the type of vulnerability assessment being conducted. Every IACS/OT environment will have a list of vulnerabilities that are a combination of known hardware and software vulnerabilities, configuration vulnerabilities, and technology implementation vulnerabilities. Some vulnerabilities can be identified using online research i.e. the [NVD Vulnerabilities search page](https://nvd.nist.gov/vuln/search) and vendor cybersecurity resources pages. Configuration and implementation vulnerabilities are identified using passive and active vulnerability testing methods. The factors that play into rating identified vulnerabilities include:

- the ease of access
- the ease of exploitation
- public awareness of the vulnerability
- detection and response of attempts to exploit the vulnerability

### Vulnerability Factors

The next set of factors are related to understanding the identified vulnerability. The goal here is to estimate the 
likelihood of the particular vulnerability will be exploited and used to gain access to the environment, provide persistence on a system or device, and be used to achieve the threat actor's goals. To understand the vulnerability the factors involving access, exploitation, and public awareness should be considered. Additionally, one of the Foundational Requirements outlined in the ISA/IEC 62443 series of standards includes Timely Response to Events (TRE). This should be added to the factors when understanding the vulnerability within the IACS/OT environment.

- **Ease of Access** - How easy is it for this group of threat agents to access the environment and discover the existence of the vulnerability? 
  - Requires physical access to environment or OT device (1)
  - Requires physical access to environment or IT device (2)
  - Remotely accessible but countermeasures protecting OT technology (3)
  - Remotely accessible but countermeasures protecting IT technology (4)
  - Remotely accessible but no automated tools to discover for OT technology (6)
  - Remotely accessible but no automated tools to discover for IT technology (7)
  - Remotely accessible and automated tools available for IT technology (8)
  - Remotely accessible and automated tools available for OT technology (9)
- **Ease of Exploit** - How easy is it to actually exploit the vulnerability? 
  - No known proof of concept (1)
  - Countermeasures protecting OT technology (2)
  - Denial-of-Service possible but no code execution (3)
  - Custom scripts / tools can be made to exploit IT technology (5)
  - Custom scripts / tools can be made to exploit OT technology (6)
  - Automated tools available for IT technology (8)
  - Automated tools available for OT technology (9)
- **Awareness** - How well known is this vulnerability? 
  - Unknown OT Vulnerability (1)
  - Unknown IT Vulnerability (2)
  - Not publicly known but common [configuration vulnerability](https://cwe.mitre.org/) (3)
  - Publicly identified on vendor website or within [NVD Vulnerabilities database](https://nvd.nist.gov/vuln/search) but no known exploit available (5)
  - Publicly identified on vendor website or within [NVD Vulnerabilities database](https://nvd.nist.gov/vuln/search) no known exploit available but target threat actor group can develop exploit (6)
  - Publicly identified on vendor website, vulnerability databases, and exploit available in public forums, i.e. [Metasploit](https://www.metasploit.com/), [Exploit-DB](https://www.exploit-db.com/) (8)
  - Public identified and in [CISA Known Exploited Vulnerabilityes Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) (9)
- **Detection/Response** - How likely is an exploit to be detected? 
  - Centrally logged with alerts and formal review and response plan (1)
  - Centrally logged with alerts and formal review but no response plan (3)
  - Centrally logged with alerts, but no formal review or response plan (6)
  - Centrally logged and without review (7)
  - Locally logged without review (8)
  - Not logged (9)

## Step 3: Estimating Consequences and Impact (ISA/IEC-62443-3-2 ZCR 5.3)

The original OWASP risk rating methodology used a combination of technical and business impact factors to analyze the impact when a vulnerability's exploitation was realized. While useful these are not the best ways to understand the impact of an exploited vulnerability to an IACS/OT enviornment. The Impact column of the [MITRE ATT&CK ICS Matrix](https://attack.mitre.org/matrices/ics/) provides good details about what can happen after successful exploitation. These Impacts are a combination of denial, loss, and manipulation to the process or locations that monitor the process. The FIRST.og [CVSSv4.0](https://www.first.org/cvss/v4-0/) scoring system has been updated to include a new supplemental metric group which includes rating factors that involve safety, automatable, recovery, value density, vulnerability response effort, and provider urgeny. Hence, the IACS STAR will estimate consequences and impacts using technical factors and safety factors.

**NOTE**: Business impacts are still an important factor for rating risk. However, business impact factors are considerations that should be left to the Detailed Risk Analysis. The IACS STAR is designed to be used when rating the risk of vulnerabilities which feed into the Detailed Risk Analysis process. Therefore, the IACS STAR calculations will attempt to understand the safety impact factors rather than the business impact factors. (These may be added at a future date.)

### Technical Impact Factors

Technical impact can be broken down into factors aligned with the traditional security areas of concern: confidentiality, integrity, availability, and accountability. The goal is to estimate the magnitude of the impact on the system if the vulnerability were to be exploited.

- **Loss of Confidentiality** - How much data could be disclosed and how sensitive is it? 
  - No data lost (0)
  - Minimal architecture configuration data disclosed (2)
  - Minimal network configuration data but no device configuration data disclosed (4)
  - Extensive network configuration data and some device configuration data disclosed (6)
  - Some process network and device configuration data disclosed (7)
  - All process network and device configuration data disclosed (9)
- **Loss of Integrity** - How is the process data changed and does it impact critical functions? 
  - Modification of historical data not used for control (1)
  - Modification of historical data used for control (2)
  - Local modification of set points used for non-critical functions (4)
  - Remote modification of set points used for non-critical functions (5)
  - Remote modification of device configurations used for non-critical functions (6)
  - Local modification of set points used for critical functions (7)
  - Remote modification of set points used for critical functions (8)
  - Remote modification of device configurations used for critical functions (9)
- **Loss of Availability** - How are production and safety services impacted? 
  - Minimal production interruption and easily recoverable (1)
  - Device or service interrupted but process not impacted (3)
  - Production services temperarily interrupted by easily recoverable (4)
  - Production services interrupted but does not affect other processes (6)
  - Production services interrupted and impacts other processes (7)
  - All production services completely lost (8)
  - Loss of process safety functionality (9)
- **Loss of Accountability** - Are the threat actor actions traceable to an individual? 
  - Central logging, Multifactor Authentication (MFA), and cameras (1)
  - Central logging, Multifactor Authentication (MFA), but no cameras (2)
  - Local logging, Multifactor Authentication (MFA), and cameras but no central logging (3)
  - Local logging and cameras but no MFA and no central logging (5)
  - Local logging but no MFA, no central logging, and no cameras (7)
  - No local or central logging, no MFA, and no cameras (9)

### Safety Impact Factors

The business impact stems from the technical impact, but requires a deep understanding of what is important to the company running the application. In general, you should be aiming to support your risks with business impact, particularly if your audience is executive level. The business risk is what justifies investment in fixing security problems.

Many companies have an asset classification guide and/or a business impact reference to help formalize what is important to their business. These standards can help you focus on what's truly important for security. If these aren't available, then it is necessary to talk with people who understand the business to get their take on what's important.

The factors below are common areas for many businesses, but this area is even more unique to a company than the factors related to threat agent, vulnerability, and technical impact.

- **Environment Damage** - How much damage to the local environment, plant or public, will be realized by successful exploitation?
  - No environmental impact (0)
  - Environment damage limited by safety equipment, active, and passive protections (1)
  - Environment damage limited by active and passive protections (2)
  - Environment damage limited by passive protections only (4)
  - Safety equipment not remotely accessible and active and passive protections are in place (5) 
  - Safety equipment remotely accessible but active and passive protections are sufficient (7) 
  - Safety equipment remotely accessible and situation might overwhelm active protections but passive protections are sufficient (8) 
  - Safety equipment on production network and situation might overwhelm active or passive protections (9) 
- **Process Damage** - How much damange to the process equipment will be realized by successful exploitation?
  - No devices can be damaged and configurations cannot be modified (0)
  - Device or monitoring systems / applications can be modified but do not damage device or process (1)
  - Device device configuration can be changed but easily recoverable (3)
  - Device damaged requiring manual update but limited impact to process (4)
  - Device damaged requiring manual update but significant impact to process (6)
  - Safety equipment configuration changed but limited impact to process (7)
  - Safety equipment damaged but limited impact to process (8)
  - Safety equipment damaged causing process failure or automatic shutdown (9)
- **Safety Equipment** - How well are digital safety equipment deployed and protected?
  - Safety equipment not required for process (0)
  - Safety equipment required for process but not remotely accessible or on the same network as vulnerability (1)
  - Safety equipment required for process, remotely accessible, and requires MFA but not on the same network as vulnerability (2)
  - Safety equipment required for process and remotely accessible but does not require MFA and not on the same network as vulnerability (3)
  - Safety equipment required for process, remotely accessible, requires MFA, and on the same network as vulnerability (4)
  - Safety equipment required for process, remotely accessible, does not require MFA, and on the same network as vulnerability (5)
  - Safety equipment vulnerable and remotely accessible but requires MFA (6)
  - Safety equipment vulnerable and remotely accessible but requires authentication but no MFA (7)
  - Safety equipment vulnerable and remotely accessible but requires authentication but but default/hardcoded password in place and no MFA (8)
  - Safety equipment vulnerable, remotely accessible, and does not require authentication (9)
- **Recoverability** - How well is the organization / process team prepared to recover during successful exploitation?
  - Vulnerability will not require or limit recovery operations (0)
  - Process will automatically recover with no manual efforts (1)
  - Process will recover with minimal manual efforts (2)
  - Process will recover with extensive manual efforts (4)
  - Recovery not possible without vendor / integrator assistance (6)
  - Recovery not possible without limited government and vendor / integrator assistance (7)
  - Recovery not possible without moderate government and vendor / integrator assistance (8)
  - Recovery not possible without significant government and vendor / integrator assistance (9)

## Step 4: Estimating Likelihood (ISA/IEC-62443-3-2 ZCR 5.4) and Consequences

The likelihood that the exploitation of a vulnerability will be realized is defined by using the factors calculated when identifying threats and vulnerabilities. The scores of the threat and vulnerability factors, respectively, are added together and deviced by the number of factors used in the calculation. The resulting values can then be assigned a less granular catorization, such as low, medium, and high, to limit subjectivity and improve consensus. 

The same method will be used when calculating scores for concequences and impacts. The scores for the technical and safety impacts will be combined to achieve a low, medium, or high categorization.

To calculate the likelihood and consequences rating category the 0 to 9 scale is split into three parts:

![Scoring Categories](./images/istar_scoring_categories_20230721.jpg)

<!---
<table width="40%" cellspacing="0" cellpadding="5" border="1" align=center>
<tr>
<th colspan="2" style="color: black; background-color: white; text-align: center; border: 2px solid black">Scoring Categories</th>
</tr>
<tr>
<td width="50%" style="color: black; background-color: white; text-align: center; border: 1px solid black">0 to &lt;3</td>
<td width="50%" style="color: black; background-color: green; text-align: center; border: 1px solid black">LOW</td>
</tr>
<tr>
<td style="color: black; background-color: white; text-align: center; border: 1px solid black">3 to &lt;6</td>
<td style="color: black; background-color: yellow; text-align: center; border: 1px solid black">MEDIUM</td>
</tr>
<tr>
<td style="color: black; background-color: white; text-align: center; border: 1px solid black">6 to 9</td>
<td style="color: black; background-color: red; text-align: center; border: 1px solid black">HIGH</td>
</tr>
</table>
--->

## Step 5: Calculate Unmitigated Cybersecurity Risk (ISA/IEC-62443-3-2 ZCR 5.5)

Once the likelihood and consequence ratings have been determined the overall risk associated with the vulnerability can be calculated. This calculation will result in the unmitigated risk rating for the vulnerability which is required for input into the ISA/IEC 62443 Detailed Risk Analysis. The following table will be used to combine the resulting likelihood and consequence categories and assign and unmitigated risk score.

![Scoring Categories](./images/istar_unmitigated_risk_categories_20230721.jpg)

<!---
<table cellspacing="0" cellpadding="5" border="1" align=center>
<tr>
<th colspan="5" style="color: black; background-color: white; text-align: center; border: 2px solid black">Unmitigated Risk Score</th>
</tr>
<tr>
<th rowspan="4" width="15%" style="color: black; background-color: white; text-align: center; border: 2px solid black">Consequences</th>
<td width="15%" style="color: black; background-color: red; text-align: center; border: 1px solid black">HIGH</td>
<td width="15%" style="color: black; background-color: orange; text-align: center; border: 1px solid black">Medium</td>
<td width="15%" style="color: black; background-color: red; text-align: center; border: 1px solid black">High</td>
<td width="15%" style="color: black; background-color: gold; text-align: center; border: 1px solid black">Critical</td>
</tr>
<tr>
<td style="color: black" style="color: black" bgcolor=yellow align=center>MEDIUM</td>
<td style="color: black; background-color: yellow; text-align: center; border: 1px solid black">Low</td>
<td style="color: black; background-color: orange; text-align: center; border: 1px solid black">Medium</td>
<td style="color: black; background-color: red; text-align: center; border: 1px solid black">High</td>
</tr>
<tr>
<td style="color: black; background-color: green; text-align: center; border: 1px solid black">LOW</td>
<td style="color: black; background-color: green; text-align: center; border: 1px solid black">Informational</td>
<td style="color: black; background-color: yellow; text-align: center; border: 1px solid black">Low</td>
<td style="color: black; background-color: orange; text-align: center; border: 1px solid black">Medium</td>
</tr>
<tr>
<td style="color: black; background-color: white; text-align: center; border: 1px solid black">&nbsp;</td>
<td style="color: black; background-color: green; text-align: center; border: 1px solid black">LOW</td>
<td style="color: black; background-color: orange; text-align: center; border: 1px solid black">MEDIUM</td>
<td style="color: black; background-color: red; text-align: center; border: 1px solid black">HIGH</td>
</tr>
<tr>
<td style="color: black; background-color: white; text-align: center; ">&nbsp;</td>
<th colspan="4" style="color: black; background-color: white; text-align: center; border: 2px solid black">Likelihood</th>
</tr>
</table>
<br/>
--->

## Step 5: Reporting Vulnerabilities and Vector Scores

The assessment report is the most important part of an assessment. It provide context about the scope of the assessment, the assets involved, the communications between assets and across enforcement boundaries, the methodology used to gather information, details about the findings, and the vulnerability scores for these findings. The report should be clearly written to convey all of this information and the results should be reviewed by all stakeholders. Eventually the information from the assessment will be used in a Detailed Risk Assessment. To this end, like the OWASP risk rating methodology, the IACS STAR scores can be categorized using a vector score. This vector score provides the likelihood and consequence factor scores when rating the risk. This allows easy integration most automated risk and vulnerability management systems and scoring calculators. To aid the generationg of a vector score each of the factors have been provided an identifier. 

- Skill Level (SL)
- Motive (M)
- Opportunity (O)
- Access (A)
- Ease of Access (EA)
- Ease of Exploit (EE)
- Awareness (AW)
- Detection/Response (DR)
- Loss of Confidentiality (LC)
- Loss of Integrity (LI)
- Loss of Availability (LA)
- Loss of Accountability (LAC)
- Environmental Damage (ED)
- Process Damage (PD)
- Safety Equipment (SE)
- Recoverability (R)

Once computed the resulting score vector would be represented in the following format:

**(SL:0/M:0/O:0/A:0/EA:0/EE:0/AW:0/DR:0/LC:0/LI:0/LA:0/LAC:0/ED:0/PD:0/SE:0/R:0)**

# IACS STAR Calculator

The IACS STAR score calculator (website coming soon) has been set up to aid in the calculation of the IACS STAR vulnerability scores. This calculator follows the model provided by the [OWASP Risk Rating Calculator](https://owasp-risk-rating.com/). It can be used when considering the scoring of each of the factors that are used to calculate likelihood and consequence. It is intended to aid in discussions and to move towards concensus amongst stakeholders. It can also be used to provide the vector score to be added to the assessment findings.

To run this calculator locally clone this repository, open a terminal to the repo directory, and run a Python web server with the following command:

**python3 -m http.server 9000**

Open your web browser to your [local IACS STAR score calculator](http://localhost:9000/iacs_star_calculator.html).
