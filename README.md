## Core kit setup

Please read core kit SDK setup part first. [ZTCoreKit](https://github.com/zhortech/ztcorekit-android-sdk/blob/main/README.md)

### Gradle

If you use Gradle to build your project — as a Gradle project implementation dependency:
```groovy
implementation "fr.zhortech.android:ztsportkit:$zhortechSdkVersion-prod"
```

### Product initialization
For product initialization provide desired algorithm type `ZTAlgorithmType`
```kotlin
val algorithmType = ZTAlgorithmType.RUNNING // or ZTAlgorithmType.WALKING
val product = ZTSport(algorithmType)
```
## Activity

### Start

To start activity first set user parameters:
```kotlin
val userParameters = ZTUserDataParameters()
userParameters.bodyWeight = ...
userParameters.bodyHeight = ...
userParameters.shoeSize = ...
 
ZTSport.setUserParameters(userParameters)
```
Next, call start activity with chosen goal
```kotlin
ZTSport.startActivity(ZTSportActivityAttributes(goal, goalValue))
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(
        {
            //activity started
        },
        {
            //error
        }
    )
```
### Activity data
You can provide data of current activity by:
```kotlin
ZTSport.addActivityData(timeStamp, activityData.toList())
```
### Realtime data
To request activity data in real time use:
```kotlin
val fields = list of desired fields
ZTSport.getActivityRealtime(fields)
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(...)
```

### Stop
Activity can be stopped explicitly:
```kotlin
ZTSport.stopActivity()
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(...)
```
Or it could happen if modules go to sleep if there was no user activity. You should listen for this kind of stop:
```kotlin
ZTSport.observeRestoredActivity()
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(...)
```
### Results

#### Summary
To request activity summary after stop call:

```kotlin
ZTSport.getActivitySummary(activityId, fields, include)
            .observeOn(AndroidSchedulers.mainThread())
            .subscribe(...)
```
Where fields is array of of type `ZTSportActivitySummary.Fields`, and include array of additional attributes that listed in zcloud docs
