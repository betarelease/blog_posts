---
layout: post
title: '2 challenges, one healthcare app, continuously delivered (Part I)'
tags:
  - healthcare
  - glooko
  - fda
  - hipaa
  - regulatory
  - featured
---
(This post is the first part of a two-part blog, I will focus on continuous delivery in the second part.)
I spent the last 3 years in building and supporting an exciting HealthCare app - [Glooko](http://www.glooko.com) - which supports people with diabetes get a unified view of their health with a single click. When I started on it, I thought of it as another software application, but was about to learn a lot more. First, I learnt that Software is more useful when it can be applied to people's health. Further, I learnt that Software is also more difficult when it applies to people's health - well, rightfully so. Here I detail, what I learnt and why it makes sense to build it that way. <!-- more -->

# Process

Every aspect of a healthcare application that deals with patient data, can risk a patient's life. In order to regulate these applications and how they treat patient data, a couple of government bodies keep a close eye on these applications/organization.
[FDA(Food and Drug Administration](https://www.fda.gov/) is one of them, the other being the Congress which passes laws from time to time related to patient safety. [HIPAA(Health Insurance Portability and Accountability Act of 1996)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act) being the most applicable act.
Both of these emphasize on the process being used to build and maintain the application.

Lets look at both of them in a little more detail.

## Challenge 1: Working towards FDA approvals

In working with anything to do with healthcare, you will come across the FDA - who makes sure that we don't hurt the one who we sought to help. FDA is the central regulatory authority that approves and controls how technology is introduced into health treatments. In Glooko's case specifically, when you build a product that aggregates data, it comes under the scrutiny of FDA and needs to prove that the data is not going hurt the person - and while doing so is represented in its true form. FDA [classifies](https://en.wikipedia.org/wiki/Medical_device) any such technology as a medical device(deriving and continuing the use of the term as if physical devices being used to treat/monitor). Now, this definition extends to medical devices where devices include software.
FDA categorises your device into one of [three classes](https://www.fda.gov/MedicalDevices/DeviceRegulationandGuidance/Overview/ClassifyYourDevice/default.htm), from
Class I (where the device may not harm you if used/misused - like a toothbrush) to Class III(where the device could cause you harm if misused/miscalibrated - eg. [defibrillator](https://www.accessdata.fda.gov/scripts/cdrh/cfdocs/cfPCD/classification.cfm?ID=NIK)). As the class number goes up the sensitivity of FDA towards your medical device increases and stricter guidelines need to be followed to get approvals. These guidelines also need to be followed with each subsequent version of the medical device.

### Regulatory team

Working with the FDA is usually an in house regulatory team who spends a lot of energy in understanding the classification and preparing for such submissions. This team usually helps you dot the t and dash the i(you know it is the other way around) - which ends up being a lot of t's and i's. This regulatory team also trains the software team on what it means to work in the healthcare setup and work with the FDA.

### Patient Safety

The regulations from FDA are tuned to look at the process of building this software application. Based on the process some controls kick in, that require special attention.

#### How you built it

FDA requires that you document the process well. From the time you started thinking about a feature of the device/software, detailed documentation of the process that was used to build your software/device, is a must. This process includes your project management tool, how it is configured, how the work is tracked and traced traced. This tool needs to be able to connect the dots between feature description, design, code as well as the testing that was carried out to accomplish the release.

#### What you built

The feature itself needs to be meticulously described, test scenarios identified and reasoned with appropriate design documentation attached for completeness. When this particular feature has been built, a verification has to be conducted and documented that proves how the feature works. This verification documentation can also be used as part of the instruction manual document.

#### Traceability

When a feature is built, it could undergo some changes/tweaks based on what you learn during the development process. These changes need to be tracked. Once the development is completed the code changes need to be linked to the feature description so that incase of an audit this change path can be traced all the way back. This is also useful in case an issue is found after the release to allow for root-cause analysis.

#### Root cause and Root effect

When a particular issue is reported by a user, this issue is recorded in the project management system and an analysis will need to be performed to identify what caused the issue. Sometimes the user will experience the issue in a certain way, but data will not reflect exactly what the user experienced. Let me give you an example.

Let us say the medical device you have allows you to record a continuum of activity. Usually this continuum is specified by the start time and an end time. Now when this device is synced the activity is in progress and there is no end time reported(say nil). The bug is that the device recorded this a single point and not as a continuum due to lack of end time. But due to this bug the user experience calculations of integral, average that are completely off. The user reports this root effect as the calculations being wrong while the root cause is the fix the end-time should be considered as sync time for any activity that has started but not ended.(unless they are for a different date in which case those data need to be flagged for admin action)

The above example illustrates the nuances involved in storing the data as well and how the data is interpreted and affects the user. This scenario requires that you as a system designer perform this analysis and report and test the fix with that context.

#### Identify, fix issue, fix process, Repeat - and don't forget the documentation

Working in this environment and under these constraints means that you need a process, that is well defined to encourage adaptability. Picking the right tool to maintain all this process is also key. A number of tools support integration between your codebase, features/stories, test plans, test executions and code packaging and release. Selecting the right tools also helps alleviate the effort and pain of maintaining traceability. This tool can be configured to build the traceability of an entire feature from start to done with links to any related features/bugs. Even though it is a long and involved process setting this up correctly will go a long way.

## Challenge 2: Compliance with HIPAA

[HIPAA](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act) on the other hand is not related to FDA and operates independently in defining how patient data can be handled to improve patient safety. In the way it applies to healthcare software it governs security of patient data in ways that it is stored, accessed, handled and archived. A comprehensive [HIPAA compliance checklist](https://www.hipaajournal.com/hipaa-compliance-checklist/) documents it well.

### Security for patient data

HIPAA classified sensitive patient data is data that can be use to adversely affect a patient/person. In order to ensure that data is protected HIPAA details what is expected of data in its different states. When we talk about patient privacy it means the ability to restrict the data is available to identify the patient as well as  information about their health.
While securing this data controls have to be in place to protect the patient's credentials, patient's identifiable's which include their name, their date of birth, their passwords, address etc. Along with this it is important to let their health data be alterable, without their permission.

#### Security while storing patient data

When handling patient data one has to ensure that the data is stored in a secure fashion. What it means, is that if anyone is able to get to the storage they are not able to read or write to that data store without proper credentials with the most current encryption mechanisms. Doing this ensures that data is secure at rest. HIPAA also mandates how the storage devices must be managed and how they need to be destroyed.

#### Security while accessing patient data

Securing data in storage seems to be an easier problem solve. Once you have ensured that, the next challenge is secure it in transit. In order to do that you need to ensure, that the path of the data from storage to the user stays secure. It is very critical to monitor how how these channels that is handled and manipulated by your infrastructure as well as your application. Since this application is delivered over the web you need to ensure all communication is delivered using HTTPS with the highest level of encryption. It also helps with credibility if you are able to obtain the appropriate SSL encryption and appropriate [SSL certification](https://www.instantssl.com/ssl-faqs/types-of-ssl-certificate.html).

#### Security while monitoring patient data

When it comes to patient data it is mandated that the access and usage of that data be monitored correctly. Audit logs need to ensure that only the patient can access their own data. If a patient explicitly shares their data with their care provider, audit logs needs to track these events and show their enforcement. Along with that, you need to keep your logs clean of any patient identifiable information. Much like passwords need to be masked for web security, patient identifiable data needs to be obfuscated to be not printed in log statements. A proper log etiquette/rule will ensure that no patient identification data leaks into the logs.

#### Security while archiving patient data

All of the above protects patient data when it is hot - or being used/reviewed. Business Continuity practices require that you have an archival strategy for all your data - in order to support quick disaster recovery. Needless to say, even while archiving snapshots of data, the data needs to be encrypted and the encryption key needs to be protected from unprivileged usage.

### Tools and Technologies to help with data security for HIPAA compliance

#### Code quality

The team that builds a medical device needs to adhere to quality standards that are exceptional. NIST defines some of those standards. Along with those, a number of software engineering techniques can be applied while building such quality into the device. These tools should at a minimum verify the scans against [OWASP standards](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

#### Code scanning

[Static code scanning tools](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis) help with scanning code for known vulnerabilities and inconsistencies. [Static code analysis](https://en.wikipedia.org/wiki/Static_program_analysis) has been around from the old C++ days so I will not belabor the point of such analysis here.

#### Application scanning

A number of [dynamic code scanning tools](https://www.owasp.org/index.php/Category:Vulnerability_Scanning_Tools) scan for security vulnerabilities, normally from outside in. When these tools look only at public interfaces and operate outside the system they perform blackbox scans.

Another scanner called the [Vulernability scanner](https://en.wikipedia.org/wiki/Vulnerability_scanner) usually sits inside the network and performs host based vulnerability scanning. This host can scan the internal network to identify any vulnerabilities that are exploitable.

#### Infrastructure Monitoring - Intrusion Detection

Sometimes also referred to as Threat Detection, Intrusion detection is a mechanism that monitors all activity inside your infrastructure to ensure only valid processes are run by authorized personal. In case of any unknown/unauthorized processes are observed/or privilege breaches are seen, this system can send system level alerts to the team. Thus allowing the team to at a minimum verify, at best thwart unexpected activity. Typically, a bot sitting on the hosts monitors this activity and reports to the team.

#### Penetration testing

Penetration testing is an activity performed less frequently as compared to all the above operations. In case of penetration testing usually a team of 'people' try to device ways and means to attack the system in order to discover its vulnerabilities. For such a test to be carried out, you are required to assemble a team that is knowledgeable about security tools as well as can understand how the system under test works. It is recommended that this be a third party team, so that all biases can be removed. This team springs into action, typically every 6 -12 months and conducts an elaborate penetration test. Once this test is conducted the development team is required to address any vulnerabilities found, after which this test will be repeated to report closure.

#### SLA

Service Level Agreement is a guarantee of availability of service and usually defined as a percentage.
SLA which is usually quoted as 99.99999(with as many number of 9's that fancy the conversation) and people focus too much on the number of 9's after the decimal instead of looking at the factors that contribute to SLA and quality of the SLA.
The reason SLA is so important in healthcare application, is because when the application is unavailable patient is at risk since they depend on the data to make health related decisions. Hampering that ability is itself a safety issue.
SLA is also sometimes interpreted as downtime(which is only a part of the service provision). SLA as I understand, encompasses the duration when the system was operational, time when the system when down, duration it was down for, the manner in which it degrades etc. Lot of relevant software development discussion attempts to provide solutions for improving and tracking SLA.
[Release It](https://pragprog.com/book/mnee/release-it) describes some techniques of doing it for distributed and production ready applications.

# AWS as your partner

AWS(Amazon Web Services) has come a long way from it initial days where hosting on AWS required that your software be amenable to be deployed into public. Now AWS provides additional services that allow it to comply with most of the regulations like HIPAA, [PCI](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard), or [FedRAMP](https://www.fedramp.gov/) for that matter.

### HIPAA compliance on AWS

For a healthcare service the regulations from HIPAA, require that any patient data be completely destroyed from any storage once that storage has retired. AWS complies with all these regulations and now allows you to sign a Business Associate Agreement(BAA) where they support your infrastructure while helping you with regulation compliance. This agreement covers you and on your behalf it is required that AWS provide you hardware to comply. This agreement lets you leverage a number of AWS services used for provisioning of your resources. Only the resources tied to hardware need to be certified for usage. AWS has thus made itself a very compelling platform for all healthcare software.

Now that we have covered some of the main challenges of building a healthcare app and making it viable/trustworthy, we will look into how we delivered it continuously in a subsequent blog post.

Related on the internet

- "Nobody knew healthcare could be so complicated" - said famously by a popular personality.
- [How a healthcare tech could not get her bill adjusted](https://www.cnbc.com/2017/12/19/social-capital-kristin-baker-spohn-health-bill-horror-story.html?lipi=urn%3Ali%3Apage%3Ad_flagship3_feed%3BTop78XymS6exZxtF0G1VAQ%3D%3D)
