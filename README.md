# Swift Storyboard-less Tutorial!
## Authored by Harrison Milbradt on February 23, 2018

Welcome to my tutorial!  Today, I'm going to show you some of the major pain points that come when learning how to build an iOS app without a storyboard.

## Why Go Storyboard-less?

The interface builder XCode provides is an amazing tool (Looking at you Android Studio 😏), which unfortunately has some serious downsides.  I'll point out two examples relevant to myself, but it won't be hard to see the advantages for yourself.

### Merge Conflicts
For those working in a team, you've probably encountered this problem before.  For those that haven't, I'll give you a quick rundown; Any changes made to the position of storyboard elements (ViewControllers) can cause merge conflicts.

There are several workarounds for this, with my favorite being a storyboard-less design.

### Code Reuse And Simplicity
Possibly the biggest benefit for solo devs is code reuse.  All that's required to copy a layout is some copypasta.  This helps keep your layouts consistent and seriously speeds up the design process.

Once you've wrapped you head around the concept of storyboard-less design, it greatly simplifies the design process.  Instead of diving through nested menus trying to find the tag of a controller or the spacing of a stack item, can you just update one line of code.

## Let's Get Started

Alright, let's figure out how to make a storyboard-less app!

### Step 1: Clone The Repo
First, make sure you've cloned this repository to an accessible directory.  Feel free to fork or star this as well!  I've saved you the work of creating two ViewControllers that we'll use later in this tutorial.

### Step 2: Open The Project
Open up the project in XCode.  In order to build the project, you must first change the projects team to your own.

To do this, click on the project in the project navigator.  Then, under signing, change the team to your own account.

![Changing your signing.](docs/readme-images/signing.png?raw=true "Changing your signing.")

### Step 3: Delete Unnecessary Files
We're going to delete all the unnecessary files included by the XCode setup wizard.

These are the files that need to be deleted:
- ViewController.swift
- Main.storyboard

Your project should now look like this in the project navigator:
