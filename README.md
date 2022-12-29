# conviva-android-coresdk

## Conviva Android Core sdk can be included in two ways in any Android app projects.

* Gradle dependency
* Offline library

## Gradle dependency
    Add the following line to app's build.gradle file.
    
    implementation 'com.conviva.sdk:conviva-core-sdk:<version>'
    
## Offline library
    Place the Conviva library in app's 'lib' folder and add the following line to app's build.gradle file.
    
    implementation fileTree(dir: 'libs',include:['*.aar'])
    
## Support Android Version    

Target sdk version : Android 13 (API level 33)
Minimum sdk version : Android 4.0.4 (API level 15)
 
## Note:  

* Refer https://pulse.conviva.com/learning-center/content/main/main.htm for integration guidelines.
