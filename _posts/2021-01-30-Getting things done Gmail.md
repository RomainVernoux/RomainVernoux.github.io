---
layout: post
title: "Getting Things Done with Gmail"
date: 2021-01-30
categories: productivity english
---
*This post, originally published in 2017 as "Getting Things Done with Inbox" has been updated to reflect the shutdown of Google Inbox in March 2019 and the addition of its main features to Gmail.*

As a software developer, I read and reply to a large number of emails.
Counting both my professional and personal accounts, I go through about a hundred emails every day, even though I don't spend more than 30 minutes in my mailbox.
I am usually very quick to answer, I don't "lose" emails, and I don't let them interrupt me.
For that, I fully rely on some core ceatures of *Gmail*.
In this article, I will discuss fundamental issues of emails as we know them, then explain how David Allen's GTD ("Getting Things Done") method tackles these issues.
Finally, I will describe how Gmail can be used as an easy and very efficient implementation of this time management system.

## The problem with emails

For good reasons, emails have become the preferred communication medium inside companies, especially large ones.
They bring the same benefits as real letters but reach their recipients anywhere on the globe in a few seconds and are virtually free.
The problem is that emails are so easy to use that we ended up using them too much.
Most people feel that emails make them less productive, and some have already reached the point where they can't keep up.
Do you have one of those always growing, overflowing mailbox in which messages stay unanswered or even unread despite the time you put in?

### Pull-based and push-based communication

Let's start with the obvious.
Interruptions are terrible for your productivity, but they are not peculiar to emails.
Text messages, social networks notifications and phone calls also causes interruptions.
And so does a human being walking to you for a quick question! All these are examples of what I call *push-based communication*: information is *pushed* your way without your asking.
I oppose *push-based* to *pull-based* communication, in which you decide to *pull* information from somewhere and consume it.
Pull-based media such as news websites, social networks feeds and forums are less trouble to your focus as they don't provoke interruptions but are a mere temptation.
There seems to be a universal recipe to retrofit pull-based systems into push-based ones: make them accessible from a smartphone and add notifications (as a matter of fact, smartphone notifications triggered by server-side events are called *push notifications*).
Think about it: in just a few years, all our communication channels have become push-based.

Of course, I am not saying that you should simply consider disabling email notifications and call it a day (but do consider it when it comes to social networks and chats!).
Sometimes you just need to be interrupted and react quickly.
What you really need is an efficient secretary, buffering low-priority messages for you to reach in a pull-based manner in due time and interrupting your work everytime something urgent comes up.

### All emails are not equal

To be more precise, I believe there are a just a few categories of emails:

- **Status reports** such as automated messages you receive from service desks, issue trackers, continuous integration and monitoring systems.
  Except when something goes wrong, they are just here to keep you posted.
  You skim them, tell yourself "good to know" and delete them.
- **Notes**, for instance meeting reports or memos.
  Nobody really knows when they will be useful, but they contain important information you need to be able to get to anytime.
  They are the archetypes of emails to archive.
- **FYIs** that is, emails you receive as a *For Your Information*.
  As your team grows big, exchanges will require to keep more than the sender and the recipient in the loop, and you quickly find yourself in the *cc* field of many discussions.
  You read these emails quickly, check that everything is okay, seldom give your input, and archive them for future reference.
- **Immediate action requests**, which you are expected to treat as soon as you are available.
  These messages could be simple questions from colleagues, messages from your boss asking for a file, or even a request from a client that you should translate into a technical issue in your tracker...
  You do what needs to be done and almost never come back to them.
- **Future action requests**, which require actions that cannot be accomplished right away, either because you don't have the information you need to answer or because it is not in your priorities at this point.
  These could be complex questions that you need to investigate, requests to start working on a subject, bills to pay or administrative procedures to go through, etc.
  You read them and when you understand this cannot be done in a few minutes, you leave them in you mailbox and get back to work.

And here is the issue.
Before you know it, *status reports* pile up in your inbox at an incredible pace.
*Notes* and *FYIs* mixed together get lost in the scrum in such a way that they are quite hard to find again.
As soon as your current task requires more than an hour, any *immediate action request* hides itself below a stack of twenty other emails, and *future action requests* (even when read) disappears forever in the one-thousand-email limbo.
Every six months, you spend an hour or two scratching the hardened surface of your hopeless email conglomerate, delete what can be, notice that you never answered to twelve important messages, organize your mails in subfolders and tell yourself that starting tomorrow you will read, reply to and archive every mail properly.
Unfortunately, with your naming convention for subfolders changing every month and your daily rhythm, this never happens.

## Getting things done

*Getting things done* (GTD) is time-management system created by David Allen and described in the book of the same name.
It consists in a five-steps workflow to "do stuff":

1. **Capture** Listen to and note an idea that might become a project or something you want to do.
2. **Clarify** Express the ideas in terms of what need to be delivered. If it requires less than two minutes, do it right away. If it does not correspond to a single action, break it into multiple, smaller tasks.
3. **Organize** Plan actions by anticipating future situations.
4. **Reflect** Review your ongoing projects and planned actions on a regular basis.
5. **Engage** Proceed with an action when you are in the right place at the right time.

> Your mind is for having ideas, not holding them — David Allen

In this post we will focus on the usual tasks that we receive by email, i.e. that already went through step 1.

The rules of step 2 are simple: if the task is too small, do it right away and don't spend time on it inside your workflow; if the task is too large to be achieved in a single action, split it into multiple, smaller tasks.
For step 3, you need an agenda and "context lists" containing items to do in particular situations (e.g., at home or at the grocery store). Depending on when and where you will be able to finish the task, put it in your agenda or the correct context list.
Step 4 requires you to maintain these lists and to be able to review them anytime.
Step 5 corresponds to picking the list corresponding to your current context (time or place) and start working on its items.

## Gmail to the rescue

Gmail can help you go through these steps and offers as core features ways to handle all of the aforementioned type of emails in a fast, intuitive and reliable way.

In Gmail, the steady state should be the empty mailbox. 
It means that you are done with your emails for now and can get back to work.

For instance, this is my professional inbox as I'm writing this:
![An empty inbox]({{ site.baseurl }}/public/resources/getting-things-done-gmail/clean-inbox.png)

Messages in your inbox should only be those that were received since you last checked your emails, which implies that you should never leave your inbox screen with emails in it. 
To do so, we will apply simple Gmail actions on each email: *Read*, *Archive* and *Snooze*.

![Available actions]({{ site.baseurl }}/public/resources/getting-things-done-gmail/email-actions.png)

### Archive

The *Archive* (first button in the screenshot above) removes the email from the inbox. 
Use it when there is nothing left to do with the information it contains.
The email is not deleted, it will still be visible in "All emails" and in search results.

Notice that there is no notion of explicit, manual classification in this action: all archived emails are put in the same "All emails" folder. 
Classification is slow, imperfect and cumbersome. You will probably give up in a few days.
With the power of Gmail [search functions](https://support.google.com/mail/answer/7190), you don't need it!

![Search features]({{ site.baseurl }}/public/resources/getting-things-done-gmail/search.png)

There is also a *Delete* (second button in the actions screenshot above) version of this action, which deletes the item instead of moving it to the "All emails" folder. 
Given the size of free storage nowadays, I don't see a real use case for it and I never assume that I will never ever need an email in the future.


### Snooze

The *Snooze* (fourth button in the actions screenshot above) action temporarily moves the email from inbox to the "Snoozed" folder. 
Snoozed emails reappear in your inbox (as if they were new emails) at a specified time.
For instance, a message can be snoozed to `At 10:00 am on Monday, January 30, 2017`. 
Gmail also offers useful shortcuts for time-based conditions (e.g., `Later today`, `Tomorrow`, `Next week` or `This weekend`).

![Snooze options]({{ site.baseurl }}/public/resources/getting-things-done-gmail/snooze.png)


### Reminders

Of course, Gmail packs many more features and this was just a short introduction to the core concepts.
*Archive* and *Snooze* is really all you need to get started. 

But to reach a high level of efficiency in your daily tasks, you might want to also look into Google Calendar's *Reminders*.
Reminders can be seen as calendar events containing just a sentence that you need to remind yourself later. 

![Reminders]({{ site.baseurl }}/public/resources/getting-things-done-gmail/reminder.png)

People sometimes make fun of me for it, but I use reminders for everything: recurrent tasks, administrative chores, errands...
Scheduled from a few minutes (e.g., after I finish a conversation) to a few months or years later (e.g., taxes), they allow me to never miss anything.

> Your mind is for having ideas, not holding them — David Allen

And the nice thing about reminders is that you can apply the same *Archive*/*Snooze* actions to them (using *Mark as done* and *Reschedule* actions)!


### Gmail in action

Now you can see how Gmail can provide a simple yet powerful implementation of GTD: action requiring less than two minutes should be treated right away then *Archived*. 
Actions that cannot be done right away are *Snoozed* to a future time and can be reviewed anytime in the *Snoozed* folder. 
Lastly, there is no need to even pick items from the list corresponding to your current context: Gmail will send you a notification for that.

To be more precise, consider again our list of email types and associate a Gmail action to each of them to see how it works.

- **Status reports** These should require less that 10 seconds to read. 
  If everything looks good, click on *Archive*. 
  Otherwise, depending on the priority of the issue, consider the email as an immediate or future action request (see below).
- **Notes** Skim the content and *Archive*. 
  Remember, there is no manual classification, just a search system that won't let you down.
- **FYIs** Read quickly to stay up to date and click on *Archive*.
- **Immediate action requests** Read and treat right away (by definition, this is always possible), then *Archive*.
- **Future action requests** For these you know there is no way to put them in the *Archive* folder in the near future. 
  What you need is to find out when is the very next time you will get a chance to do so, and *Snooze* the email to that date.

In some situations, you will only be able to handle part of an email and you need to recall to do the rest later. 
In these cases, do what can be done, create a reminder for what remains and archive the original email.

It will also happen that you snooze an email to a time that turns out to be wrong. 
You thought you were going to be available but you now have something more important to do. 
No problem, simply re-snooze until the next time you think you will get a chance to treat the matter.
But be careful, re-snoozing over and over again is usually an anti-pattern: you are probably snoozing to over-optimistic dates... or procrastinating ;)


### In practice

Thursday, 5pm. 
You are just out of a meeting in which everybody agreed on your team investigating an idea of yours. 
You have been in this room for one hour and you didn't get a chance to check your emails. 
You now have 16 new messages in your inbox. 
You get yourself a cup of coffee and go through them.

- 5 notifications from JIRA. You guys are doing progress and things are moving forward. These are all status reports: *Archive*, *Archive*, *Archive*, *Archive*, *Archive*.
- 2 emails from your teammates talking about their actions and discussing their findings on this topic they are working on. Everything seems to be going well. These are FYIs: *Archive*, *Archive*.
- 3 notifications from Slack. You click on the link to open the corresponding channels, figure out that two guys pinged `@channel` (even though nobody cares about their problems), and focus on this message from your teammate asking for help "I can't find the doc we wrote about configuring Tomcat locally, do you know where it is?".
  You thought these were simple status reports, but one turned out to be an immediate action request.
  You type "Yes, it is in our project's Wiki here: https://mycompany.com/wiki/tomcat-local" then close everything and press *Archive*.
- 3 emails from Air France about your flight next month. You snooze them to the day before departure, when the information they contain will become useful.
- 1 email from your colleague sharing his notes from the meeting and asking for feedback. It states that you will work on the idea as agreed. You reply "Looks good to me", *Snooze* a reminder to `Monday, 10:00 AM` to start working on it and *Archive* the original email.
- 1 email from your house insurance. You need to pay a bill and fill out a form for them. But you are at work and don't have the time nor the information to do it. This is a future action request: *Snooze* to `Later today, 7:00 PM`.
- 1 email from your boss asking for material about your idea: "Can you put up a few slides for me to include in tomorrow's presentation?".
  This is an immediate action request that you will work on next.

And that's it, in just a few minutes, you're back to a clean inbox.
1 hour later, your slides are finished and sent to your boss for approval.
In your inbox, you find another Slack notification from your teammate.
No need to open the link, the snippet is enough: "Oh cool, thanks mate!". *Archive*.
It is now time to go home!

30 minutes later, you are on your couch watching TV. Your smartphone vibrates. Oh, you almost forgot! The insurance bill!
