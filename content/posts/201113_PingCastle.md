---
title: "Active Directory Security Assessment with PingCastle"
date: 2020-11-13T10:30:00-00:00
draft: false
Victor_Hugo: "true"
Focus_Keyword: "PingCastle, AD Security"
picture: "windows"
author: "Myron Estibeiro"
refs: ["https://www.pingcastle.com/"]
tags: ["Security"]
---

Microsoft Active Directory (AD) is one of the technologies that is predominantly used in several organizations around the world. While we all know its purpose, most of the time it keeps growing or gets convoluted with uncontrolled Organizational Units, ever growing Users, ever increasing Computers, Policies, Integrations etc. While the Administrators remain busy in putting out the fires at work, they rarely get time to optimize, tune, clean or deploy best practices on the AD, and lock it down by implementing good security controls.

Enter PingCastle. I found this tool through an auditor who was using mimkatz to perform a pentest on an AD, and I found it to be super valuable. This may help some of the AD Admins to quickly identify some of the areas that they can fix within their AD deployments.
The entire Pingcastle project is written in C# and it is available on GitHub, located at: https://github.com/vletoux/pingcastle

It is an AD security assessment tool, designed to quickly assess the AD security level with a methodology based on a risk assessment and maturity framework.

![PC](/pingcastle.PNG#center)

__Features of PingCastle__
1.	Health Check - This is the default report produced by PingCastle. It quickly collects the most important information of the Active Directory and establishes an overview. Based on a model and rules, it evaluates the score of the sub-processes of the Active Directory. Then it reports the risks.
2.	Active Directory Map - This report produces a map of all Active Directory that PingCastle knows about. This map is built based on existing health check reports or when none is available, via a special mode collecting the required information as fast as possible
3.	Consolidation - When multiple reports of PingCastle have been collected, they can be regrouped in a single report. This facilitates the benchmark of all domains
4.	Scanner - Checking workstations for local admin privileges, open shares, start-up time is usually complex and requires an admin. PingCastle’s scanner bypasses these classic limits. This feature is awesome and can help to cover some of the most blind spots on the network that are often overlooked

To learn more about this tool, please visit https://www.pingcastle.com/

