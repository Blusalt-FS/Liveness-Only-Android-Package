# Liveness-Only-Native-Android

Facial Comparison and Face Detection SDK for Android and IOS

Get your Liveness credentials and application ID registration from [Blusalt](https://blusalt.net/)

## Features

#### Facial comparison (LivenessFacialComparisonType)
1. Dynamic Liveness Detection (Motional):
   This method verifies liveness by requiring the user to perform specific actions like opening their mouth or shaking their head.
   It's strict because attempts to bypass the action (like holding a still image) will be detected and likely terminate the process.
2. Static Liveness Detection (Still):
   This approach asks users to position their face within the camera frame and validates liveness based on facial data points.
   It's less strict than the dynamic method. If users struggle with finding the right camera position, the process might not automatically terminate, allowing them to adjust.

#### Face Detection (LivenessDetectionOnlyType)
1. Dynamic Liveness Detection (Motional)
2. Static Liveness Detection (Still)
3. Flash Liveness Detection (Flash):
   This method is only available for facial detection, not facial comparison.
   It works similarly to the static method but increases the device screen brightness to enhance facial data point detection.

   
## Installation

#### Step 1

Create a [github.properties] file in root of android folder and put below into content. E.g. "myDemoApp/android/github.properties"
Replace values with your github credentials from github and make sure to grant necessary permissions especially for github packages

```
USERNAME_GITHUB=SampleUsername
TOKEN_GITHUB=SampleClassicToken
```

#### Step 2

Add below to project level gradle file `/android/build.gradle`

```gradle
buildscript {
    ext.kotlin_version = '1.9.+'
    ...

    dependencies {
        classpath 'com.android.tools.build:gradle:7.3.+'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

allprojects {
    def githubPropertiesFile = rootProject.file("github.properties")
    def githubProperties = new Properties()
    githubProperties.load(new FileInputStream(githubPropertiesFile))


    repositories {

        maven {
            name "GitHubPackages"
            url 'https://maven.pkg.github.com/Blusalt-FS/Liveness-Only-Android-Package'

            credentials {
                username githubProperties['USERNAME_GITHUB']
                password githubProperties['TOKEN_GITHUB']
            }
        }
    }
}
```

#### Step 3

Change the minimum Android sdk version to 24 (or higher) in your `/android/app/build.gradle` file.

```

android {
    ...
    defaultConfig {
      ...
      minSdkVersion 24
    }
    ...
    
    ...
    dependencies {
      implementation 'net.blusalt:liveness_android_data:1.4-1'
      implementation 'net.blusalt:liveness_android_core:1.4-1'
    }
    ...
}
```

#### Step 4

If you're experiencing crashes or timeouts.
Add below to your progaurd.pro file if using progaurd or minify is enabled  `/android/app/proguard-rules.pro`

```proguard
-keep public class com.megvii.**{*;}
-keep class net.blusalt.liveness_native.** { *; }
```

Enable proguard in `/android/app/build.gradle` file like below.

```

android {
    
    ...
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    ...
}
```

## USAGE

```sh
## To use facial comparison
Double thresholdPercent = 91.0; (Optional, can be null)
Int timeoutDuration = 120; (Optional, can be null; default to 120 second)


        BlusaltLiveness liveness = new BlusaltLiveness.BlusaltLivenessFacialComparisonBuilder(this)
                .setClientId("")
                .setAppName("")
                .setApiKey("")
                .setIsDev(false)
                .setReference("")
                .setWebhookUrl("") // Reference is required
                .startProcessOnGettingToFirstScreen(false)
                .setImageByte(null)
                .setLivenessType(LivenessFacialComparisonType.MOTIONAL)
                .setThresholdPercent(91.5)
                .setTimeoutDuration(120)
                .setListener(new LivenessCompletedCallBack() {
                    @Override
                    public void onSuccess(LivenessSuccess response) {

                    }

                    @Override
                    public void onFailure(int statusCode, String errorObject) {

                    }
                })
                .build();

## To use liveness feature only without doing facial comparison
        BlusaltLiveness liveness = new BlusaltLiveness.BlusaltLivenessDetectionOnlyBuilder(this)
                .setClientId("")
                .setAppName("")
                .setApiKey("")
                .setIsDev(false)
                .setReference("")
                .setWebhookUrl("") // Reference is required
                .startProcessOnGettingToFirstScreen(false)
                .setLivenessType(LivenessDetectionOnlyType.MOTIONAL)
                .setTimeoutDuration(120)
                .setListener(new LivenessCompletedCallBack() {
                    @Override
                    public void onSuccess(LivenessSuccess response) {

                    }

                    @Override
                    public void onFailure(int statusCode, String errorObject) {

                    }
                })
                .build();
```

## Note:

If you are getting an error on android which says "Unauthorized" when gradle is downloading or
building, generate a new
github token that have access to clone, read and write to repo, access github packages. If you don't
know which to tick, tick all boxes. Cheers
