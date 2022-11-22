---
title: "Blameless Post Mortems"
description: "An introduction to Blameless Post Mortems"
author: "Brent Stewart"
date: "2022-11-17T16:00:14-05:00"
markup: 'mmark'
math: false
draft: false
Victor_Hugo: "true"
picture: "leadership"
Focus_Keyword: "post-mortem"
youtube: ""
github: ""
refs: 
  - "https://www.datadoghq.com/blog/incident-postmortem-process-best-practices/"
  - "https://www.bts.gov/content/us-general-aviationa-safety-data"
tags:
  - "leadership"
---

If you support IT infrastructure, the goal is to minimize issues (particularly issues with big impacts).  Some issues can be anticipated and pre-mitigated.  Considerable success can be achieved but _perfect_ is not for this life.  As a leader, I want to ensure that we learn and improve from every issue.  One way to do that is through a _post-mortem_ process.

I had a mish-mash of approaches that I used from reading Atul Gwande.  If you haven't read his books, I am amazed at how the stories of the history of surgical improvements can be applied to IT and highly recommend them (a good one to start with is _The Checklist Manifesto_).  I was inspired to strengthen and standardize the use of post-mortems from a presentation given by Data Dog at an AWS conference.  This article describes my experiences.  I've also linked to an excellent article from Data Dog.

## History of Post-Mortems
The concept of a Morbidity and Mortality conference originated with Ernest Codman, who worked at Massachusetts General in the early 1900s.  It should be noted that the idea of reviewing the mistakes of peers was not initially well received.  Their use over time has helped to instill best-practices, avoid repeating errors, and driven increasing success in patient care.

These conferences are held regularly at most hospitals.  At the meeting, selected cases are presented by the responsible doctor.  They lay out the details of the case, approach used, and progression of symptoms.   M&M is particularly good at building accountability and drawing out errors in process and communication.  This process is so integral and well respected as a part of medicene that the presentations and discussion are not legally "discoverable" in most states to allow doctors to freely discuss these cases.

The National Transportation Safety Board (NTSB) and Federal Aviation Administration (FAA) use a similar approach when investigating a airplane crash.  Information and timeliens are assembled, involved parties interviewed, and the conclusions are fed back to the community.  This has led to a drop in annual fatalities from 6.15 (1965) to 2.11 (2010) per 100,000 flight hours.

## Using Post-Mortems in an IT setting

In my experience, blameless post-mortems should be incorporated into weekly meetings (although events with large impacts usually require a seperate discussion).  I ask the ticket lead to present the case.  

"Blameless" is a big part of success - I have found that, while no one enjoys being second guessed, most folks want to do good work and that they embrace the accountability, support, and forgiveness of their teammates.  They want to improve and not repeat a bad experience.

 It's important to recognize carelessness and sloppy work with consequences.  This is a critical part of leadership - __it's not what you say, it's what you tolerate__.  When that's the case, folks may need to be assigned a mentor, work on less challenging projects, or even be reassigned.  
 
 Generally however, these discussions will find some flaky piece of tech that - when mixed with process confusion and communication issues - leads to fiasco.  When that's the case, the team owns identifying the technical workaround and suggesting process improvements and leadership owns ensuring that those suggestions are carried out and adhered to.  Done well, this process builds ownership and accountability to each other within a team. 

I've found that teams can process a limited number of these, so I select interesting cases for review.  Certainly every SEV1 needs to be reviewed - I typically do that in a dedicated meeting and it leads to a full Reason for Outage (RFO) document.  

### Weekly Reviews

I tried to review everything at first, but that got impractical and frankly repetitive.  These days, I try to pick three cases that might lead to interesting discussions in weekly meetings.  The idea here is to catch issues before they have a big impact.  I forewarn the implicated people so that they can prepare, and I ask them to consider four lines of questions:

* What happened?
* Response
* Resolution
* Deltas

I've found that I can do this "short version" with each event taking about 5 minutes.  I document the answers to those four questions in my meeting notes so that we can refer back to them later, if needed.

### RFOs
For RFOs, I usually ask the lead person to write up a report using the bullets above as sections, although we can combine events and response on a timeline.  The report usually settles everyone down and helps establish that the response is under control.  This report needs to be more formal and the areas are probed much deeper.  To help with that, here's an expanded interogative version of those four areas.

* What happened?  How did we learn of the issue(and should that be automated)?  Did we get the right metrics?  A timeline is useful, and email and IM are helpful resources to get times.

* How did we respond?  Did it go well?  What was the impact?  How did we communicate?  Communication in the event and after the event is usually the difference between it being classified as a fiasco or learning experience.

* What resolved the situation?  Did we follow the appropriate processes?  If yes, were they effective?  If no, why not?

* What needs to change going forward?  Note that this might be directly related to the issue or just something that was noticed in the process (like labeling to make response faster).

## Review
This process, to be successful, requires a commitment to follow-up on the findings and drive them to conclusion.  The conclusion of some may be that risk has to be communicated and accepted (for instance, where capital isn't available to purchase new equipment).  Many conclusions, as previously mentioned, will be around tecnique, process, and communication and they are absolutely addressable.  I typically put all the reports into a shared directory or repository and review them as a team periodically.  I also add specific  follow-ups as tickets (and note the ticket number in the RFO), so that the team has integrity about following through.

## It's about team improvement
Here's the punch line: it's about growing the capability of the people you work with.  For this to work (for me at least), it has to come from a place of good will.  That is frankly difficult when things go pear-shaped.  Leadership has to believe in the team, support the team, and want to see that team succeed.  There is certainly a time to deal with underperforming members, but this is not that time.  Focus on the problem, then focus on never having that problem again, then focus on the confidence and preparedness of the team so that they are ready for whatever comes next.