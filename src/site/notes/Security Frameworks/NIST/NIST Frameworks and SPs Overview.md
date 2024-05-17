---
{"dg-publish":true,"permalink":"/security-frameworks/nist/nist-frameworks-and-s-ps-overview/"}
---


### What's the recommended order for study leading to the 800-53?
This guide ([Complete Guide to the NIST Cybersecurity Framework — RiskOptics](https://reciprocity.com/resource-center/complete-guide-to-the-nist-cybersecurity-framework/)) provides a great 
1. [[Security Frameworks/NIST/NIST CSF/NIST CSF\|NIST CSF]]
	1. **Purpose**: The CSF provides a high-level overview of cybersecurity concepts and outlines six core functions: Govern, Identify, Protect, Detect, Respond, and Recover. Understanding the CSF helps grasp the broader objectives of cybersecurity practices that SP 800-53 aims to support with specific controls.
	2. **Relevance**: It sets the stage for understanding the risk-based approach to selecting and implementing the appropriate controls detailed in SP 800-53.
2. [[NIST SP 800-37\|NIST SP 800-37 - RMF]]
	1. **Purpose**: SP 800-37 guides the implementation of the RMF and explains how to integrate security and risk management activities into the system development life cycle.
	2. **Relevance**: SP 800-53 is used within the RMF as the catalog of controls for organizations to implement based on their specific risk assessments. Understanding the RMF is crucial for knowing how and why specific controls from SP 800-53 are selected.
3. [[NIST SP 800-39\|NIST SP 800-39 - Managing Information Security Risk]]
	4. **Purpose**: This document provides a structured approach to managing risk at the organizational, mission/business process, and information system levels.
	5. **Relevance**: It helps understand the broader context of organizational risk management, within which SP 800-53 controls are applied.
4. [[NIST 800-30\|NIST 800-30 - Guide for Conducting Risk Assessments]]
	1. **Purpose**: SP 800-30 provides detailed instructions on conducting risk assessments, which are crucial for identifying threats, vulnerabilities, and impacts.
	2. **Relevance**: Knowing how to assess risk is essential for appropriately applying and tailoring the controls in SP 800-53 to an organization’s specific needs.
5. [[NIST 800-53A\|NIST 800-53A - Assessing Security and Privacy Controls]]
	1. **Purpose**: This publication serves as a companion document to SP 800-53, focusing on assessing the effectiveness of the implemented controls.
	2. **Relevance**: Understanding the assessment process helps ensure that the controls detailed in SP 800-53 are not only implemented but are also effective and functioning as intended.
6. [[Security Frameworks/NIST/NIST 800-53/800-53R5\|NIST SP 800-53]]




## Difference between NIST frameworks:
[What is the main difference between these 3 framworks? : r/cybersecurity](https://www.reddit.com/r/cybersecurity/comments/11h09bz/what_is_the_main_difference_between_these_3/)
- NIST 800-53 - This one was made specifically for the US federal govt. To quote _"The use of these controls is mandatory for federal information systems10 in accordance with Office of Management and Budget (OMB) Circular A-130 "_ As the name implies it's very specific around controls.
- NIST CSF - Again to quote the NIST site: " The Framework is based on existing standards, guidelines, and practices for organizations to better manage and reduce cybersecurity risk. In addition, it was designed to foster risk and cybersecurity management communications amongst both internal and external organizational stakeholders." Think of this one as being a bit "higher level" than 800-53 as it's more focused on an overall program then just controls.
- NIST RMF - " The NIST Risk Management Framework (RMF) provides a comprehensive, flexible, repeatable, and measurable 7-step process that any organization can use to manage information security and privacy risk for organizations and systems and links to a suite of NIST standards and guidelines to support implementation of risk management programs to meet the requirements of the Federal Information Security Modernization Act (FISMA)." As stated this one is more focused on risk and was again created for federal entities to use to satsfy the requiremetns of FISMA. It would be part of what you do in the NIST CSF. 800-53 and the CSF work hand in hand with this.

