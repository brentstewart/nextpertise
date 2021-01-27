---
title: "How to Plan Good Downtime"
date: 2021-01-26T08:18:24-05:00
draft: true
author: "Brent Stewart"
Victor_Hugo: "true"
Focus_Keyword: "IT"
picture: "life"
github: ""
youtube: ""
refs: [""]
tags: [""]
---
In another blog, planning downtime would involve tropical pictures.  Sigh.

## Downtime
![Time](https://buidln.clipdealer.com/000/183/082/previews/1--183082-time%20abstract.jpg#floatsmallright)
IT downtime is a period when IT systems have a planned outage.  These periods are negotiated with internal and external stakeholders and communicated so that no one is surprised by the lack of availability.  During this time, changes take place to improve performance, enhance reliability, address security concerns, or add features.

It is a good idea to have a recurring scheduled downtime at least once a month.  Most organizations sync their downtime to "patch Tuesday".  Microsoft, Oracle, and other big organizations release patches on the second Tuesday each month.  Since there's a good chance that something in an environment will need to be patched and this leads to a disruption, it's a good time to do other changes as well.

Sometimes changes must be deployed to react to events.  In that case the process is hastily put together and it's more critical to have a clear idea of how to structure that time.

![Plan](/plan.jpeg#floatsmallleft)

## Checklist for Good Downtime
A six step process for thinking it through.

- [ ] Plan testing
- [ ] Communication Plan
- [ ] Build a Script
- [ ] Recognize Risks
- [ ] Negotiate Window
- [ ] Make the change!

## Plan Testing
There's an old adage to "start with the end in mind" and that's appropriate when planning changes.  Start by thinking through _what_ should be tested to confirm success and _how_ it will be tested.

Circulate that testing plan within the parties of interest and allow them to modify the tests.  There are cases where a change passes testing but problems are later revealed.  In such cases, you will _always_ be asked "Didn't you test for that?"  Having those other folks review the testing plan provides space to skip the blame game in those cases and focus on restoring service.

How does this apply to "emergency" changes?  Build a "Crown Jewels" testing list.  These are the critical services for your organization.  Any testing plan should include these tests, but in a pinch this can serve as a base level set of tests.

You'll want to automate these tests as much as possible.  Automation allows you to easily re-run the tests many times as you make adjustments.  "Ping scripts" are a good place to start, but be creative (for instance, wget can serve to test browsing).
![Process](/process.jpeg#floatsmallright)
Finally, make sure to include testing of monitoring.  If you expect alarms when a redundant power supply fails, test that.  When you make a change, confirm that your change didn't interfere with critical monitoring.

## Communication Plan
With a testing plan in place, the next step is to build a communication plan.  _Who_ should be updated, _how_ should they be updated, and _when_ or _how often_?

Updates need to go to your management, the equipment owners (internal or external), and the group that is dependent on the service.  Generally, updates to users should be to the point and limited to how the change impacts their use.

Updates should be sent at critical points in the process:
* When the change is proposed
* At the beginning of the change window
* If the change needs to be backed out, when that decision is made
* When backout is complete, if applicable
* When the change is complete

Keep in mind that your change may impact your ability to get to corporate directories phone systems, or email.  Make sure you have a way to communicate with this group that is not dependent on things affected by the change.  I usually use corporate email as the primary path, but have cell phone numbers "just in case".

Another aspect of communications is setting up the time for the change, getting it on people's calendars and insuring they can participate, and publishing the meeting information.

## Build a Script
A downtime script can be as simple as a spreadsheet.  It needs to have columns for description, time, and responsible party.  Each row describes a task.  The _time_ column tells when that task should be complete.

Having a script allows the team to do a dry run and to be aware of what others are doing.  It makes it easier to coordinate and easier to track whether things are progressing as expected.

This process also applies to the backout script.  Understand how the change will be unwound, roles, and make sure you preserve adequate time to accomplish it.

A word on time estimates - their going to be wrong.  Stuff always comes up.  Still, a good faith estimate of each steps helps you to get a sense of where things stand relative to expectations at any point.


## Recognize Risks
Recognizing risks in a change allows that risk to be communicated to stakeholders so that it can be accepted.  Anticipating likely scenarios also provides opportunities to mitigate.

Is there a risk of disconnection to a remote facility?  Plan on having someone on site or available.

Is there a risk that an upgrade doesn't work?  Have a backup.

_One risk that isn't often discussed is the risk that comes from asking your family to tolerate your work schedule.  As you prep for the activity, take some time to be at home (awake, alert, and agreeable) and spend time with them, especially if your work means that you have to be up all night Saturday night!_

## Negotiate Window
Changes take place within a window.  This period of time needs to be agreed upon by everyone, short enough to minimize the disruption for users and long enough for the IT team to complete it's work with quality.

One question that needs to be asked - what happens if the work isn't complete by the close of the window?  In other words, is it a "hard window"?  Unless otherwise advised, assume that it is.

Take the time estimate from your script (above) and apply a "confidence" factor of between 50% and 100%.  In other words, if the script calls for thirty minutes, assume it will _really_ take between 45 minutes and an hour.  This helps to account for the unknown but inevitable stuff that pops up when working.  Adjust your confidence factor over time as you gain insight into task complexity, team competence, and your ability to estimate.

Next, make sure that a third of the time is for backout.  This is time to work through the recovery process if the change goes awry.  For a half-hour change, we'll budget an hour for implementation.  If at the end of the hour it's not done, we immediately go into backing out the change so that we're done within an hour an a half.  And that's the right period of time to ask for - 300% of your estimate.  Unless you can get more.

Seriously though, this drop-dead time to begin backout is crucial to being able to have integrity about honoring the window.  IT folks are always "5 minutes!" away from fixing things and can easily get lost in a spiral without maintaining discipline about when the backout has to begin.

## Make the Change!
Finally it's time to make the change.  Ask everyone to arrive early and join the conference.  Make sure that backups are up to date and configurations saved before beginning.  Also before beginning, run the test script to make sure that all elements are working before the change to prevent confusion afterward.
![Winner](/winner.jpeg#floatleft)
If the change is a group exercise, it is usually a good idea to have one person act as tracker.  In addition to making sure that the team is ready to begin backout at the right time, the tracker can check off items as they are accomplished, recognize any slippage, and suggest strategies to keep things moving. 

It's a good idea to communicate to stakeholders at the beginning of the window.  

Run the test script again when things are complete (win or lose).  Each script run should produce output, which can be saved, in case there are questions afterward.  

Finally, communicate to stakeholders when things are complete.

I realize the process is complicated, but it encompasses _years_ of hard-won ideas.  Each point has a story about the incident where it was learned.  However, well planned exercises run smoothly and feel like a victory lap, making all the effort worthwhile.  I sincerely hope that this helps you to move swiftly past these learning experiences and on to greater success!