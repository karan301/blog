--- 
layout: post
title: "Workflow for keeping 8-Ball Scores"
date: 2017-04-25
author: Karan Varindani
categories: apple workflow gaming
excerpt: My incredibly unnecessary 'solution' to keeping track of iMessage 8-Ball scores with Workflow.
permalink: /pool-scores-old

---
I'd wager that [GamePigeon](https://itunes.apple.com/us/app/gamepigeon/id1124197642?mt=8&uo=4&at=10l6nh)'s 8-Ball is the most used iMessage app out there. It feels like everyone (myself included) plays it. But how do we go about keeping score?[^1] I used to type the score after every game but after forgetting / losing track a couple of times, I decided to build a [Workflow](https://itunes.apple.com/us/app/workflow-powerful-automation-made-simple/id915249334?mt=8&uo=4&at=10l6nh) to semi-automate the process.

## Getting started
Other than the [GamePigeon](https://itunes.apple.com/us/app/gamepigeon/id1124197642?mt=8&uo=4&at=10l6nh) and [Workflow](https://itunes.apple.com/us/app/workflow-powerful-automation-made-simple/id915249334?mt=8&uo=4&at=10l6nh) apps (both free on the App Store), you'll need both of the following workflows: [New Pool Opponent](https://workflow.is/workflows/3b7f492e4b8946e68cbf03ce827818dc) and [Pool Scores](https://workflow.is/workflows/6733c1a06cae429683fd94d9aedd38e8).

The first thing you do is run `New Pool Opponent`, which is a Normal workflow (it only shows up in the Workflow app, not the widget). It'll ask for your name the first time you run it. The workflow will then ask for the name of your opponent and ask you if you want to initialize scores. If you say yes, you can set the score to what your current scores are. If not, it'll set the score to 0-0. Repeat this for each of your opponents. It'll save your scores as `'Pool Scores'` in Workflow's iCloud Drive directory.

Next, run `Pool Scores` and enter your name **exactly as you did above**. It'll grab the `'Pool Scores'` file and ask you to select a name from your list of opponents, and then ask if you won, lost, or just want to get the score. If you choose won or lost, it'll update the score accordingly and then get the score (it both displays the score and copies it to clipboard in case you want to send it to your opponent). 

The `Pool Scores` workflow is a Today Widget (and Apple Watch) workflow so after every game, you can just swipe down to bring up Notification Center and update scores from there.  

Read on below for a more detailed documentation of how the workflows work.

---- 

## How does it work? (Documentation)
Each workflow has one Import Question. Enter your name here. Expected input is text `myname` with no white-space (e.g. Karan), but anything works (so long as you enter it exactly the same in both workflows).

Both workflows rely on a dictionary file in Workflow's iCloud Drive directory. By default, this is read and written as `Pool Scores.json`. You can change it by manually changing the name in both workflows (it is referenced twice in `New Pool Opponent` and five times in `Pool Scores`).

### New Pool Opponent
This is a fairly simple workflow. It either adds or updates key-value pairs in an existing dictionary, or it creates a new dictionary with the given key-value pair. It works as follows.

1. The workflow asks for your opponent's name. Expected input is text `opponentname` with no white-space (e.g. James), but anything works.
2. The workflow asks if you want to initialize scores. If yes, expected input is number. Enter your score and your opponent's score where prompted. If no, scores are assigned as 0-0. 
3. The workflow gets the `Pool Scores` dictionary file from Workflow's iCloud Drive directory, and gets the dictionary from the file. If the file doesn't exist (for example, if you're running the workflow for the first time) it'll be created later. 
4. The workflow sets the dictionary value for you and for your opponent. It does this with keys `mynameopponentname` and `opponentname`  with values `myscore` and `opponentscore` respectively (e.g. {"KaranJames":100, "James":0}). If these names already exist in the dictionary, they will be overwritten.
5. The workflow saves the dictionary to the `Pool Scores` file in Workflow's iCloud Drive directory. If the file exists (for example, if you're adding a new opponent or re-initializing scores) it'll overwrite the file.

## Pool Scores
This is a slightly more complicated workflow. It grabs an existing dictionary (make sure you run the `New Pool Opponent` workflow at least once first to create one), asks you to choose from a list of your opponents, updates the score (if necessary), and copies/displays it. This workflow is best used from the Today Widget (after first run). It works as follows.

1. The workflow gets the `Pool Scores` dictionary file from Workflow's iCloud Drive directory. **This file must already exist**, execution will end here otherwise. 
2. The workflow gets all keys from the dictionary file. Some of these will be in the form `opponentname` while others will be in the form `mynameopponentname`. We need to get rid of the latter to present a list of opponent names. 
3. The workflow splits these keys into new lines. It then finds all the keys that begin with `myname` and replaces them with a blank. It then goes through and deletes all blank lines. 
4. The workflow presents this updated list of keys (opponent names) and asks you to pick one. 
5. The workflow fetches the dictionary file again, getting just the key-value pairs for you and the opponent you selected this time, and stores the values in `My Score` and `Opponent Score` variables as appropriate (with the keys again being of the form `mynameopponentname` and `opponentname`).
6. The workflow asks you to choose from a menu: "I won", "I lost", or "Get score". "I won" and "I lost" simply increment either the `My Score` or `Opponent Score` variable as appropriate, and then jumps to "Get score".
7. The workflow fetches the dictionary file again. It sets (updates) the dictionary value for keys `mynameopponentname` and `opponentname` with the updated score variables. 
8. The workflow saves the updated dictionary file, overwriting `Pool Scores.json`.
9. The workflow copies `My Score - Opponent Score` to clipboard, so you can share it with your opponent if you want.
10. The workflow displays the text `MyName MyScore-OpponentScore OpponentName`.

----

[^1]:	The app keeps track of how many games you've won, but it doesn't sync across devices (so iPhone and iPad keep different scores) and it doesn't let you know your opponent's score. I've also found the number to be inconsistent.