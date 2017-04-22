---
layout: post
title: Messaging Plugin for Xamarin 4 Alpha2
comments: true
tags: [xamarin, mobile, plugins]
image: https://raw.githubusercontent.com/cjlotz/Xamarin.Plugins/master/Messaging/Plugin.Messaging.png
---

I've just published another alpha build of the next major version of the [Messaging Plugin](https://github.com/cjlotz/Xamarin.Plugins) to [NuGet](https://www.nuget.org/packages/Xam.Plugins.Messaging/4.0.0-alpha2).  

New features/changes in this build are Android related and include:
- Android: Add new `CrossMessaging.Current.Settings().Email.UseStrictMode` flag (default value `false`) to filter list of apps to only email apps and not other text messaging or social apps. **This replaces the `EmailMessageBuilder.UseStrictMode` added in the first alpha.**
- Android: Add new `CrossMessaging.Current.Settings().Phone.AutoDial` flag (default value `false`) to automatically phone the number instead of only showing the phone dialer with the number populated. **Please note using this settings requires the `android.permission.CALL_PHONE` added to the manifest file.**
