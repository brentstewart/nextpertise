---
title: "Strategy vs Tactics"
description: ""
author: "Brent Stewart"
date: "2025-10-15T20:30:47-04:00"

math: false
draft: false
Victor_Hugo: "true"
picture: "Leadership"
Focus_Keyword: ""
youtube: ""
github: ""
refs: ["Leadership"]
tags:
  - "Leadership"
---

In my research on John Boyd, I came across a discussion of Strategy and Tactics that is worth revisiting as it applies to IT leadership.

#### Tactics involve our plans around the known and controlled, strategy is our plans around the unknown and uncontrolled.

Tactics adress things that we understand and control. For example, I know that my team has a certain number of vacation days left and that there are X-many working days remaining in the year.  I will develop a tactic around the use of vacation days.  This is a really good example, because we can imagine a variety of tactics that are all valid.  For instance, I can choose to deny vacation or to take a laise faire approach.  This tactic might not be popular or respect the people I work with, but at the same time we can imagine an extreme case that demands "all hands on deck" where this might be appropriate.  A more conciliatory tactic might be to proactively work to make sure that everyone has a chance to use their vacation days.

Strategy is focused on things we don't understand or don't control.  A major software supplier will release an update and our IT team has to think through requirements to buy compute _today_.  If that supplier isn't publishing details, we may not know the appropriate hardware.  Our strategy might be estimate and then massively overbuild or to provide for an easy upgrade path.  Alternately, our strategy might be to wait for the official release and only purchase servers when the requirements are known.  At the same time the hardware saga is playing out, we might also be concerned about the upgrade strategy.  Enterprise software is typically customized to some extent and new releases have to have those customizations ported.  We also need to get data from the old system to the new system.  Again, we're limited by what we don't know but we might develop a strategy around enumerating and documenting customizations and backing up data.

## Focus
I find that these concepts help explain roles and the types of leeway we provide employees.

An individual contributor is mostly going to focus on working through a tactical plan.  The Toyota Production System has a concept that the guy turning the wrench has a say in how a job is accomplished, and that ethos carries through to IT.  These folks need to understand the tactical problem, the approach, and understand how to innovate within that window.

Managers recieve strategy and develop tactical plans.  Managers have a broader view of where the "known unkmown" is and help their team navigate in that structure.  They typically have experience in the environment and can set objectives, develop an approach to achieving that objective, and coach the team on techniques that might be helpful.

Directors and VPs typically work at the level of strategy.  They might look at business objectives like a sales approach and targets.  Directors typically develop strategic concepts with VPs supplying the ultimate direction. _Grand Strategy_ deals with direction and alliances and is mostly reserved for the executive suite and Boards of Directors.

## The Role of Risk
Based on my understanding of Nassim Taleb, I characterize risk as "within normal" and outlier risk.  An example of normal risk is the risk of a given part being non-functional on delivery.  For example, if I order a thousand SFPs (fiber connectors) then I might expect a certain percentage to be bad and make allowances.  AI represents extraordinary risk, in the sense that the parameters and variations seen in the past twenty years might suddenly no longer apply.  In 2024, we experienced a series of heavy storms followed by a hurricane.  In short order, the foothills and mountains near my home received almost a meter of rain.  No one had that on their bingo card.

Risk maps into a discussion of strategy and tactics.

Grand Strategic risks are the most difficult to characterize and backstop.  Company leadership has to understand larger trends - such as how AI will impact our business - as well as the potential for complete disruption happening from an unexpected source _a la_ COVID.

Strategic risks deal with unknowns, so a strategy has to feel out those unknowns and be prepared to quickly adapt.  Napoleon would divide forces on a broad scale to obfuscate objectives while providing support for each group.  His subordinates followed a "plan with branches" that contained general concepts on how to exploit situations that may occur.  The American Civil War generals Jackson and Sherman both used similar concepts to manuever into positions that threatened multiple objectives, which forced opposing groups into difficult decisions and allowed them to exploit opportunities.

In a similar way, IT strategy should use OODA-based approaches to empower subordinates down to the individual contributor to adapt to resistance, re-orient, and quickly exploit opportunities by updating tactics.  In fact, one thing seen in the military examples is that speed and manuever are critical weapons and that speed is prohibitively difficult in heavily heirarchical organizations.  Speed is accomplished through trust, which takes time and purposeful investment to create.  Speed is executed through devolving decisions and crisp communication, things that I think are best build on Boyd's OODA framework.  Distributing decision making doesn't distribute accountability however, and leaders who expect results _have_ to invest heavily in relationships long before the bell rings.

Tactical risk is the most practical risk.  What if FedEx doesn't deliver the part in time?  What if this change results in an outage?  Tactical risk is handled by those closest to the action.  Common responses to tactical risks include spares, backups, and redundancy.  Just thinking about tactical risks however, it's impossible to eliminate failure.  Simple math or a monte carlo run will demonstrate that even redundant components with low failure rates will - eventually - both fail at the same time.  Part of the tactical response then is to enumerate and communicate those risks and to develop contingencies to minimize downtime.

Google, in their SRE book, has a rule-of-thumb that "adding a 9" (that is, going from 99.9% uptime to 99.99%) doubles costs.  I don't know that is true, but it's true enough.  It's therefore important to understand when the cost of adding one more nine exceeds the business cost of extended outage.

## Takeaways
1. Tactics deal with knowns, Strategy with unknowns
2. Tactics address risk through redundancy
3. OODA techniques are a useful way to quickly address "facts on the ground"
4. Strategic risks can be addressed through a plan with branches.
5. Recognize that speed and maneuever are a strategic asset (maybe the most important).