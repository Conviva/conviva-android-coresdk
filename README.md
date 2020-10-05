# conviva-android-coresdk

## Conviva Android Core sdk can be included in two ways in any Android app projects.

* Gradle dependency
* Offline library

## Gradle dependency
    Add the following line to app's build.gradle file.
    
    implementation 'com.conviva.sdk:conviva-core-sdk:4.0.10.141'
    
## Offline library
    Place the Conviva library in app's 'lib' folder and add the following line to app's build.gradle file.
    
    implementation fileTree(dir: 'libs',include:['*.aar'])
    
    
## Note:  

* Refer https://community.conviva.com/ for integration guidelines.
