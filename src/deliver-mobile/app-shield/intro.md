---
summary: Protect your apps against tampering. OutSystems AppShield works by repackaging the build into a more secure version of it that can detect attempts of modification. 
tags: support-application_development; runtime-mobile;
---

# OutSystems AppShield for native mobile apps

OutSystems AppShield is a feature that lets you harden the protection of your Android and iOS apps. OutSystems AppShield works by repackaging the builds from Mobile Apps Build Service (MABS) and adding features against tampering.

<div class="info" markdown="1">

To use OutSystems AppShield, you need to have a license. If you haven't got it already, contact the sales team.

</div>

## Prerequisites

To protect your apps with OutSystems AppShield, you need to meet the following requirements.

* You installed the OutSystems AppShield plugin in your environment.
* You have a license for OutSystems AppShield.
* You're using MABS 6.1 and later.

Additionally:

* You need to rebuild and redistribute the mobile apps that you want to protect.
* Be aware that the app file size increases after hardening the security. 

## Supported features

These are the features you can use with the current release of the AppShield plugin.

### Android

Protection available for the Android builds.

* Root detection
* Repackaging detection
* Code obfuscation
* Code injection protection
* Debugger protection
* Emulator detection
* Keylogger protection
* Screenshot protection
* Task hijacking protection

### iOS

Protection available for the iOS builds.

* Jailbreak detection
* Repackaging detection
* Code injection protection
* Debugger protection
* Screenshot protection

## How to use OutSystems AppShield

To create a mobile app build with OutSystems AppShield to hardened security, do the following:

1. Install the OutSystems AppShield component.
2. Add OutSystems AppShield dependencies to your app. Press **Ctrl+Q** to open the **Manage Dependencies** window. Enter `OutSystemsAppShieldPlugin` in the producer search field and then select all the elements in the right pane. Click **Apply** to add the references to your app and close the window.

    ![Manage dependencies](images/reference-appshield-ss.png?width=600)

3. Optionally, configure OutSystemsApp Shield by editing Extensibility Settings in the module properties.
4. Publish the app.
5. Create native mobile builds of the app.

### Configuration

Here is an example of the JSON for Extensibility Configurations. You can use different sets of settings for iOS and Android.

```
{
    "preferences": {
        "global": [
            {
                "name": "DisableAppShielding",
                "value": "false"
            },
            {
                "name": "AllowJailbrokenRootedDevices",
                "value": "false"
            }
        ],
        "android": [
            {
                "name": "AllowJailbrokenRootedDevices",
                "value": "true"
            },
            {
                "name": "AllowScreenshot",
                "value": "false"
            }
        ],
        "ios": [
            {
                "name": "AllowJailbrokenRootedDevices",
                "value": "true"
            },
            {
                "name": "AllowScreenshot",
                "value": "false"
            }
        ]
    }
}
```

## Obfuscation

In the current version, only the code of the core native shell and supported plugins are obfuscated. A crash from the core OutSystems components generates an obfuscated stack trace.


## Reference

These are the values on the OutSystems AppShield configuration JSON.

| Value                        | Type       | OS           | Description                                                            |
| ---------------------------- | ---------- | ------------ | ---------------------------------------------------------------------- |
| android                      | JSON value | Android      | The key denoting values that apply to the Android devices.             |
| AllowScreenshot              | boolean    | iOS, Android | If set to true, allows users to take screenshots of the app.           |
| AllowJailbrokenRootedDevices | boolean    | iOS          | If set to true, allows users to run the app on the jailbroken devices. |
| DisableAppShielding          | boolean    | iOS, Android | Activates or deactivates OutSystems App Shield.                                |
| global                       | JSON value | iOS, Android | Settings in this section apply to both Android and iOS builds.         |
| ios                          | JSON value | iOS          | The key denoting values that apply to the iOS devices.                 |

## Limitations

OutSystems AppShield has the following limitations. 

* On iOS, the plugin doesn't block user-initiated screenshots, but only to notify the app that a screenshot was taken. OutSystems currently doesn't support this event. However, AppShield blocks taking screenshots of iOS App Switcher.
* Only the supported OutSystems mobile plugins are obfuscated.
* Native Android Service Center logs are obfuscated. You need to use an external tool to deobfuscate the logs.
* JavaScript files obfuscation isn't supported. OutSystems provides guidance to obfuscate JavaScript.
* Native iOS bitcode obfuscation isn't supported.
* You need to contact Support to get the mapping files.
* After MABS creates a build, with the AppShield plugin active, and signs the build, you can't sign that build again manually because the app would recognize that as signs of tampering.  

## How to retrace Android obfuscated logs

The purpose of the obfuscation is to make the app harder to reverse-engineer. During the obfuscation process, the platform creates a mapping with the old and new names of methods and classes.

In an obfuscated app, an error stack trace might look like this:

![Obfuscation example](images/obfuscated.png?width=600)

**Prerequisites**

These are the prerequisites to deobfuscate the logs.

* Android Studio on Mac, Linux, or Windows.
* The mapping.txt file from the build. Please reach Support and request the mapping file. 
* A stack trace to deobfuscate and retrace.

**Steps**

1. Have the mapping file ready.
1. Locate the proguard tools, located under your Android SDK folder, usually in `$ANDROID_HOME/tools/proguard/bin`.
1. Inside, you should have the retrace cli, or the proguardgui.

    With **retrace cli**. Use the retrace command, followed by the path to the mapping file and the file with the stack trace. Or, run the command without the stack trace file and then paste the log content. 

    With **proguardgui**. Click the **ReTrace** button in the left pane and locate your mapping file. Paste the obfuscated stack trace and click **ReTrace!**.

    ![Proguard UI](images/proguard-log.png?width=600) 

### Known limitations

The lines that for parsing can't have the timestamp, which is what logcat tools usually produce. Instead, the lines must start with the **at** keyword.

### More information

For more information see:

* [Write and View Logs with Logcat](https://developer.android.com/studio/debug/am-logcat#format)
* [ReTrace](https://www.guardsquare.com/en/products/proguard/manual/retrace)
* [ProGuard](https://www.guardsquare.com/en/products/proguard)

## Troubleshooting

Here are fixes you can try if you notice issues when using OutSystems AppShield.  

### The app build fails

Make sure you have a license for OutSystems AppShield. Without the license, the MABS rejects the build.