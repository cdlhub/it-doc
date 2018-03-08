Stop application to auto-launch at startup
----------------

Use `launchctl` command. Use case with Adobe Creative Cloud from [Ask Different](https://apple.stackexchange.com/questions/138941/how-do-i-stop-the-adobe-creative-cloud-app-from-auto-launching-on-login):

```
launchctl unload -w /Library/LaunchAgents/com.adobe.AdobeCreativeCloud.plist
```

To reverse the operation, use option `load`:

```
launchctl load -w /Library/LaunchAgents/com.adobe.AdobeCreativeCloud.plist
```

[Uninstalling Applications in Mac OS X](http://guides.macrumors.com/Uninstalling_Applications_in_Mac_OS_X)
------------------

To manually remove an application and all associated files: 
 
 - Launch Activity Monitor and change "My Processes" at the top to "All Processes", then make sure the app you want to remove is not running. If it is, quit the process before proceeding. 
 -	Launch Finder and search for the app name (hopefully unique, such as Skype) 
 -	You can narrow the search to specific folders or search your whole Mac 
 -	Searching "File Name" vs "Contents" usually provides better results. 
 -	Click the + button below the search term to add criteria 
 -	Click the search criteria drop-down and select "Other...", then "System files" 
 -	Click the "don't include" and change to "include" 
 -	Sort by name, kind, date, etc. to identify components of the app, such as folders, .plist files, cache files. etc. 
 -	Delete all files and folders related to the app. 
 -	Don't empty your Trash until you've determined that everything is working OK, in case you need to restore something you deleted by accident. 
 -	A reboot might be necessary to completely remove some apps. 

## Homebrew

https://brew.sh/

Install Homebrew:

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Bash completion

```sh
brew install bash-completion
```

Add the following lines to your `.bash_profile`:

```sh
[ -f /usr/local/etc/bash_completion ] && . /usr/local/etc/bash_completion
```

Activate `root` account
------------

- From the Apple menu choose System Preferences....
- From the View menu choose Users & Groups.
- Click the lock and authenticate as an administrator account.
- Click Login Options....
- Click the "Edit..." or "Join..." button at the bottom right.
- Click the "Open Directory Utility..." button.
- Click the lock in the Directory Utility window.
- Enter an administrator account name and password, then click OK.
- Choose Enable Root User from the Edit menu.
- Enter the root password you wish to use in both the Password and Verify fields, then click OK.

Or from Terminal when logged in as an admin user

- `dsenableroot` to enable,
- `dsenableroot -d` to disable


Verify SHA-1 checksum
----------

Use the terminal:

```
openssl sha1 [full path to file]
```

Example:

```
openssl sha1 /Users/myaccount/Documents/1024SecUpd2003-03-03.dmg
```

The SHA-1 digest is displayed as: `SHA1 (full path to the file)= [checksum amount]`

``` SHA1(/Users/myaccount/Documents/1024SecUpd2003-03-03.dmg) =2eb722f340d4e57aa79bb5422b94d556888cbf38
`` 



To create a checksum file, use:

```
shasum [full path to file] > [output file path]
```

The output file is of the form:

```
[checksum file-name]
```

To make the verification when the checksum file is provided, use:


```
shasum -c [checksum file]
```

Virtual machine network configuration
--------------------------

- Different network configurations described in [Networking in VirtualBox](https://blogs.oracle.com/fatbloke/entry/networking_in_virtualbox1).

OS X Yosemite: Reinstall OS X
----------------

Use the built-in recovery disk to reinstall OS X while keeping your files and user settings intact.

Important: **You must be connected to the Internet to reinstall OS X.**

- In the menu bar, choose Apple menu > Restart.
- Once your Mac restarts (and the gray screen appears), hold down the Command (⌘) and R keys.
- Select Reinstall OS X, then click Continue.
- Follow the onscreen instructions. In the pane where you select a disk, select your current OS X disk (in most cases, it’s the only one available).

