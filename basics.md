Android -- is a mobile operating system created for touchscreen devices like phones now for TVs.  
Based on a modified version of the Linux Kernel.  
Pmary hardware architecure used in majority android devices is ARM - Advanced RISC(reduced instruction set computing) machine  
ARM is a type of chip architecture that focuses on being physically small and using the least amount of electricity as possible to do the work it is instructed.  

**Android Plaform stack** consists of 6 components  
Linux kernel -- wifi, display, audio, keypad, bluetooth, USB, camera  
Hardware Abstraction layer(HAL) -- software layer for interacting with the hardware components  
Android Runtime -- 
is the managed runtime environment used by the Android operating system to execute applications.  
can execute apps packaged in .dex dalvik executable format - maintains backward compatibility with applications originally built for Dalvik  
Dalvik was the default runtime environment in Android versions prior. It was eventually replaced by the Android Runtime (ART)  
Native C/C++ libraries -- Android components like ART and HAL are created using native code, and in order for these components to work,  
access to native libraries is needed. Applications can access these libraries through the Java Native Interface (JNI)  
java APi framework -- This component provides software tools and interfaces for building Android applications example: activity, resource,locataion manager, content providers  
System apps -- preinstalled apps - phone, messages, play store, calendar etc  

**Rooting**  
Android flash storage has 2 portions -- /system/ OS parition, and /data/ -- used for user data and application installs  
root -- full access enhanced capabilities - also more susceptable to malware since rooting disables some built-in security  

**Important Directories**  --these now vary from device to device  
/system/app  -- contains the pre-installed apps  
/data/app -- apks installed by user  
/data/data -- contains all apps installed by the user  
/system/bin -- contains binary files  
/data/local/temp -- world writable directory  

Kotlin and java are the 2 primary languages used to develop Android apps  
Kotlin is design to interopereate fully with java runs on JVM  
Google use Kotlin as the default programming language for app development  

**Android Security Features**  
Each Android application runs within its own isolated security sandbox, enforced by the underlying Linux-based architecture.  
Android is a multi-user Linux system where each application is treated as a separate user.  
By default, the system assigns each app a unique Linux user ID (UID). This UID is used by the system for access control, but is not exposed to the app itself.  
File system permissions ensure that only the app assigned a particular UID can access its own files.  
Android enforces the principle of least privilege, meaning apps only receive the permissions necessary to perform their core functionality.  
Additional privileges must be explicitly declared in the app's manifest and approved by the user.  

UID examples -- ls -al /data/data/  ---- com.android.chrome, com.android.shell  
app1 with Process 1466 can only access /data/data/com.android.app1 directory not that of other  

**Application signing**  
To install an application on a device or upload it to the Play Store, the APK file must be signed.  
Signing the APK is crucial for security, as it protects the package from malicious modifications.  

Insecure signing --- Jar signing v1 - some parts not covered by this signature  
Secure -- V2 and V3  
How to sign -- jarsigner/apksigner, play app signing, android studio -  google for procedure  

**APK Structure**  -- unzip and check apk file contents  
APK - android package - archive that contains all the components needed for app to run. 
when an android app is compiled the java/kotlin source code is first converted into java bytcode which is then transformed into a dex file.  
these dex files are executable an dcan be interpreted by Dalvik virtual machine or Android runtime ARTs  
In addition to compiled code, APK files include resources such as assets, images, UI layouts, and the AndroidManifest.xml file  

**Unzipped APK contains following**  
META-INF -- certificate/signatures - used in code signing  
assets -- application assets  -- images,videos,docs,dvs and other raw files  
lib -- native code  -- written in c/C++ -- Native code is typically included in the application as shared libraries (.so files).  
These libraries can then be invoked from Java or Kotlin using the Java Native Interface (JNI).
res -- application resources  -- unlike assets cannot be modified by user at runtime. UI layouts, fonts, screen orientations and more  
AndroidManifest.xml  -- Metadata about the app - overall template of what app does  
classes.dex - compiled java code  - Large applications may include multiple DEX files  
resources.arsc - resource information  -- maps resource identifiers in the code to their actual values -- such as strings, colors, layouts and styles  
kotlin -- for apps written in kotlin and contains kotlin specific metadata  

**Android App and Development**  
Android Studio -- Official IDE for android application development  
google how to install on kali/windows  

**Types of Applications**  
Native apps -- specifically built for particular OS  
Web apps -- build using HTML, CSS and JS similar to native apps uses mobile browser to disaply and interact with app web pages  
Hybrid -- both combined  

WebView are components that allow developers to emded and display web content directly within app.  
webview.loadUrl("file:///android_asset/html/index.html")  
setJavaScriptEnabled(true)  - means that the WebView is allowed to execute Javascript code -- XSS  

**Application Frameworks**  
An application framework is a set of libraries that provides developers with a structured way to build applications using pre-built components and tools. 
These often include UI elements, security and authentication mechanisms, error handling, logging systems, and more.  

Flutter -- use IDE like Android studio  
Xamarin -- cross platform supports Android IOS and desktop apps  
React Native, Apache Cordova and others  

## Androind Application Components and Interprocess communication  
These components are declared in the AndroidManifest.xml and can be used individually or in tandem with one another.  
Interprocess Communication (IPC) is a mechanism that allows for communication between applications or different processes within the same application.  
Each application runs in its own process, and thus, IPC has to make sure that applications have a way to communicate with each other when necessary.  

**Activities**  
main component that represents a single screen with a user interface responsible for managing user interaction with the application, and can be started by other Activities, apps, or system events.  
Activities are also responsible for managing the app's lifecycle.  
To start an Activity programmatically, we first create an Intent object. Intents are messaging objects used to request an action. startActivity(intent);  
to use an Activity properly, you must declare it in your app's **manifest file**.  
example: login screen  

**Services**  
component that performs long-running operations in the background without providing a user interface.  
example -- downloading files, playing music even after user left the app  
startService() method  
Bound Service --  other application components to bind to them by calling the **bindService()** method.  

**Broadcast Receivers**  
Broadcast Receivers are designed to respond to system-wide or custom events broadcasted by other applications.  
For example, the system broadcasts an event when the device starts charging.  
let other apps know file downloaded etc  
Broadcast Receivers also need to be declared in the **AndroidManifest.xml** file.  

**Content Providers**  
they act as the intermediate between the app and its underlying data storage.  
Create, Read, Update, Delete operations on data stores such as  
local SQLite databases, the device's internal or external storage, or even on a remote server.  
Activity-->CursorLoader-->ContentResolver-->ContentProvider-->Data Storage  
Content Providers, along with the permissions required to access the provider's data, must be declared in the **AndroidManifest.xml** file.  

**Intents**  
Intents are messaging objects used by applications or the Android system to request actions from other components such as Activities, Services, and Broadcast receivers.  
starting an activity -- navigating from list of contacts to specific selected contact  
starting a service --Downloading a file in the background  
Delivering a Broadcast --  Informing other components that the battery is low  
Explicit Intents -- Explicit Intents are commonly used for navigating between activities within the same app or starting services.  
Implicit Intents -- Implicit Intents are used when we don't know the exact target component  
In addition, Intents can also carry data between components in the form of key-value pairs called Extras - putExtra().  

Understanding and analyzing Intents while assessing an application is crucial—not only for  
gaining insight into the app’s flow but also for identifying potential security bypasses

**Binders**  
The Binder is Android's core Interprocess Communication (IPC) mechanism, enabling efficient and secure communication between different processes.  
remote service can refers to a service running within the same application but in a different process  
Binders are not declared in the manifest file directly, as they are a part of the Service implementation.  
However, if the Service runs in a different process, the attribute android:process should be specified in the AndroidManifest.xml file as remote process.  

**DeepLinks**  
A Deep Link is an Interprocess Communication (IPC) mechanism that allows users to navigate directly to specific content within an app  
by tapping a URL found on a website, email, or SMS  
example -- amazon sale email clicking link takes me to produc on amazon app, if not installed redirected to app store to install.  
Two types -- Standard deep link and Android App link  
To enhance security when using Deep Links, it is suggested to use Android App Links (which ensure links are verified and securely handled) instead of generic Deep Links.  
we must set up an intent filter in the Androidmanifest.xml file for the corresponding activity  

**Android Emulators**  
programs allowing users to run Android applications and simulate the Android operating system on devices like personal computers.  
Android Virtual Device (AVD) -- Android Studio's AVD is a fast and feature-rich emulator  
In a new Android project, navigate to Tools -> Device Manager --> Create device  
system images that only include Google APIs will allow elevated privileges (root) by using the ADB tool through the terminal  

Other Emulators -- Corellium - both Android and iOS with web based interface, Genymotion, Bluestacks, NoxPlayer, Memu PLay, LDplayer  

**Android Debug Bridge**  
ADB is a versatile command-line tool that enables communication between a computer and a device.  
shell to the device emulated  
allows developers to perform tasks like installing and debugging applications, transferring files between the host computer and the device  
apt-get install adb -- can also be installed on windows and ios  
adb start-server   /facilitates ineraction between adb client shell and adb daemon on the emulated device  
adb devices -- list devices  
adb push ./test.apk /data   -- copy file to device  
adb shell whoami  
adb shell  //shell to the device  then ls -l /sdcard/  
adb root  
The adb root command restarts the ADB daemon (adbd) on the Android emulator with root privileges. gives root perms on device  
as mentioned earlier in the Android Emulators section, only system images labeled with Google APIs will provide this feature.  
System images labeled with Google Play are signed with a release key, and elevated privileges (root) are not supported.  

**Application Pentesting Methodology and Tools**  















