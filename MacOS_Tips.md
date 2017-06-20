# MacOS Tips & Tricks


### Tip 1: Change the extension of screenshot image to jpg

By default, screenshots save as .png files in MacOS. To save space, you can change the xetnsion to jpg. You can also use other formats like .tiff, .pdf, etc. Just change the last letters of the code to your preference.

```
defaults write com.apple.screencapture type jpg
```


### Tip 2: Encrypt any file on MacOS

Encryption is important when it comes to moving senstive files such as financial information, passport photos, and more. While there are standalone apps that encrypt files, OS X is capable of encrypting right from the Terminal!

Be sure to change {path-in} and {path-out} with the files you plan to encrypt/decrypt. The easiest way is to drag-and-drop the file you’d like to encrypt into the window and Terminal will write out the path automatically.

```
encrypt (change path): openssl enc -aes-256-cbc -e -in {path-in} -out {path-out}
```
Need to decrypt the file now? Easy! Just change -e to -d; or paste the command below!
```
decrypt (change path): openssl enc -aes-256-cbc -d -in {path-in} -out {path-out}
```

### Tip 3: Fetch Updates More Often

By default, Mac App Store only checks for updates once per week. To power-users, that can be annoyance; especially if we’re looking for new app and system updates with frequency. Rather than check manually, you can tell Terminal to change the interval at which your Mac checks for updates.


```
defaults write com.apple.SoftwareUpdate ScheduleFrequency -int 1
```
In our example above, we’ve set the interval to 1. Therefore, our machine is set to check for updates every day. You can change the number of days, though, we don’t recommend you go for longer than the default of a week (7).

### Tip 4: Change audio device quickly

Instead of entering _System Preferences_ and clicking the Sound panel in order to change your output/input devices (e.g. bluetooth speaker, headphones, external mic, etc.), you can simply hold down the option key and click the volume button in the menubar. A little list will pop down that will allow you to change audio devices on the fly!

### Tip 5: Prevent sleep
Sometimes, you don’t want your computer to fall asleep automatically. Perhaps you’re uploading a YouTube video, downloading Steam games, or torrenting. Rather than fiddle around with your Power Saver preferences in System Preferences, you can `caffeinate` your Mac so that it doesn’t fall asleep until you say so!

In Terminal, type:
```
caffeinate
```
You can give your Mac a break once it is done by hitting Control+C in the Terminal or quitting Terminal altogether. Yes, this command only continues to work as long as Terminal stays open.

Perhaps you need your computer to stay awake for a few hours but want it to go to sleep after a set time when the task it needs to finish completes. It’s easy. All you have to do is enter the following command:

```
caffeinate -i -t 3600

```

The number at the end represents the number of seconds. So, 3600 = 1 hour. If you need it to stay awake 5 hours, 3,600*5=18,000.

Source: (http://snazzylabs.com/tutorial/five-advanced-tricks-for-mac-users/)

### Tip 6: Adjust volume in smaller increments

By default, when you try to increase or decrease the volume using the volume keys, the incrementation is not so accurate. To adjust in smaller increments, use `Shift+Option (Vol-/Vol+)`

### Tip 7: Move Files instead of copying them

On MacOS, we're not allowed to use Cmd+X/Cmd-P (like Ctrl+X - Ctrl-P on Windows), and we're only allowed to do Command+C/Command+V and it creates a duplicate file on the disk. To avoid this and to be able to move a file instead of creating a copy of it, we can use `Command+C` to copy and then `Option+Command+V` to move the file to a new location.

### Tip 8: Use 'Picture in Picture' mode for Youtube videos

If you want to continue working on other documents while following a Youtube video, you can do it by `double right click` on the Youtube video and selecting `Enter picture in picture` from the popup menu. By default, the video will try to snap to one of the four corners on desktop. But if you prefer to move the picture to wherever you want, press `Command` key and then move the picture frame. You can also resize the frame by dragging from and of the corners.

### Tip 9: Change ComputerName/HostName/LocalHostName
In `Terminal`
```
sudo scutil —set ComputerName
sudo scutil —set HostName
sudo scutil —set LocalHostName
hostname -f
```

### Tip 10: Show hidden files/folders in MacOS
In `Terminal`

```
defaults write com.apple.finder AppleShowAllFiles YES
killall Finder
```

First line enables showing hidden files/folders and second in restarts Finder

### Tip 11: Unhide/Hide the Library folder
In `Terminal`
```
chflags nohidden ~/Library
chflags hidden ~/Library
```

### Tip 12: Show my IP
In `Terminal`
```
ip=$(ifconfig en0 | grep "inet " | awk '$1=="inet" {print $2}') && echo "My IP is: $ip"
```

To add the ip to host

```
xhost + ${ip}
```


-- END --
