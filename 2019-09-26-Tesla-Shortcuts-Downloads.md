--- 
layout: post
title: "Tesla Shortcuts Downloads"
date: 2021-04-18
author: Karan Varindani
categories: tesla shortcuts
excerpt: Links to get started with the shortcuts.
permalink: /tesla-shortcuts-tldr

---

_To get started, make sure that you’re running on an iPhone, iPad, or iPod touch running iOS 15.4 or newer, and that you have ‘Allow Untrusted Shortcuts’ enabled in Settings. Read [the main post](/tesla-shortcuts) for more information._

### Step 1: Authenticate
Download the [Auth app for Tesla](https://apps.apple.com/us/app/auth-app-for-tesla/id1552058613) from the App Store. Sign in with your Tesla credentials and make sure that you have a valid access token.

### Step 2: Setup
Download the [Setup Tesla](https://www.icloud.com/shortcuts/86ddef437cb6409d87bbb9f56e6247a4) shortcut and add it to Shortcuts. Run it once, enter a preferred temperature, and make sure you receive a confirmation message.

### Steps 3: Actions
Download and add all of the following shortcuts to Shortcuts.
* [Authenticate Tesla](https://www.icloud.com/shortcuts/cb8944d11a4542e7b184c75b0b7e22f5)
* [Get Car Battery](https://www.icloud.com/shortcuts/c79e8389e9bb4e85be3758951793edfb)
* [Sentry Mode](https://www.icloud.com/shortcuts/308b132245b94168bf852fdf98081963)
* [Climate Control](https://www.icloud.com/shortcuts/cbd337e28229424e90bcbd7c5bf3b607)
* [Open the Trunk](https://www.icloud.com/shortcuts/e9e96fb22d01468891bbba0c1ccf82e8)
* [Control Tesla](https://www.icloud.com/shortcuts/1cbdea1e42f642efa80a6d9fb0783966)

### Step 4: Control
I've found that the best way to use these shortcuts is to add the `Tesla Control` shortcut to a widget, to your Home Screen, and/or to run it from Siri. It links to all the other shortcuts so you can consider it a launcher of sorts. It's cleaner than trying to find the specific shortcut you want to run (you can think of these as modules).

----

## Customizing the action shortcuts:
Some of the shortcuts support multiple actions and will present you with options when run.
* Control Climate supports checking current climate, setting your preferred temperature, setting a given temperature, and turning off climate control.
* Sentry Mode supports checking the status of Sentry Mode, turning Sentry Mode on, and turning Sentry Mode off.
* Open the Trunk supports actuating the front trunk or the rear trunk.
In some cases you may want the shortcut to quickly do one of the above actions. For example, you might want a shortcut that lets you set your preferred temperature or actuate the front trunk with a single tap. This is supported.
1. Duplicate one of the three shortcuts above.
2. Rename it to give it a more specific name (e.g. "Open the front trunk").
3. Edit the shortcut. Scroll down just a little to the Comment action that says "Below this is the Default State." Read the comment.
4. Change the action below from `0` to the behavior you want. For example, in your Climate Control duplicate shortcut, you would change it from `0` to `preferred` if you always want it to set your preferred temperature.

## How token management works:
During *Step 2*, Tesla Setup talks to the Auth app for Tesla to get an access token. It then stores this token in a Tesla Preferences file in iCloud Drive. This initial setup is the only time you need Auth app. It's then not accessed again by any of the shortcuts. (Your devices will reference the token from the file in iCloud Drive, which should sync over pretty quickly, so they don't need the Auth app installed.) This shortcut will take care of automatically refreshing the token once it expires.

## Other Notes:
* The first time you run any of the shortcuts, you'll be flooded with access/permission prompts. These are one-time prompts so you don't have to worry about seem them constantly. However, you have to run each shortcut at least once from the app or the widget to accept the permission prompts before you can run them using Siri.
* None of the action shortcuts will work if Authenticate Tesla is missing.
* Please don't rename any of the shortcuts. They all have dependencies with each other so this will cause issues.
* If you have any feedback or find any bugs, please let me know and I'll be sure to correct them for other people as well. You can run Setup Tesla → Report an Issue to create an email to me and automatically include debug logs.
* After running Tesla Setup the first time, it creates a preferences file in iCloud Drive. Subsequent runs of this shortcut will check for the presence of the preferences file. If it exists, you'll be presented with a few other options (e.g. reset debug logs, report an issue, check for updates).