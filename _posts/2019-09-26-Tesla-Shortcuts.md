--- 
layout: post
title: "Tesla Shortcuts"
date: 2019-09-26
author: Karan Varindani
categories: tesla shortcuts
excerpt: A suite of shortcuts that can be used to control a Tesla vehicle through Siri (and more).
published: true
permalink: /tesla-shortcuts
---

	

A few months ago I wanted to see if it would be possible to control my Model 3 with Siri. After a lot of research/procrastination[^1], I've finally come up with a set of shortcuts[^2] (based on the [unofficial Tesla API](https://tesla-api.timdorr.com/)) that I'm happy enough with to share. You can download all the shortcuts [here](/tesla-shortcuts-tldr), or read on for a more detailed guide on how they work and why you might be interested in using them.

![](/assets/tesla-spoiler.jpeg)

## About the shortcuts
*(**Note:** The names of the shortcuts referenced below are slightly different than the names on the download page. There's a long, uninteresting reason behind this. I'll resolve this eventually and update when I do.)*

I've put together seven shortcuts in total: three administrative, and four actions. Let me briefly explain what each of them does.

To get started, you first have to download the [Auth app for Tesla](https://apps.apple.com/us/app/auth-app-for-tesla/id1552058613) from the App Store. Sign in with your Tesla credentials and make sure that you have a valid access token. I used to handle this natively in Shortcuts, but Tesla recently updated its [authentication process](https://tesla-api.timdorr.com/api-basics/authentication) for the Tesla API and it now requires a separate app for this. Fortunately, the app donates some actions to Shortcuts, and the developer is receptive to feedback. The app is [open source](https://github.com/teslabuds/authappfortesla) if you'd like to check out what it's doing.

The first shortcut that you have to run is `Setup Tesla`. As the name implies, this is the setup script. It'll ask you what your preferred climate temperature is (in °F). It'll  
then ping the Auth app and save your access token, token expiration, and temperature preference in iCloud Drive (in a file named ‘Tesla Preferences’ in your Shortcuts folder).

Once your preferences are set up, you probably won't need this shortcut anymore -  but there are a few reasons to keep it around. Running it again will let you:
1. Update your preferred climate temperature.
2. Delete your preferences file from iCloud Drive.
3. Manually refresh your access token
4. Report an issue (email me).

The second administrative shortcut is `Authenticate Tesla`. This is the backbone of all four action shortcuts, but **you never need to run it individually**. It grabs your access token from your preferences file, confirms that it's not expired, and uses it to connect with your car. Once authenticated, it'll wake up the car (and make sure that it's awake) then pass on the validated access token that the action shortcuts need to work.

The last administrative piece is `Control Tesla`. This is the launchpad for all the shortcuts that I’m sharing here. All it does (for the most part)[^3] is present a menu that lets you pick between the supported actions and runs one of them. It works conversationally in Siri, through the Shortcuts widget, or, of course, through the Shortcuts app.

As a result of it simply being a controller for the other action shortcuts, `Control Tesla` is pretty much useless on its own. This was intentional; I could have combined all the shortcuts into a Frankenstein master shortcut, but this approach allows the other shortcuts to function on their own (e.g. You can say “Hey Siri, Open the Frunk” without having to go through the `Control Tesla` menu first).

Now let's talk about those action shortcuts.

* `Get Car Battery` checks your car’s current battery percentage and available range. If your car is currently charging, it'll also let you know how long is left until it's fully charged.
* `Control Sentry Mode` lets you check if sentry mode is on or off. It also lets you turn it on or off.
* `Control Climate` lets you check what your car’s current climate status is. It also lets you turn your climate on or off.
* `Open The Frunk` actuates the front trunk.

You can say any of those commands to Siri to perform the action on its own, or say “Control Tesla” if you don't remember the exact name.

`Control Sentry Mode` and `Control Climate` are both fully interactive. If you run them on their own, they'll present a menu and let you choose how you'd like to run them. However, you can duplicate either of them, and change a text action close to the top (named 'Default State') to give it a permanent action. (For example: You can duplicate `Control Climate`, name it `Turn climate off`, and change the 'Default State' variable from "0" to "off". This will let you say "Hey Siri, turn climate off" and it will perform that action without any user interaction.) This is accomplished by using Input Variables. Based on the input provided, the shortcuts will go down a different path. For example, to control Sentry Mode, `Control Tesla` will always run the `Control Sentry Mode` action - but will either pass in "check", "on", or "off" based on what you choose in the menu. This is explained in greater detail in the comments inside the radar.

---- 

## Why should I use this?
If you happen to open and read through the shortcuts, I hope you'll see just how much work went into making them. I took a lot of care in making sure that each shortcut is laid out logically, well-documented, checks for potential errors wherever appropriate, provides lots of options, and more. Yet you might be wondering why I bothered to do so. After all, [Tesla](https://apps.apple.com/us/app/tesla/id582007913) has a pretty good app that accomplishes everything that my shortcuts do.

The first reason that I built these is speed. Let's use the example of turning on your car’s climate: As it stands you need to launch the Tesla app, wait for it to connect to your car, go to the climate section, then tap the arrows until you get to the temperature you want. Instead, I just tap on the  `Control Tesla`  widget, tap ‘Control Climate,’ and then I can either tap on my preferred temperature or enter a custom temperature value. I can then put my phone away or wait for the confirmation message.

The biggest distinction is in where the wait occurs. Using the Tesla app, I have to launch the app then wait for it to load _before_ I can do what I want. Using my shortcut, I can do what I want then just wait to confirm that it went well. Let’s use Mail as an analogy. Using the Tesla app feels like I’m launching Mail, waiting a few seconds for it to load, then typing my email and tapping send. Using these shortcuts feel like I’m launching mail and instantly typing my email, then tapping send and just waiting for it to confirm that the email was sent. 

It’s been a much more pleasant experience in practice. I can confidently say that, since I’ve gotten these shortcuts to work reliably, **I haven’t been using the Tesla app _at all_**. It isn’t just [dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) — I simply haven’t been in a situation where it would be quicker to use the app instead. 

Speaking of reliability, one of the reasons it took me so long to release these shortcuts is because, historically, they had worked super inconsistently. I used to have lots of failures where my car simply refused to wake up in the morning. However, this time around I was able to include a check to make sure that the car did actually wake up. If not, this shortcut will run itself again several times until the car eventually wakes up and connects. With that in place, all these actions have been _substantially_ more reliable for me. I’ve also added additional checks for better error handling in unusual situations (such as when your login information has changed or when your car is being serviced).

Speed aside, I’ve really been enjoying the added flexibility that these shortcuts have brought. This manifests itself in several important ways.

**I can control my car from my iPad.** There’s no Tesla app for iPad, and I work on an iPad Pro for most of the day. I now no longer have to stop what I’m doing and pick up my phone to check its battery level or turn on the climate. It now just takes a couple of taps and I can get back to what I was doing.[^4]

**I can control my car using Siri.** With iOS 13.1, Siri can have a conversation with you when running a shortcut. This means that you can go through the menus of `Control Tesla` (saying things like “Check battery level” or even “the second one”) to control your car hands-free. This has been particularly useful for me while getting ready for work in the morning.

**I can control my car using Shortcuts Automation**. This is another _really_ cool feature of Shortcuts in iOS 13.1. You can have certain shortcuts run automatically using certain triggers. A few examples:
* I keep a spare [key card](https://shop.tesla.com/product/model-3-key-card) for my Model 3 on my desk at work. Now, whenever I tap my iPhone on this key card, Shortcuts connects to my car and sets the climate to my preferred temperature _without me doing anything_.
* Siri can predict when I’m going to leave work to go home and, 15 minutes before, can send me a notification asking if I want to run the shortcut to cool my car down.
* I’m currently looking into putting a clear NFC tag on my hood so that when I tap my iPhone against it, Shortcuts can immediately actuate the front trunk. 
Those are just three examples but there are lots of triggers that you can use for [Shortcuts Automation in iOS 13.1](https://support.apple.com/guide/shortcuts/intro-to-personal-automation-apd690170742/ios). This is the best example of the flexibility that these shortcuts have afforded me.

----

## Additional information
* These shortcuts require a minimum of iOS 13 to work. Furthermore, iOS 13.1 Is required for Conversational Siri and Shortcuts Automation to work. They will not work at all on iOS 12, even if the user downloads the Shortcuts app from the App Store, because the underlying file system behind shortcuts has been changed between those two versions. Additionally, I've only really tested them on iOS 14.5 - so you may need to update if you run in to any issues.
* To add these shortcuts to your library, you have to [enable running untrusted shortcuts](https://support.apple.com/guide/shortcuts/enable-shared-shortcuts-apdfeb05586f/ios). This is a security feature new to iOS 13; you won't be allowed to run (or add) shortcuts shared outside the Shortcuts Gallery until you’ve enabled certain security settings.[^5]
* Continuing with the theme of security, the first time you run each shortcut, it will require authentication for some actions (the ‘Get Contents of URL’ actions that are used for the Tesla API calls will be most noticeably). These are all required to run the shortcut correctly.
* Lastly, at this time, these shortcuts only support accounts that have one Tesla vehicle. Get in touch if this is a hard limitation for you and I’ll see what I can do.

----

[^1]:	If I'm being honest, it was two days of intense research with many months of complete procrastination between them.

[^2]:	Check out [Apple’s Shortcuts User Guide](https://support.apple.com/guide/shortcuts/welcome/ios) if you’re not sure what those are.

[^3]:	There's literally one added nicety: It looks up your preferred temperature from your preferences file, so that the menu is dynamic.

[^4]:	Pro-tip: If you’re working on an iPad with a keyboard attached, you can use ⌘space (Cmd-space) to enter Spotlight search, type in the name of one of the shortcuts (e.g. "Get Car Battery"), and then hit return run it.

[^5]:	You have to have run a shortcut (either user-created or from the Gallery) once before, and then head to `Settings > Shortcuts` to enable an “Allow Untrusted Shortcuts” toggle. Remember: These security measures exist for a reason. Please inspect and carefully vet all shortcuts downloaded outside the Gallery before running them on devices with personal information.