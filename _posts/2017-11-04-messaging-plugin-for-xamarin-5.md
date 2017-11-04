---
layout: post
title: Messaging Plugin for Xamarin 5
comments: true
tags: [xamarin, mobile, plugins, community]
image: https://raw.githubusercontent.com/cjlotz/Xamarin.Plugins/master/Messaging/Plugin.Messaging.png
---

I've just pushed v5 of the [Messaging Plugin](https://github.com/cjlotz/Xamarin.Plugins) to NuGet. The new version of the plugin has been reworked to support .NET Standard. Besides .NET Standard support, some other minor fixes/enhancements were also made.  Here's the full [Change Log](https://github.com/cjlotz/Xamarin.Plugins/blob/master/Messaging/ChangeLog.md):
- Upgrade to .NET Standard
- Android: Update to newer Support Libraries (v25.3.1)
- Android: Fix support for formatting number using country's default convention on API 21 and later. See  `PhoneSettings.DefaultCountryIso`. 
- Android: Fix HTML formatting on Android N (API 24) and later
- iOS: Improve `IPhoneCallTask.CanMakePhoneCall` on iOS to check support of url and connection to a carrier
- iOS: Fix issue with email client not appearing
- UWP: Updated to latest UWP platform
- **Breaking Change**: Remove Windows Store support
- **Breaking Change**: Remove deprecated `MessagingPlugin` facade