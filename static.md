Static analysis is a technique used to evaluate the security of an application by examining its source code without executing it.  
Benefits -- early detection, complete coverage - app code not just the features  

## Types of Vulnerabilities that Static Analysis can detect  
Insecure storage - data stored in locations other apps or users can access  
Hard-coded sensitive information -- such as passwords api keys  
Insecure communication -- use of HTTP  
Insufficient cryptography -- Use of tls1.0 weak cyphers  
Insecure permissions -- requesting more perms than necessary giving app access to sensitive data  

**APK Sources**  
Google play store, Manufacturers play stores -- samsung galaxy store, amazon appstore, vivo app store etc  

## APK extraction  
To install from sd card or unknown source -- enable  Settings -> Security -> Unknown sources  
APK download from Third party app stores  -- https://apkcombo.com  
Extraction using third-party tools --  APK Export  - install this to export any app apk file  
Extracting The APK From The Device -- /data/app/com.example.myapp-1/base.apk  
Access to the /data/app/ directory is restricted for non-rooted users, which makes it difficult to explore or guess full package paths.  
If package name is known get the path --  adb shell pm path com.example.myapp  
if package name is not known try adb shell pm list packages | grep myapp  // to list package name first  
Once path confirmed download the apk -- adb pull /data/app/com.example.myapp-1/base.apk .  

## Disassembling the APK   
analyze the content of an APK file by using tools to decompile the app's source code to a more human-readable language,  
then decode the assets and resources to read any configuration files.  
APKTool allows us to decode the APK's resources, also supports rebuilding the APK after changes have been made.  
apktool d myapp.apk  //disassemble the apk with dir of same name in current dir with decooded files  
why apk why not open with 7zip ? because example - manifest file is encoded in APKs and cannot be read by simply unzipping the APK  

## Understanding the Manifest  
mousepad myapp/AndroidManifest.xml  
**Package Name** -- package="com.example.myapp" -- uniquely identifies the application  
also reveals target SDK version, different Android SDK versions may present different security implications.  

**Permissions** -- app request users to allow specif perms those declared here, poor security hygiene may request more permissions than necessary  
<uses-permission android:name="android.permission.INTERNET"/>  -- many others related to camera, storage, location, sms, contacts etc google  

**Application Class**  -- has many declarations one of the important ones are  
android:name="com.example.myapp.Init" means the Init class will run immediately when the app starts and before the main activity or user interacts.  
Developers usually use this for initializations but if vulnerable 3rd party is loaded 1st then a finding.  
Network Security config - is another one -- android:networkSecurityConfig="@xml/network_security_config  
specify custom trusted Certificate Authorities, permit or deny cleartext (HTTP) traffic, and other network-related settings  
vim myapp/res/xml/network_security_config.xml  -- shows the custom cert in use  
if cert pinning in place defined in this file as -- <pin digest="SHA-256">7HIpactkIAq2Y49orFOOQKurWxmmSFZhBCoQYcRhJ3Y=</pin>  
cert pinning is to ensure app talks to legit app server, ensures integrity of comms. need to modify this to use burp for interception add its cert  
ls -l myapp/res/raw  //network config file points to where cert is located  

**Components**  -- shows the activities of the application.  
<activity android:exported="true" android:name="com.example.myapp.MainActivity">  -- name of activity  
exported=true  activity is accessible from outside the app - can be launched from adb  
<action android:name="android.intent.action.MAIN"/> -- indicates main entry point of the application, the first screen launched when the user taps the app icon  

Followed by content providers and a service and use of Deep links(which are declared as activity with intent filters)  

**Application resources**  
The directory res contains resources like the app's layout, UI strings, images, and other static files - network setting  
notice the directory values within the res directory. This directory contains XML files with hardcoded strings and integers,  
such as Strings.xml, which stores UI strings as key-value pairs -- check for any sensitive creds, keys etc  
Also examine under res layout, drawabale, xml sub directories  

## Understanding Smali  
When an Android application is compiled, the Java compiler (javac) compiles .java files into Java bytecode (.class files).  
Likewise, Kotlin source files are compiled into .class files by the Kotlin compiler (kotlinc).  
These are then translated into Dalvik bytecode, and the output is bundled into .dex (Dalvik Executable) files.  
then packaged into apk with compiled code, resources, and manifest  
in APKtool baksmali is used to disassemble the Dalvik bytecode into a human-readable format known as Smali.  
Smali is a low-level, assembly-like language used by the Dalvik Virtual Machine and Android Runtime  

To be continued switching to Dynamic analysis  


## TCM Security  
Injured android - not there anymore on playstore  
install your app to test from playstore.  
pm list packages | grep injured  //find the apk path of the installed app / pm package manager.  
adb pull /datapp/~~/path/base.apk Injuredapppulled.apk   //pull an apk from installed, use this apk to run on rooted non playstore machine.  

Insecure apps for testing from owasp - https://mas.owasp.org/MASTG/apps/android/MASTG-APP-0001/  

**Android Manifest.xml**  
Is where the basics of the applications are defined such as minSDKversion, Permissions, activities, content providers.  
Permissions - what the app needs access to -- contacts, camera, internet, bluetooth etc.  
list of all permissions is listed in the developer.android.com. with descriptions  
Activities - basically UI elements/different screens in the app - example accounts, profile page etc.  
All activities must be defined in manifest along with whether exported=true or false to allow other apps to launch/not.  
Intent filter -- 2 types implicit and explicit intents.  
Explicit intents - Inside activities, to connect pages with data ex: whatsapp homepage to chat with specific user.  
implicit intents -  opening whatsapp say from chrome webpage - So whatspp should define this implicit filter in manifest. 
Only 1 explicit intent in manifest pointing to main page, rest all implicit intents.  
android:exported="true" is required on Android 12 and higher so external apps like Chrome are allowed to launch that WhatsApp Activity.  
Activities can also be exposed outside of the application -- setting exported=true in the activity. shouldn;t allow to get say profile from login page.  
You define these intents, activities, deeplinks etc in AndroidManifest.xml so the Android OS knows what your app can do before your app is ever launched or running in memory.  
Because Android manages battery and RAM strictly, **most installed apps are completely shut down in the background** Android cannot run your Kotlin/Java code to ask,  
"Hey WhatsApp, can you handle shared links?" Instead, Android reads your app's static AndroidManifest.xml file at installation time.  

Also for security and sandboxing --  
By default, one app cannot open another app's screens. Declaring an <intent-filter> (paired with android:exported="true") acts as an explicit security permission slip telling Android, "It is safe for external apps to launch this specific Activity, and only for these specific data types.  
Deep links say whatsapp and chrome - chrome defines deep links to catch all https, while whatsapp if opening from chrome is limited to  
Why link in chrome opens whatsapp when both has same deep links catchers ? exact host match has higher preference and another reason of 
deep links are part of activity, cannot be freely floating, androind should know which screen to open when link is clicked.  

Content providers - serve data from app to other, example contacts app shares contacts with whatsapp.  
if exported true can expose data to any user or app on the device.  
but exported should be true for other apps to access even with contacts permission.  
with androind.readpermission explicitly given to whatsapp to read contacts.  
on phone settings its simply allow access to contacts but in the code developer must declare read vs write.  

4 core components androidmanifest.xml. 
Activity, Service(for background service - message sync,), Broadcast receiver, content provider. 
Broadcast receiver -- whatsapp otp auto fill after google sms retriver sends broadcast. 
Service -- play a voice note in whatsapp chat and open gmai audio continues in background with a notification its running.  

Singning -- v1 is bad and has some issues - can be repacked with malicious content.  
V2 and V3 is the latest, apk signture shows google but sha2 is different for each app. 
app on google playstore are signed by google play  
developer simply logins to play console, uploads their android app bundle soda.aab signed with upload key.(signed with private and uploaded is public key).  
when user tries to install app from playstore, google play generates the apk and signs it with dedicated app signing key stored in googles hsm.  

The app is installed in /system which is not accessible from folder explorer without rooting.  
The app stores its runtime data such as images, chat backup etc on Android/data/package_name folder.  
ok whatapp stores chats in /data/data/com.whatsapp --requires root,  while local backups and media files on Android/med or Android/data - no root required.  

whatsapp does encrypt whatsapp chat backups, if someone steals it they cannot decrypt without key stored in /data/data which requires root access.  
Android/data - hides media from gallery.  
/Android/Media/com.whatsapp  -- allows gallery to index photos.  

Note: Unlike traditional desktop operating systems where the user control when a program opens and closes,  
Android completely controls the apps lifecycle - pausing, stopping or terminating processes to manage limited mobile power and memory resources.  
Desktop apps run as a continous single entry process main(), While android apps broken into independent components(activities,services) that android can destroy and recreate individually.  
Desktop apps stays in memory until closed, Android app must constantly saves its UI state because Android can kill it mid-use. 
Android kills older background processes to free memory for active apps, swiping away or leaving an app puts it in a cached background
Android acts as a central manager of the phone it needs a complete  
Because the android  acts as the central manager of phone it needs a complete blueprint of every app and AndroidManifest.xml serves as that blueprint.  
Why Manifest ? no single entry point, check what permissions app requires defined, minSDK defined.  
Right before app goes into background, app to save its instance state in the memoryRAM protected system process  
Activity record -- whenver you open a new screen activity record is created in memory holds the name of the screen, the intent that launche it and its position. 
Attached to this record is the bundle of UI data example typed text or scroll position 

• App Open: Activity (displays buttons) \(\leftrightarrow \) Service (plays music + updates notification).  
• App Closed: Activity is gone. Notification (displays mini buttons) \(\leftrightarrow \) Service (still playing music).  
So for music app services form majority, for banking app activity as everything requires user interaction.  
No, a Service does not have a user interface (UI) A Foreground Service must display a notification in the status bar (like a music player controller or a file download progress bar).  
However, the Android System UI draws that notification—the Service just sends text and icons to the system.  































