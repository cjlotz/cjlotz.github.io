---
layout: post
title: Messaging Plugin for Xamarin 4 released
comments: true
tags: [xamarin, mobile]
image: https://raw.githubusercontent.com/cjlotz/Xamarin.Plugins/master/Messaging/Plugin.Messaging.png
---

I've just published the final build of the next major version of the [Messaging Plugin](https://github.com/cjlotz/Xamarin.Plugins) to [NuGet](https://www.nuget.org/packages/Xam.Plugins.Messaging).  I've added no new features since the last beta build.  

Here is the [complete list of changes](https://github.com/cjlotz/Xamarin.Plugins/blob/master/Messaging/ChangeLog.md) for v4 of the plugin:
- Add support for sending SMS to multiple recipients
- Android+UWP: Add support for sending SMS without user interaction via the `ISmsTask.CanSendSmsInBackground` and `ISmsTask.SendSmsInBackground`
- Android: Add support for using `FileProvider` to add attachments using content Uri's on Android Nougat and later
- Android: Add new `Settings` class to configure Android specific behavior when sending emails/making phone calls (see next bullets).  Access from **Android project only** using `CrossMessaging.Current.Settings()` extension method.
  - Add new `EmailSettings.UseStrictMode` flag (default value `false`) to filter list of apps to only email apps and not other text messaging or social apps. **Unfortunately adding attachments when using StrictMode does not seem to work, and is therefore currently not supported.**
  - Add new `PhoneSettings.AutoDial` flag (default value `false`) to automatically phone the number instead of only showing the phone dialer with the number populated. **Please note using this settings requires the `android.permission.CALL_PHONE` added to the manifest file.**
- **Breaking Change**: Remove iOS Classic support
- **Breaking Change**: Remove Windows Phone 8.0 and 8.1 support
- **Breaking Change**: Reworked `EmailMessageBuilder.WithAttachment` platform API to provide consistent API

Full details of how to use the plugin can be found [here](https://github.com/cjlotz/Xamarin.Plugins/blob/master/Messaging/Details.md).  You can also find samples illustrating the use of the different features in the [GitHub repo](https://github.com/cjlotz/Xamarin.Plugins/tree/master/Messaging).