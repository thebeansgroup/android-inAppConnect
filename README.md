# Student Beans Connect Android SDK
Library for integrating Student Beans Connect in Android.

![Screen_01](/uploads/c4abb731afed20018a6dee6e46d5aa22/Screen_01.png)
![Screen_02](/uploads/255d59998bc80015d18892beb725865c/Screen_02.png)
![Screen_03](/uploads/c65db8df52570436e6be09bd63a4f3c0/Screen_03.png)
![Screen_04](/uploads/132beb2751db6528d8c0579844976ff6/Screen_04.png)

## Getting Started
Languages: Kotlin

Minimum Android sdk version: 21

### Gradle

We highly recommend that you use our SDK with Gradle, all you need to do is make sure you have jcenter() as one of your app's repositories. 
Note: This is still under review and subject to change.
```kotlin
repositories {
    jcenter()
    [publisher to be confirmed]
}
```
Then add the library as one of your dependencies
```kotlin
dependencies {
    implementation [to be confirmed]
}
```
## Usage

Please refer to the demo within the app folder as an example of how to integrate this library into your own application. 

```kotlin
import com.studentbeans.sbconnect.SBConnect
import com.studentbeans.sbconnect.interfaces.SBConnectInterface
```
Instantiate the class:
```kotlin
val sbConnect = SBConnect()
```
SBConnect has one method: 

```kotlin
openConnect(context, offer-slug, offer-countryCode, listener)
```

Call this method to show the user the SBConnect view which allows your student users to log in to Student Beans and collect their discount code. 

You will also need to create an instance of the `SBConnectInterface` to pass to `openConnect`. This has three override methods:

`onSuccess` - This is triggered when your user has logged in successfully to Student Beans and retrieved their discount code. It is now copied to their clipboard and passed as a String so you can choose to perform your own actions such as automatically applying the discount code to their basket.

`onError` - This is triggered when an error occurs during the lifetime of the SBConnect view. You will receive an error message as a String which will detail the error.

`onClose` - This is triggered when the user manually closes the SBConnect view.

```kotlin
 private val connectListener = object : SBConnectInterface {
        override fun onSuccess(code: String?) {
            // Do something with code
        }

        override fun onError(errorMessage: String?) {
            // Handle error 
        }

        override fun onClose() {
            // Handle close
        }
    }
```
Example:
```kotlin
sbConnect.openConnect(your-activity-context, "your-offer-slug", "your-offer-countryCode", connectListener)
```
It is important you liaise with your Student Beans account manager to confirm your slug and country code and pass these correctly, otherwise you will be returned an error.

### Errors
The type of errors you may recieve will fall under one of three categories:

`PageNotFound` - occurs when your offer cannot be found by SBConnect. This could be due to a typo in your slug or countryCode. This can also occur when your offer is not yet live, or expired. If you have checked your slug and countryCode are correct and still receive this error, please contact your Student Beans account manager.

`CodeIssuance` - occurs when we have run out of unique discount codes for your offer or your offer has expired. These are rare and will usually be quickly resolved by your account manager at Student Beans. If you experience these errors often, or for a prolonged period of time, please contact your Student Beans account manager.

`GeneralError` - is a catch-all error that can occur for a number of reasons such as network errors and server errors. As above, if you experience these errors often, or for a prolonged period of time, please contact your Student Beans account manager.
