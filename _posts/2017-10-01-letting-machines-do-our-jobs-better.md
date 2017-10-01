---
layout:     post
summary:    How we adopted continuous integration into our workflow.
title:      Letting Machines Do Our Jobs Better
---
![tractor](/images/tractor.jpg){: .centre-image}
*["Tractor"](https://www.flickr.com/photos/juanedc/13978948993) by Juanedc is licensed under CC BY 2.0*

## In the Beginning…

When I joined [Apptivity Lab](https://www.apptivitylab.com) just over a year ago, we were using [Beta by Crashlytics](http://try.crashlytics.com/beta/) to send out builds to our testers. This was triggered from a [fastlane](https://fastlane.tools) script that we had fashioned together. The magic script would transform our code into a downloadable .ipa/.apk file that would be emailed to testers.

This was a pretty neat solution. We were quite pleased with the entire thing.. until we started noticing some hiccups.

> Did you send out a build last week?

"Uhhh… I forgot. Sorry."

> Did you send out a build yesterday?

"Well… the script only seems to run on Joe’s computer and he wasn’t around yesterday."

> Can you send out a build now?

"Hey… what’s this weird error fastlane is giving me?"

---

As you can see, it was problematic.
1. Manual action was required — someone had to run the script. Sure it was just running a script, but why did we have to keep asking someone every time?
2. Sometimes magic fails — if you don’t even know how it works, there’s no chance you’re gonna be able to figure out what went wrong.
3. Humans make mistakes — to err is human. We forget things all the time. No one is perfect and we shouldn’t expect them to be.
4. It’s not development — these hiccups distracted us from what we should be concentrating on: building cool features and geeking out about our code.

We realised that we had to reduce the amount of human interaction required. We should be offloading all of these tasks to machines that were dedicated to doing this sort of thing; machines that would automatically do all of this for us.

What we needed was **_continuous integration_**.

## Weighing the Options

At first we considered setting up our own continuous integration machines using Jenkins and Xcode Server. But then we realised that it wouldn’t be that much different from those magic fastlane scripts we were using before. If something went wrong, someone would have to go in and fix it.

We decided that it would be best to leave it in the hands of those who specialise in CI tools. After comparing a few options on the market, we decided to try out [buddybuild](https://www.buddybuild.com) as our CI tool. They had a free plan which would allow us to better understand how CI would help us and how it would affect our work. 

We were excited, but we were cautious.

![roomba-robot](/images/roomba-robot.jpg){: .centre-image}
*["roomba robot"](https://www.flickr.com/photos/99781513@N04/20998474933) by Scott Lewis is licensed under CC BY 2.0*

## Letting the Machines Take Over
The benefits were immediate. Anytime anyone pushed in code into our repository, buddybuild would fire up and inform us on Slack whether the build succeeded or failed.

Pushing in code that doesn’t compile is a huge sin in the world of software engineering, but we’ve all done it before. With a CI tool running, we would no longer have to find out the hard way if someone had broken the build. Buddybuild would scream at us in the Slack channel if the build failed.

Buddybuild would also prepare an installable version of the build ready to be distributed to testers if we wanted to. No more asking Joe if he released a build last week; buddybuild could do it all automatically.

We started to feel the limitations of the free plan right away; build queue times were erratic. Sometimes they would be processed within 30 minutes, sometimes it would take 3 hours. Even with this, we still felt that the safety net provided by buddybuild was priceless.

It was love at first sight for us. It was time to take this relationship seriously.

## Things Started Getting Serious
Instead of crashing for free on buddybuild’s couch in the living room, we decided that it would be better if we moved in and started paying rent. In other words, we moved to a paid plan.

The biggest gain was that our builds would no longer take hours to process; instead it was immediate. This opened up a whole new door of opportunities for us to take advantage of:

- More strict unit testing could be enforced
- UI testing could be run automatically on multiple devices (screenshots accessible from the buddybuild dashboard)
- Pull requests could be introduced into the workflow
- Schedule builds to be sent automatically to our testers at the end of every week

This entire exercise basically eliminated a whole categories of issues from our workflow; all the previous issues became a thing of the past. Knowing that our CI tool would have our backs allowed us to concentrate on more pressing issues.

---

With the past behind us, we went on for a few more months before we ran into our next set of problems:

**_Deployment._**

Buddybuild had been an amazing contribution to the development workflow, but we were still lacking in the deployment department. Our process of getting apps uploaded to the App Store and Play Store was a bizarre mix of legacy fastlane scripts, manual deployment and a whole lot of “have you deployed it to the App Store & Play Store yet?”

## The Final Frontier: Continuous Deployment
We knew what we had to do. It was time to reduce the amount of human interaction required in the deployment flow. We had a CI tool that was already fully capable of becoming our continuous deployment tool; we just had to utilise it.

This was a recent exercise for us, but it was just as fulfilling as it was when we first felt continuous integration. Right now, anytime a push is made into the `master` branch, buddybuild will:

1. Build the project and automatically bump up build numbers.
2. Sign it with the relevant deployment keys/certificates.
3. Deploy it automatically to iTunes Connect and the Google Play Console.

## Where We Are Today
Looking back at how things were a year ago versus how things are now, I’m pretty confident in saying that our technical health has improved tremendously. We have saved countless hours by offloading the work of sending out builds and handling deployments to our buddies over at buddybuild.

If your organisation hasn’t adopted continuous integration or continuous deployment yet, I urge you to give it a shot. I don’t think we can ever go back to the days where we were doing it all by hand.

---

_This is a mirror of the article that I [published on Medium](https://medium.com/@adamhaafiz/letting-machines-do-our-jobs-better-e637940d5fb5)._