--- 
layout: post
title: "Tesla Shortcuts"
date: 2019-09-26
author: Karan Varindani
categories: tesla shortcuts
excerpt: A suite of shortcuts that can be used to control a Tesla vehicle through Siri (and more).
permalink: /tesla-shortcuts
---

	

A few months ago I wanted to see if it would be possible to control my Model 3 with Siri. After a lot of research/procrastination[^1], I've finally come up with a set of shortcuts[^2] (based on the [unofficial Tesla API](https://tesla-api.timdorr.com/)) that I'm happy enough with to share. You can download all the shortcuts [here](/tesla-shortcuts-tldr), but pease read on for a guide on how they work and why you might be interested in using them.

![](/assets/tesla-spoiler.jpeg)

## About the shortcuts
I've put together twelve shortcuts in total: three administrative, and nine actions. Let me briefly explain what each of them does.

The first shortcut that you have to run is `Setup Tesla`. As the name implies, this is the setup script. It'll ask you for your Tesla email address, grab your Tesla password from your Clipboard, then ask what your preferred climate temperature is (in °F). It'll  
then save your login and temperature preference in iCloud Drive (in a file named ‘Tesla Preferences’ in your Shortcuts folder). Your password will be encrypted before it's saved. Don't worry if you forget to copy it before running the shortcut - you’ll be reminded to go do that and return to the Shortcuts app.[^3]

Once your preferences are set up, you probably won't need this shortcut anymore -  but there are a few reasons to keep it around. Running it again will let you:
1. Update your preferred climate temperature.
2. Update your login information.
3. Delete your preferences file from iCloud Drive.
4. Check out the documentation (this website).

The second administrative shortcut is `Authenticate Tesla`. This is the backbone of all nine action shortcuts, but **you never need to run it individually**. It grabs your email and encrypted password from your preferences file and uses those to connect with your car. Once authenticated, it'll wake up the car (and make sure that it's awake) then pass on an access token that the action shortcuts need to work.

The last administrative piece is `Control Tesla`. This is the launchpad for all the shortcuts that I’m sharing here. All it does (for the most part)[^4] is present a menu that lets you pick between the supported actions and runs one of them. It works conversationally in Siri, through the Shortcuts widget, or, of course, through the Shortcuts app.

As a result of it simply being a controller for the other action shortcuts, `Control Tesla` is pretty much useless on its own. This was intentional; I could have combined all the shortcuts into a Frankenstein master shortcut, but this approach allows the other shortcuts to function on their own (e.g. You can say “Hey Siri, Turn Climate Off” without having to go through the `Control Tesla` menu first).

Now let's talk about those action shortcuts.

* `Set Climate` sets your car’s climate to the preferred climate from your preferences file.
* `Set New Climate` lets you enter a temperature[^5] and sets your car’s climate to it.
* `Turn Climate Off` turns off your car’s climate.
* `Get Climate` tells you what your car’s current climate status is and lets you toggle it (e.g. it'll let you know that your climate is on and set to 72, and ask if you would like to turn it off). 
* `Get Car Battery` checks your car’s current battery percentage and available range. If your car is currently charging, it'll also let you know how long is left until it's fully charged.
* `Enable Sentry Mode` turns on sentry mode.
* `Disable Sentry Mode` turns off sentry mode.
* `Check Sentry Mode` lets you know whether sentry mode is on or off, and lets you toggle its current state.
* `Open The Frunk` actuates the front trunk.

You can say any of those commands to Siri to perform the action on its own, or say “Control Tesla” if you don't remember the exact name. Only `Control Tesla	` will show up in the Shortcuts widget by default but you can easily put any or all of these actions there too.[^6]

---- 

## Why should I use this?
If you happen to open and read through the shortcuts, I hope you'll see just how much work went into making them. I took a lot of care in making sure that each shortcut is laid out logically, well-documented, checks for potential errors wherever appropriate, provides lots of options, and more. Yet you might be wondering why I bothered to do so. After all, [Tesla](https://apps.apple.com/us/app/tesla/id582007913) has a pretty good app that accomplishes everything that my shortcuts do.

The first reason that I built these is speed. Let's use the example of turning on your car’s climate: As it stands you need to launch the Tesla app, wait for it to connect to your car, go to the climate section, then tap the arrows until you get to the temperature you want. Instead, I just tap on the  `Control Tesla`  widget, tap ‘Control Climate,’ and then I can either tap on my preferred temperature or enter a custom temperature value. I can then put my phone away or wait for the confirmation message.

The biggest distinction is in where the wait occurs. Using the Tesla app, I have to launch the app then wait for it to load _before_ I can do what I want. Using my shortcut, I can do what I want then just wait to confirm that it went well. Let’s use Mail as an analogy. Using the Tesla app feels like I’m launching Mail, waiting a few seconds for it to load, then typing my email and tapping send. Using these shortcuts feel like I’m launching mail and instantly typing my email, then tapping send and just waiting for it to confirm that the email was sent. 

It’s been a much more pleasant experience in practice. I can confidently say that, since I’ve gotten these shortcuts to work reliably, **I haven’t been using the Tesla app _at all_**. It isn’t just [dogfooding](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) — I simply haven’t been in a situation where it would be quicker to use the app instead. 

Speaking of reliability, one of the reasons it took me so long to release these shortcuts is because, historically, they had worked super inconsistently. I used to have lots of failures where my car simply refused to wake up in the morning. However, this time around I was able to include a check to make sure that the car did actually wake up. If not, this shortcut will run itself again several times until the car eventually wakes up and connects. With that in place, all these actions have been _substantially_ more reliable for me. I’ve also added additional checks for better error handling in unusual situations (such as when your login information has changed or when your car is being serviced).

Speed aside, I’ve really been enjoying the added flexibility that these shortcuts have brought. This manifests itself in several important ways.

**I can control my car from my iPad.** There’s no Tesla app for iPad, and I work on an iPad Pro for most of the day. I now no longer have to stop what I’m doing and pick up my phone to check its battery level or turn on the climate. It now just takes a couple of taps and I can get back to what I was doing.[^7]

**I can control my car using Siri.** With iOS 13.1, Siri can have a conversation with you when running a shortcut. This means that you can go through the menus of `Control Tesla` (saying things like “Check battery level” or even “the second one”) to control your car hands-free. This has been particularly useful for me while getting ready for work in the morning.

**I can control my car using Shortcuts Automation**. This is another _really_ cool feature of Shortcuts in iOS 13.1. You can have certain shortcuts run automatically using certain triggers. A few examples:
* I keep a spare [key card](https://shop.tesla.com/product/model-3-key-card) for my Model 3 on my desk at work. Now, whenever I tap my iPhone on this key card, Shortcuts connects to my car and sets the climate to my preferred temperature _without me doing anything_.
* Siri can predict when I’m going to leave work to go home and, 15 minutes before, can send me a notification asking if I want to run the shortcut to cool my car down.
* I’m currently looking into putting a clear NFC tag on my hood so that when I tap my iPhone against it, Shortcuts can immediately actuate the front trunk. 
Those are just three examples but there are lots of triggers that you can use for [Shortcuts Automation in iOS 13.1](https://support.apple.com/guide/shortcuts/intro-to-personal-automation-apd690170742/ios). This is the best example of the flexibility that these shortcuts have afforded me.

----

## Additional information
* These shortcuts require iOS 13 to work. Furthermore, iOS 13.1 Is required for Conversational Siri and Shortcuts Automation to work. They will not work at all on iOS 12, even if the user downloads the Shortcuts app from the App Store, because the underlying file system behind shortcuts has been changed between those two versions.
* To add these shortcuts to your library, you have to [enable running untrusted shortcuts](https://support.apple.com/guide/shortcuts/enable-shared-shortcuts-apdfeb05586f/ios). This is a security feature new to iOS 13; you won't be allowed to run (or add) shortcuts shared outside the Shortcuts Gallery until you’ve enabled certain security settings.[^8]
* Continuing with the theme of security, the first time you run each shortcut, it will require authentication for some actions (the ‘Get Contents of URL’ actions that are used for the Tesla API calls will be most noticeably). These are all required to run the shortcut correctly.
* Although your password is base64 encoded and is never stored in plaintext, it is still stored in a file accessible in iCloud Drive. If an attacker knew what they were doing (and what they were looking at), it would not take much to decrypt the key and get the original password.
* Lastly, at this time, these shortcuts only support accounts that have one Tesla vehicle. Get in touch if this is a hard limitation for you and I’ll see what I can do.

----

[^1]:	If I'm being honest, it was two days of intense research with many months of complete procrastination between them.

[^2]:	Check out [Apple’s Shortcuts User Guide](https://support.apple.com/guide/shortcuts/welcome/ios) if you’re not sure what those are.

[^3]:	You can also enter your password in yourself but you’ll rightfully be shamed for doing so. You should really be using a password manager, like iCloud Keychain.

[^4]:	There literally one added nicety: It looks up your preferred temperature from your preferences file, so that the menu is dynamic.

[^5]:	You can enter this number directly from the Shortcuts widget which is pretty neat.

[^6]:	From the Shortcuts app, tap the (...) button beside the shortcut name to bring it up in the editor. From the editor, tap the (...) on top and enable “Show in Widget”.

[^7]:	Pro-tip: If you’re working on an iPad with a keyboard attached, you can use Cmd+Space to enter Spotlight search, type in the name of one of the shortcuts (e.g. ‘Set Climate’), and then tap on it to run it.

[^8]:	You have to have run a shortcut (either user-created or from the Gallery) once before, and then head to `Settings > Shortcuts` to enable an “Allow Untrusted Shortcuts” toggle. Remember: These security measures exist for a reason. Please inspect and carefully vet all shortcuts downloaded outside the Gallery before running them on devices with personal information.