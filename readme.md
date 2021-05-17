# Calculator

Project developed as an object of study of the course "React Native: Develop Native APPs for Android and iOS" using Expo.

## Clone

```bash
https://github.com/felipethamay/calculator.git
```

## Expo

```bash
https://docs.expo.io/
```

## Generate build 

### 1. Install Expo CLI

```bash
Expo CLI is the tool for developing and building Expo apps. Run npm install -g expo-cli (or yarn global add expo-cli) to get it.
If you haven't created an Expo account before, you'll be asked to create one when running the build command.
Windows users must have WSL enabled. You can follow the installation guide here. We recommend picking Ubuntu from the Windows Store. 
Be sure to launch Ubuntu at least once. 
After that, use an Admin powershell to run: Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

### 2. Configure app.json

```bash
{
   "expo": {
    "name": "Your App Name",
    "icon": "./path/to/your/app-icon.png",
    "version": "1.0.0",
    "slug": "your-app-slug",
    "ios": {
      "bundleIdentifier": "com.yourcompany.yourappname",
      "buildNumber": "1.0.0"
    },
    "android": {
      "package": "com.yourcompany.yourappname",
      "versionCode": 1
    }
   }
 }
```
- The iOS bundleIdentifier and Android package fields use reverse DNS notation, but don't have to be related to a domain. Replace "com.yourcompany.yourappname" with whatever makes sense for your app.
- You're probably not surprised that name, icon and version are required.
- slug is the url name that your app's JavaScript is published to. For example: expo.io/@community/native-component-list, where community is my username and native-component-list is the slug.
- The ios.buildNumber and android.versionCode distinguish different binaries of your app. Make sure to increment these for each build you upload to the App Store or Google Play Store.

There are other options you might want to add to app.json. We have only covered what is required. For example, some people like to configure their own build number, linking scheme, and more. We highly recommend you read through Configuration with app.json / app.config.js for the full spec. This is also your last chance to double check our recommendations for App Store metadata.


### 3. Start the build

Run expo build:android or expo build:ios. If you don't already have a packager running for this project, expo will start one for you.

Please note: When you run expo build, Expo automatically publishes your app (with expo publish). In order to avoid accidentally publishing changes to your production app, you may want to use release channels.

### If you choose to build for Android
When building for android you can choose to build APK (expo build:android -t apk) or Android App Bundle (expo build:android -t app-bundle). App bundles are recommended, but you have to make sure the Google Play App Signing is enabled for your project, you can read more about it here.

The first time you build the project you will be asked whether you'd like to upload a keystore or have us handle it for you. If you don't know what a keystore is, you can have us generate one for you. Otherwise, feel free to upload your own.

If you choose to let Expo generate a keystore for you, we strongly recommend that you later run expo fetch:android:keystore and backup your keystore to a safe location. Once you submit an app to the Google Play Store, all future updates to that app must be signed with the same keystore to be accepted by Google. If, for any reason, you delete your project or clear your credentials in the future, you will not be able to submit any updates to your app if you have not backed up your keystore.

```bash
[exp] No currently active or previous builds for this project.

Would you like to upload a keystore or have us generate one for you?
If you don't know what this means, let us handle it! :)

  1) Let Expo handle the process!
  2) I want to upload my own keystore!
```

### If you choose to build for iOS
You can build standalone apps for iOS with two different types, an archive (expo build:ios -t archive) or simulator (expo build:ios -t simulator) build. With the simulator build, you can test your standalone app on a simulator. If you want to publish your app to the store or distribute it with tools like TestFlight, you have to use the archive.
When building for iOS, you are given a choice of letting Expo create the necessary credentials for you, while still having a chance to provide your own overrides. Your Apple ID and password are used locally and are never saved on Expo's servers.

```bash
$ expo build:ios
[16:44:37] Checking if current build exists...

[16:44:37] No currently active or previous builds for this project.
[16:44:37]
We need your Apple ID/password to manage certificates, keys
and provisioning profiles from your Apple Developer account.

Note: Expo does not keep your Apple ID or your Apple ID password.

? What\'s your Apple ID? xxx@yyy.zzz
? Password? [hidden]
✔ Authenticated with Apple Developer Portal successfully!
[16:44:46] You have 4 teams associated with your account
? Which team would you like to use? 3) ABCDEFGHIJ "John Turtle" (Individual)
✔ Ensured App ID exists on Apple Developer Portal!
[16:44:59] We do not have some credentials for you: Apple Distribution Certificate, Apple Push Notifications service key, Apple Provisioning Profile
? How would you like to upload your credentials? (Use arrow keys)
❯ Expo handles all credentials, you can still provide overrides
  I will provide all the credentials and files needed, Expo does limited validation
```


Unless you're very familiar with iOS credentials already, it's best to let Expo handle the creation & management of all your credentials for you. 
If you'd like to know more about iOS credentials, we've written a guide with everything you need to know here.

If you plan on providing your own certificates, we recommend creating them in the Apple Developer Portal.

### Enterprise distribution
The Expo build service supports both normal App Store distribution as well as enterprise distribution. To use the latter, you must be a member of the "Apple Developer Enterprise Program". Only normal Apple developer accounts can build apps that can be submitted to the Apple App Store, and only enterprise developer accounts can build apps that can be distributed using enterprise distribution methods. When you call expo build:ios, you just need to choose the correct team, it will be labeled (In-House).


It's usually easiest to test your standalone app on a simulator with the expo build:ios -t simulator command, and then use TestFlight for your physical device testing. But if you'd like an IPA that you can install directly onto your physical device through Xcode, you can generate one with Expo CLI:

- Run expo build:ios once to generate some preliminary certificates, namely your app identifier, and distribution certificate.
- ow, create a new provisioning profile here, and under Distribution, select Adhoc. Then provide your app identifier and the distribution certificate you created in the last step. If you don't provide those exact certificates, your build will fail.
- Download the provisioning profile, and then pass it to the build command like this: expo build:ios --provisioning-profile-path ~/Path/To/provisioningProfileYouCreated.mobileprovision, and make sure the rest of the certificates you use match those you selected when creating your adhoc provisioning profile in the previous step.

Once Expo finishes your build, you can install it onto your device via Xcode by opening the Devices & Simulators window, selecting your connected device, and under Installed Apps, click the + and then select the IPA Expo generated for you.

#### Switch to Push Notification Key on iOS
If you are using Push Notifications Certificate and want to switch to a Push Notifications Key you need to start the build with --clear-push-cert. We will remove the legacy certificate from our servers and generate a Push Notifications Key for you.


### 4. Wait for it to finish building

When one of our building machines is free, it'll start building your app. You can check how long you'll wait on the Turtle status site. We'll print a url you can visit (such as expo.io/builds/some-unique-id) to watch your build progress and access the build logs. Alternatively, you can check up on it by running expo build:status. When it's done, you'll see the url to your app file - an .apk, .aab (both Android), or .ipa (iOS) file. Copy and paste the link into your browser to download the file.

```bash
Expo CLI is the tool for developing and building Expo apps. Run npm install -g expo-cli (or yarn global add expo-cli) to get it.
If you haven't created an Expo account before, you'll be asked to create one when running the build command.
Windows users must have WSL enabled. You can follow the installation guide here. We recommend picking Ubuntu from the Windows Store. 
Be sure to launch Ubuntu at least once. 
After that, use an Admin powershell to run: Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

### 5. Test it on your device or simulator

#### Android
- To run it on your Android emulator, first build your project with the apk flag by running expo build:android -t apk, and you can drag and drop the .apk into the emulator.

- To run it on your Android device, make sure you have the Android platform tools installed along with adb, then just run adb install app-filename.apk with USB debugging enabled on your device and the device plugged in.

#### iOS
- To run it on your iOS simulator, first build your project with the simulator flag by running expo build:ios -t simulator, then download the artifact with the link printed when your build completes. To install the resulting tar.gz file, unzip it and drag-and-drop it into your iOS simulator. If you'd like to install it from the command line, run tar -xvzf your-app.tar.gz to unpack the file, open a simulator, then run xcrun simctl install booted <path to .app>.

- To test a device build with Apple TestFlight, download the .ipa file to your local machine. Within App Store Connect, click the plus icon and create a New App. Make sure your bundleIdentifier matches what you've placed in app.json. Now, you need to use Xcode or Transporter (previously known as Application Loader) to upload the .ipa you got from expo build:ios. Once you do that, you can check the status of your build under Activity. Processing an app can take 10-15 minutes before it shows up under available builds.

### 6. Submit it to the appropriate store
Read the guide on Uploading Apps to the Apple App Store and Google Play.

### 7. Update your app
For the most part, when you want to update your app, just Publish again from Expo CLI. Your users will download the new JS the next time they open the app. To ensure your users have a seamless experience downloading JS updates, you may want to enable background JS downloads. However, there are a couple reasons why you might want to rebuild and resubmit the native binaries:
- If you want to change native metadata like the app's name or icon
- If you upgrade to a newer SDK version of your app (which requires new native code)

To keep track of this, you'll need to update your app's versionCode and buildNumber in app.json (see here for details).

It is a good idea to glance through the app.json documentation to get an idea of all the properties you can change, e.g. the icons, deep linking url scheme, handset/tablet support, and a lot more.

If you run into problems during this process, we're more than happy to help out! Join our Forums and let us know if you have any questions.

## License
[MIT](https://choosealicense.com/licenses/mit/)