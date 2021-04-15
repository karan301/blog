--- 
layout: post
title: "Tesla Shortcuts Downloads"
date: 2021-04-15
author: Karan Varindani
categories: tesla shortcuts
excerpt: Links to get started with the shortcuts.
permalink: /tesla-shortcuts-tldr

---

_To get started, make sure that you’re running on an iPhone, iPad, or iPod touch running iOS 14 or newer, and that you have ‘Allow Untrusted Shortcuts’ enabled in Settings. Read [the main post](/tesla-shortcuts) for more information._

### Step 1: Authenticate
Download the [Auth app for Tesla](https://apps.apple.com/us/app/auth-app-for-tesla/id1552058613) from the App Store. Sign in with your Tesla credentials and make sure that you have a valid access token.

### Step 2: Setup
Download the [Tesla Setup](https://www.icloud.com/shortcuts/1212abd071fb405b9844009885d6cd9d) shortcut and add it to Shortcuts. Run it once, enter a preferred temperature, and make sure you receive a confirmation message.

### Steps 3: Actions
Download and add the following shortcuts to Shortcuts.
* [Tesla Auth](https://www.icloud.com/shortcuts/6320086e9adb4901beb532ca3006a598)
* [Get Battery Level](https://www.icloud.com/shortcuts/0396e3fb28f14da8a964af4e609b212b)
* [Control Sentry Mode](https://www.icloud.com/shortcuts/1d9dc451f39d47ba8adca76f0c3a2d1b)
* [Control Climate](https://www.icloud.com/shortcuts/c8551fdc9c5b41dfa2803eac36623432)
* [Open Frunk](https://www.icloud.com/shortcuts/d099fef59a1943d1bc02322cb77c9dcd)
* [Tesla Control](https://www.icloud.com/shortcuts/105cc176b1d54b5e9cc686d6b34194aa)

### Step 4: Control
I've found that the best way to use these shortcuts is to add the `Tesla Control` shortcut to a widget, to your Home Screen, and/or to run it from Siri. It links to all the other shortcuts so you can consider it a launcher of sorts. It's cleaner than trying to find the specific shortcut you want to run (you can think of these as modules).

----

## An aside on token management:
During _Step 2_, `Tesla Setup` talks to the Auth app for Tesla to get an access token. It then stores this token in a Tesla Preferences file in iCloud Drive. This allows you to only need the Auth app on one device for initial setup - it's not accessed by any of the shortcuts. (Your iPad or other iOS devices will reference the token from the file in iCloud Drive, which should sync over pretty quickly, so they don't need the Auth app installed.) This shortcut assumes that you keep the Auth app installed on your iPhone, and only your iPhone. As long as you run any of the shortcuts once within 7 days of the token expiring from your iPhone, you'll never need to manually refresh your access token. However, if it expires, you can re-run `Tesla Setup` from any device that has the Auth app installed to manually refresh the token.

## Other notes:
* Please don't rename any of the shortcuts. They all have dependencies with each other, and this will cause issues.
* If you find any issues, please [let me know](mailto:karan301@me.com) and I'll be sure to correct them for other people as well.
* These shortcuts have only been validated as of iOS 14.5. There may or may not be issues with older versions of iOS (I have not tested iOS 13 at all).
* After running `Tesla Setup` the first time, it creates a preferences file in iCloud Drive. Subsequent runs of this shortcut will check for the presence of the preferences file. If it exists, you'll be presented with a few other options (e.g. update your preferred temperature, refresh your token).