## App Packager
####A simple application for creating package installers from app bundles or disk images with app bundles

App Packager is a very simple application written in Platypus with shell and Applescripting.
The purpose of the app is to be able to quickly create .pkg style installers from disk images that contain a simple .app bundle, or from .app bundles that are already installed on a Mac.

####Usage
**Use case 1:**
- Drag a disk image onto App Packager.app. The application will launch and show a simple log window. The app will analyze the item dropped and determine if it is a disk image (.dmg) or application bundle (.app).  
*Note: The application will only accept either an app or dmg file*
- The app will mount the disk image and attempt to locate an .app bundle at the top level.  
- Once it does it will read the applications name and version, and build a new .pkg installer that installs the app into `/Applications/` using a pkgbuild command.  
- It saves the resulting package into a folder named `AppPackagerBuilds` on the current user's Desktop.  

**Use case 2:**
- Launch App Packager.app
- A dialog prompt will appear asking for a selection.  
*Note: the application will only accept either an app or dmg file for selection*  
- The App Packager.app will read the applications name and version, and build a new .pkg installer that installs the app into `/Applications/` using a pkgbuild command.  
- It saves the resulting package into a folder named `AppPackagerBuilds` on the current user's Desktop.  

####Error Checking  
The application currently does some basic error checking, including the following:
- Determines if an app is located at the top level of a mounted disk image. Alerts if no apps are found and exits if so.  
- Determines if more than one app was found at the top level of the disk image. An Applescript dialog will appear asking for a single app selection from any valid items found in the disk image.  

####Known Issues  
App Packager.app currently only builds packages that are installed into the root `/Applications/` directory. If you want your application to be installed into an alternate location, App Packager cannot currently do this, so you will need to build your package using a different method.  

####Download
Download App Packager.app 1.0 from [this link](https://github.com/mm2270/App-Packager/releases) on the Releases tab. 

####Credits
Credit for the tip on building installer packages with pkgbuild goes to [neilatosi](https://jamfnation.jamfsoftware.com/viewProfile.html?userID=4817) on JAMFNation on [this thread](https://jamfnation.jamfsoftware.com/featureRequest.html?id=3719).  
I simply created a more automated process for doing these steps in App Packager.app.
