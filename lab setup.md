## Windows and Kali  
Install JADX-GUI.  
Install adb  
Install apktool.  
Install Android studio.  

## MAC  
Install brew package manager - an apt install for mac  
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"  

**Install JADX** for apk reverse engineering  
brew install jadx  
jadx-gui  - launch jadx  

**Install apktool** -- for signing and verifying signature  
brew install apktool  

**Install Android studio and ADB**  emulator for apk install  
from self service -- installs and adds to app list.  
https://developer.android.com/studio  
drag the app into app folder/downloads to install  

brew install android-platform-tools //installs adb, fastboot and other tools.  
adb //verify its installed.  

**Emulator setup and optoins**   
View-->Tools window -->File explorer, terminal, Logcat.  
Install a virtual device.  
use the latest api version and phone.  
what is this api version. 
In an APK file, the minSdk and targetSdk values are stored inside the compiled AndroidManifest.xml.  
min version -- is the lowest supported.  
target version is the tested  
API level- Determines which security rules a newer phone enforces on that app using built-in if/else backward-compatibility switches.  
Latest is 37 that is android os 15, and doesnt allow http, older levels ssl pinning bypass was easier now on latest 37 very diff due to client hello encryption 

AVD with play store not root by default hence.  
use the playstore avd to install the apk if not avaialble on apkmirror.  
then stop the playstore avd instance and use the non playstore instance for root access.  
or use script to root the same avd using magisc - this gets flagged by defender  
if apk shared not on store yet - then allow install from storage to install.  

adb shell  
adb root  
adb whoami  //root.  

**Accessing ADB shell from a VM or remote network device.**  -- is also possible - google when necessary.  

**Other emulator options:** Genymotion - both cloud and desktop versions., Xamarin and Visual studio emulator for android  

**Physical Device Setup**.  
no android studio required if physical device used - rooted  
Connect with usb/type c port  
Settings->system/advanced-->enable developer mode on the device  and enable usb debugging.  
adb shell will now get into the physical device.  
can use stay awake feature and not turn off is helpful.  

**Common Emulator issues**  
some AVD's latest dont have proxy setting in the extended settings  
File--Settings--Emulator--Launch in tool window uncheck - allows extended settings enabled.  














