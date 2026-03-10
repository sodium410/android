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
