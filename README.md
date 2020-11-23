## App Packager
#### A simple application for creating package installers from app bundles or disk images with app bundles

App Packager is a very simple application written in Platypus with shell and Applescripting.
The purpose of the app is to be able to quickly create .pkg style installers from disk images that contain a simple .app bundle, or from .app bundles that are already installed on a Mac.

### Updated version 2.0
#### New Features and changes  
1. **New window format:** App Package now uses a droplet window style, instead of the previous log window format.  
2. **Log file:** A new log file is created to track of all packages that have been created with the application. The log file is created and located in `~/Library/Logs/AppPackager.log` and contains time stamps and relevant steps such as the application name and version, 
3. **Post processing state:** The application now remains running after each use, until you quit it. This allows you to keep it open and just drag apps or disk images into its window for processing as needed. In addition, a final dialog is shown when all items are processed that details which packages were created, and if there were any errors during processing.  
4. **Multiple drag and drop:** The app can now take multiple items dropped into the window. It can be any mix of applications (.app bundles) or disk images (.dmg). The program will take each item and convert the application or the app contained within the disk image to create an installer package. You are notified of each package installer that is created as it processes them.
5. **Cache Organization string:** The organization string for the bundle identifier can now be stored in a plist and re-used each time the app creates a package. On the first run, after entering an organization string, you will be asked if you want to save the organization name for future use. Once you click Save, you won't be prompted on future runs for an organization name. If you would like to reset this to be asked for a name again, you can remove the `com.mm2270.apppackager.plist` file in your home directories Preferences folder. Or, you can change/delete the setting with a simple defaults command. #1  
6. **Bash removal:** The main script was moved to `#!/bin/sh` instead of `#!/bin/bash` due to Apple deprecating bash, and possibly removing it from a future OS release.  

#### Usage  
Drag a disk image or application (or any combination of these) onto App Packager.app. The application will launch and show a simple droplet window. The app will analyze the item dropped items, one at a time, and determine if it is a disk image (.dmg) or application bundle (.app).  
*Note: On first run, or each run, you will be prompted to enter an organization name to use with the bundle identifier. You'll have the option of saving this information to the plist so you don't get prompted on each use.*  
- If the item is a disk image, the app will mount the disk image and attempt to locate an .app bundle at the top level. 
- If the item is an app bundle, it will skip to the next phase.  
- In either case above, it will read the applications name and version, and build a new .pkg installer that installs the app into `/Applications/` using a pkgbuild command.  
- It saves the resulting package into a folder named `AppPackagerBuilds` on the current user's Desktop, or to the specified folder stored in the `packageBuildDir` string in the `com.mm2270.apppackager.plist`.  
- Alternatively, you can launch App Packager and then drag items into the droplet window to process them. The same steps above apply.  

#### Edits to the plist  
The `com.mm2270.apppackager.plist` can store a few pieces of information. The table below shows the keys and value types along with an explanation of how the key is used by the application.  

| Key Name  | Value Type  | Function  |
|-----------|-------------|-----------|
| `orgName` | `string`      |Stores the org name to be used in the bundle identifier for packages |
|`saveOrgName`|`boolean`|Stores the response to the prompt to save the org name string in the plist |
|`packageBuildDir`|`string`|Can store a specific folder path for packages to be created in. The app does not prompt for this, but it can be added to the plist manually, such as `defaults write com.mm2270.apppackager packageBuildDir "/Users/Shared/Packages"` **Note: The destination must exist and be writable**|



#### Error Checking  
The application currently does some basic error checking, including the following:
- Determines if an app is located at the top level of a mounted disk image. Alerts if no apps are found and exits if there are no other dropped items, or skips and continues to process additional dropped items.  
- Alerts if any errors were encountered when creating a package installer from the dropped item. It may include an error code, but usually dispaly error 1.   

#### Known Issues  
- App Packager currently only builds packages that are installed into the root `/Applications/` directory. If you want your application to be installed into an alternate location, App Packager cannot currently do this, so you will need to build your package using a different method.  
- App Packager cannot build a package installer for macOS Big Sur, due to known file size limitations. If you need to create a .pkg installer for a macOS Big Sur upgrade workflow, you may want to look into William Smith's (AKA Talking Moose's) excellent MegaPKGr.zsh script to help with building a package to deploy a macOS Big Sur installer. [https://gist.github.com/talkingmoose/e9ed319226c6da30dd633725e48a97b0](https://gist.github.com/talkingmoose/e9ed319226c6da30dd633725e48a97b0)

#### Download
Download App Packager.app 2.0 from [this link](https://github.com/mm2270/App-Packager/releases) on the Releases tab. 

#### Credits
Credit for the tip on building installer packages with pkgbuild goes to [neilatosi](https://www.jamf.com/jamf-nation/users/4817/neilatosi) on JAMFNation on [this thread](https://www.jamf.com/jamf-nation/feature-requests/3719/drag-and-drop-dmgs-and-composer).  
I simply created a more automated process for doing these steps in App Packager.app.
