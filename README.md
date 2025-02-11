# Liveness-Only-Native-Android

## Installation

## Step 1
```sh
#Create a github.properties file in the root folder of android. Path should be like below: E.g. "myDemoApp/android/github.properties"

# Add the following in github.properties:
USERNAME_GITHUB=MyUsernameOnGithub
TOKEN_GITHUB=TokenFromGithubClassicToken
```

## Step 2: build.gradle (project)
```gradle

buildscript {
    ...
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
## Step 3: build.gradle (app)
```gradle


android{
    minSdkVersion 24
}


dependencies{
    implementation 'net.blusalt:liveness_android_data:1.3-1'
    implementation 'net.blusalt:liveness_android_core:1.3-1'
}
```


## USAGE
```sh
## To use facial comparison
  LivenessNativeManager.getInstance().startFacialComparisonSDK(this, apiKey, appName, clientId, isDev, imageByte, LivenessFacialComparisonType.MOTIONAL, new LivenessCompletedCallBack() {
            @Override
            public void onSuccess(LivenessSuccess response) {

                Log.e("Liveness comparison success", "" + response);
            }

            @Override
            public void onFailure(int code, String error) {
                Log.e("Liveness comparison failed", "" + error);
            }
        });

## To use liveness feature only without doing facial comparison
LivenessNativeManager.getInstance().startLivenessDetectionOnlySDK(this, apiKey, appName, clientId, isDev, LivenessDetectionOnlyType.MOTIONAL, new LivenessCompletedCallBack() {
            @Override
            public void onSuccess(LivenessSuccess response) {

                Log.e("Liveness detection only success", "" + response);
            }

            @Override
            public void onFailure(int code, String error) {
                Log.e("Liveness detection only failed", "" + error);
            }
        });
```
