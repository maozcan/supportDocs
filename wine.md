# Wine

Summary from http://www.davidbaumgold.com/tutorials/wine-mac

## What is Wine

Instead of using **BootCamp** to install Mac and Windows side by side and switch between them at startup, or using **Parallels Desktop** or **Vmware Fusion** virtualization tools, **Wine** behaves differently.

When any program runs, it requests resources like memory and disk space from the operating system. All that Wine does is make sure that those requests get answered so that the program can run correctly. It never even realizes that it's not running on Windows! It's simpler than emulating a whole new computer, so it's faster. Since it's just translating requests, you don't need a copy of the actual Windows operating system. Plus, Wine is open source, which means people are continually improving it and adding new features. And it's free!

## Install XCode
Most programs that you download for your computer have already been passed through a compiler, and are all ready for you to run; but unfortunately, you can only download the Wine source code, not the working, runnable version of the program. Because most people don't need to compile source code on their computer, Macs don't have any of the command line tools installed by default, so we need to install them before we can get anywhere.

Fortunately, it's pretty easy to do that: Apple makes a piece of software called Xcode that has everything you need to compile source code, and it's available for free on the Mac App Store. Use the Mac App Store to install Xcode, and then run it and open the Preferences. Go to the Downloads tab, and if the "Command Line Tools" are available, install them. (If not, it means that they are already installed.)

## Install Homebrew
**[Homebrew](https://brew.sh/)** is a package manager that makes installing open source programs much easier. In particular, trying to install a large program like Wine without the help of a package manager would be tremendously difficult. Fortunately, Homebrew itself is simple to install: just open up the Terminal and run this command:
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
The *Terminal* will tell you what it's about to do, and ask you if you want to proceed: press Enter to do so. The Terminal may then ask for a password: this is the password to the Admin account on your computer.

As a security measure, the Terminal does not display anything as you type, not even asterisks (\*). Type your password anyway, and press Enter. If you get some kind of error, it might be because the Admin account doesn't have a password set. Setting a password is required.

Installing *Homebrew* should only take a few seconds or minutes (depending on the speed of your internet connection). When it's done, the Terminal will say that the installation was successful, and ask you to run brew doctor. Do as it suggests:
```
brew doctor
```
This will make *Homebrew* inspect your system and make sure that everything is set up correctly. If the Terminal informs you of any issues, you'll need to fix them yourself, and then run brew doctor again to verify that you fixed them correctly. When everything is set up correctly, you'll see the message Your system is ready to brew, and you can move on to the next part of the tutorial.

>Note: If Homebrew tells you that you need to agree to the Xcode license, you can do that by running:

```
sudo xcodebuild -license
```
The Terminal window will fill up with the Xcode license: read it, type agree and hit enter to agree to the license.

## Install Homebrew Cask
Homebrew uses an extension called [Homebrew Cask](https://caskroom.io/) to install other programs. Install the Cask extension by running:
```
brew tap caskroom/cask
```

## Install Required Casks
Wine needs both [Java](https://www.java.com/) and [XQuartz](http://www.xquartz.org/) to run correctly, and the easiest way to install them is with **Homebrew Cask**. Run the following command:
```
brew cask install java xquartz
```
Homebrew Cask will automatically download and install both Java and XQuartz for you. It may take a few minutes, but it will display messages to let you know what it's doing. Homebrew Cask may also ask you for your password while doing the installation. Just as in Step 1, this is the password to the Admin account on your computer, and the Terminal does not display anything as you type, not even asterisks. (If you don't get asked for your password, that's fine too.)

When Homebrew Cask is done installing both Java and XQuartz, it will stop displaying status messages and wait for you to type in a new command. Onto the next step!

## Install Wine Using Homebrew
Now we get to actually install Wine! We'll let Homebrew do all the work, all you have to do is tell it what you want with this command:
```
brew install wine
```
Just like in the last step, Homebrew will start automatically downloading and installing software onto your computer. We've already installed a bunch of things that Wine needs, but we haven't installed everything it needs: *Homebrew can grab the rest*. When Homebrew has installed everything that Wine needs, it will automatically download and install Wine, as well.

This step of the tutorial will probably take awhile -- anywhere from 10 minutes to an hour or two. The good news is, you don't need to be actually sitting at your computer during this time. Feel free to get up and do something else while Homebrew does its thing. As long as it continues downloading and installing things for you, don't interrupt it. When Homebrew stops displaying status messages and is ready for you to type in a new command, Wine is installed and ready to go!

If you get an error message at this step that reads ```error: C compiler cannot create executables or Failed to locate 'make' in path```, it means you forgot to install Xcode, or it installed incorrectly.

If you get an error message at this step that indicates that Homebrew has accidentally downloaded a file that is empty or incorrect, you can delete Homebrew's downloaded files by running ```brew cleanup```. Then try running this step again, and Homebrew will redownload the file -- hopefully correctly!

## Install Windows Programs Using Wine
To install a Windows program, first download the installer file: it should end with .exe. Remember the location you put it, and open up the Terminal again. cd to the location, and use ls to make sure you can see the installer file. (Note: if you do not know what cd and ls are, you should learn how to use the command line before using Wine.)

Once you are in the correct directory, run the installer through Wine by running the following command in the Terminal:
```
wine $INSTALLER.exe
```
Where $INSTALLER is the name of the installer file. For example, if the installer file is named setup.exe, you would run:
```
wine setup.exe
```
XQuartz will open (if it isn't already), and soon you will see a regular graphical Windows installer. Click through it, and you're done!

## Run Windows Programs Using Wine
Open up the Terminal and run this to get to your Program Files folder:
```
cd ~/.wine/drive_c/Program\ Files/
```
Run ```ls``` to see what programs you have installed. Pick a program, and enter its directory using ```cd```. (If the folder has a space in it, you must type a \ before the space. For example, ```Program\ Files```. If you're having problems, try using tab autocomplete.) There should be a file that ends in .exe: this is the program file. Type this into Terminal:
```
wine $PROGRAM.exe
```
Where $PROGRAM is the name of the .exe file. XQuartz will open (if it isn't already), and the program will pop up, ready to use! It will probably open fullscreen: to reduce it in size, go open the Window menu from the Mac OS X menu bar, and select Zoom Window. You can then resize the program normally. Enjoy using Windows on your Mac, freely and legally!

## Making a Dock Icon
Many people want to be able to run Windows programs the same way they run other programs on the Mac: by clicking an icon in the Dock. Wine isn't specifically designed to support this, but with a little trickery, we can make it do what we want.

Note: Wine prints out error messages in the Terminal when something goes wrong. By launching Windows programs via a Dock icon, you are sidestepping the Terminal, which means that if something does go wrong and Wine has to quit, it will not be able to tell you what the problem was. The first step to solving a problem is knowing what it is, so without running Wine from the Terminal, you won't be able to fix it, and neither will anyone else. Running from the Dock is fine as long as your program seems to be working correctly, but if it crashes, the first thing you should try is running it from the Terminal instead: it won't prevent the program from crashing, but it will give you some clues on how to fix the problem.

In order to launch a Windows program via the Dock, we're going to write an [AppleScript](https://secure.wikimedia.org/wikipedia/en/wiki/AppleScript) that launches the program for us, and then put that AppleScript in the Dock. Essentially, we're writing a program ourselves! Don't worry, it's easy enough. There is a program on your computer that is designed for helping you write AppleScripts: it's called ```Script Editor```, and you can find it in the ```/Applications/Utilities``` directory of your computer, same as the Terminal itself.

Open up the Script Editor. You should see a window with a large area you can type in near the top: this is where you write your AppleScript. In that area, type the following text:
```
tell application "Terminal"
    do script "/usr/local/bin/wine ~/.wine/drive_c/Program\\ Files/$PATH_TO_PROGRAM.exe"
end tell
```
You'll need to replace ```$PATH_TO_PROGRAM``` with the path from the Program Files directory to your program executable. You can see that you're simply telling the AppleScript to run a line of code in the Terminal: the same line of code that you could run to start your Windows program.

Next, **press the Compile button** at the top of the window. The text should become colored to indicate that Script Editor understands what you wrote. You can also try pressing the Run button to run your script: it should open the Windows program successfully.

Lastly, *save your script*. You can give it whatever name you'd like, but be sure to select File Format: Application in the save options, and leave Startup Screen unchecked.

Open up the Finder, go to where you saved your script, and drag that file to your Dock. It should stay there, just like a real application -- because it is a real application! However, all it does is run that launcher command for you, so you can move the application around, rename it, or even delete it, and it won't affect the Windows program that you're running.

## Keeping Wine Up to Date
Wine is an open source program. That means that programmers around the world are continually improving it, adding new features and squashing bugs. If you don't update Wine, though, it will never get those improvements, so it's generally a good idea to check for updates every so often. We can use Homebrew to keep Wine up to date: it's easy! Just run this command:
```
brew update && brew upgrade
```
With this command, Homebrew will first update itself, if any updates are available. It will then find all the outdated software it knows about (including Wine) and upgrade them all to the latest version. Checking for updates isn't strictly necessary, as Wine runs quite well currently. However, it's a good idea to run this command every few months or so.

## Uninstalling Wine and Homebrew
If you try Wine and you don't like it, uninstalling it is easy. Just run this command:
```
brew uninstall wine
```
And Homebrew will helpfully remove Wine from your computer. However, in order to install Wine, Homebrew also had to install many other small programs that Wine relies upon to work correctly. (That's why the install process takes so long!) If you want to remove these as well, [download and run the Homebrew uninstallation script](https://gist.github.com/mxcl/1173223). That script will remove everthing that you installed in this tutorial, including Homebrew, Wine, and all the other programs Homebrew installed to get Wine to work correctly.
