# Swift 4 Storyboard-less Tutorial!
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
We're going to delete all the unnecessary files included by the XCode setup wizard.  Make sure you select "Move to trash"."

These are the files that need to be deleted:
- ViewController.swift
- Main.storyboard

Your project should now look like this in the project navigator:

![Files after deletion.](docs/readme-images/after-delete.png?raw=true "Files after deletion.")

### Sanity Check
Go ahead and try to build the project.  You'll notice that it succeeds, but crashes when run.  Looking at the crash log, you'll see this:

![Could not find a storyboard named 'Main'.](docs/readme-images/main-not-found.png?raw=true "Could not find a storyboard named 'Main'")

This is because we deleted the Main.storyboard (Duh).  Lets fix that now.

### Step 4: Remove Main.storyboard Reference
Click on the project in the project navigator and scroll down to ""Deployment Info".

Remove the reference to Main in the "Main Interface" option by deleting "Main".

![Remove Main reference.](docs/readme-images/main-reference.png?raw=true "Remove Main reference.")
Your project should now build!  Now onto the fun part 🙂.

### Step 5: Set Initial View Controller
Now that we removed our Main.sotryboard, we need some way of telling our app which ViewController should be displayed first.

We can do this in the AppDelegate ```didFinishLaunchingWithOptions``` method, which was provided by the apple project setup wizard.

Replace the ```didFinishLaunchingWithOptions``` method with the following snippet:

```swift
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

        // Setup our window to display on
        window = UIWindow(frame: UIScreen.main.bounds)

        // Our starting ViewController
        let initialViewController = FirstViewController()

        // Set the background so we know it works
        initialViewController.view.backgroundColor = UIColor.cyan

        // Show our starting view controller
        window!.rootViewController = initialViewController
        window!.makeKeyAndVisible()

        return true
    }
```

Here, we define our window, create our initial ViewController with a funky background color, then show it on the window!  Easy!

### Sanity Check
Running the app now, you should see this:

<img src="docs/readme-images/its-alive.png?raw=true" width="40%" alt="It works!">

If you don't see this, go back and make sure you haven't missed a step!

### Step 6: Add A Button
Now let's give this app some functionality.  We're going to add a button that will let users navigate to the SecondViewController.

Inside FirstViewController.swift, add the following code to the ```viewDidLoad()``` method, underneath the call to ```super.viewDidLoad()```:

```swift
    // Created button with a color and title
    let button = UIButton()
    button.backgroundColor = .blue
    button.setTitle("Go!", for: .normal)

    // Add an on press action
    button.addTarget(self, action: #selector(buttonPressed), for: .touchUpInside)

    // Add it to our view
    self.view.addSubview(button)
```

We're also going to need a function to handle the button press.  Add the following function to your viewController:

```swift
    @objc func buttonPressed(sender: UIButton!) {
        print("You pressed me!")
    }
```

### Sanity Check
At this point, your app will build, but doesn't show the button.  Why is that?

The answer is it needs some contraints!  So let's add some.

### Step 7: Adding Constraints
Let's add some constraints to our button so we can see it.

Add the following snippet to the bottom of ```viewDidLoad()```:

```swift
    // False because we are setting them manually :)
    button.translatesAutoresizingMaskIntoConstraints = false

    // Center our button in the middle of the screen
    let centerHorizontally = NSLayoutConstraint(item: button, attribute: .centerX, relatedBy: .equal,
                                                toItem: view, attribute: .centerX,
                                                multiplier: 1.0, constant: 0)

    let centerVertically = NSLayoutConstraint(item: button, attribute: .centerY, relatedBy: .equal,
                                                toItem: view, attribute: .centerY,
                                                multiplier: 1.0, constant: 0)


    // Give some width and width to the button
    let width = NSLayoutConstraint(item: button, attribute: .width, relatedBy: .equal,
                                    toItem: nil, attribute: .notAnAttribute,
                                    multiplier: 1.0, constant: 100)
    // Give some width and height to the button
    let height = NSLayoutConstraint(item: button, attribute: .height, relatedBy: .equal,
                                    toItem: nil, attribute: .notAnAttribute,
                                    multiplier: 1.0, constant: 50)

    // Add our constraints to the view!
    view.addConstraints([centerHorizontally, centerVertically, width, height])
```

Wow, that's a lot of code.  But bear with me, it's a lot simpler than it looks.

First, we're disabling autoresizing.  Our app will crash if we don't add this line, as our constraints will conflict with it.

Then, we're adding contraints to center the button on the screen.  Understanding the arguments for NSLayoutContraint is easiest if you seperate them to different lines, based on their functions.  The first line is the object we want to apply the constraint to (Our button).  The second line is the object we're constraining it to (The view in this case).  The last line is the amount we're constraining it.

Anyways, we add some width and height to the button, then add our constraints to the view.

Try running it!  You should see our button.

![Our Button!.](docs/readme-images/button-shown.png?raw=true "Our Button!")


### Sanity Check
Clicking on the button should display "You pressed me!" in our debug area.  If not, go back and make sure you've added the function!

### Step 8: Performing a Segue
Now that we have a button with all the functionality hooked up, our final step is to display our SecondViewController.

Inside the ```buttonPressed``` funcion, add the following code:

```swift
    // Instantiate a viewcontroller and set its background color
    let secondViewController = SecondViewController()
    secondViewController.view.backgroundColor = UIColor.red

    // Present our viewcontroller to the user
    present(secondViewController, animated: true, completion: nil)
```

Here, we're instantiating a view controller, changing the background color of it to red, then presenting it to the user.

Go ahead and run the app.  When the button is pressed, you should see this:

![Finished.](docs/readme-images/second-controller.png?raw=true "Finished!")

### Challenge Time
Up for a challenge?  See if you can add a UILabel to our second viewcontroller, center it in the screen, with the text "Storyboard-less Design!"

Example:

![Completed Challenge.](docs/readme-images/challenge.png?raw=true "Completed challenge!")


Hint: Most of the code is the exact same 🙂

Don't worry if you get stuck, I'll post the code for you to look at below this.

### Challenge Answer
Here's the completed code for the challenge.  Hopefully you got it, but if not, keep practicing!

```swift
    override func viewDidLoad() {
        super.viewDidLoad()

        // Created our label
        let label = UILabel()
        label.text = "Storyboard-less Design"


        // Add it to our view
        self.view.addSubview(label)

        // False because we are setting them manually :)
        label.translatesAutoresizingMaskIntoConstraints = false

        // Center our label in the middle of the screen
        let centerHorizontally = NSLayoutConstraint(item: label, attribute: .centerX, relatedBy: .equal,
                                                    toItem: view, attribute: .centerX,
                                                    multiplier: 1.0, constant: 0)

        let centerVertically = NSLayoutConstraint(item: label, attribute: .centerY, relatedBy: .equal,
                                                  toItem: view, attribute: .centerY,
                                                  multiplier: 1.0, constant: 0)


        // Disclaimer:  This is not an ideal method for working with label text.  For simplicitys sake,
        // I have kept this almost identical to our previous button example.  However, if you're interested,
        // there are more appropriate solutions to label constraints
        let width = NSLayoutConstraint(item: label, attribute: .width, relatedBy: .equal,
                                       toItem: nil, attribute: .notAnAttribute,
                                       multiplier: 1.0, constant: 200)
        let height = NSLayoutConstraint(item: label, attribute: .height, relatedBy: .equal,
                                        toItem: nil, attribute: .notAnAttribute,
                                        multiplier: 1.0, constant: 50)

        // Add our constraints to the view!
        view.addConstraints([centerHorizontally, centerVertically, width, height])
    }
```

### Thanks For Reading!
Now you're ready to go tackle your first big project without a storyboard.  Good luck!
