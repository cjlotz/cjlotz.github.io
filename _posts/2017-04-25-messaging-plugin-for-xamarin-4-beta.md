---
layout: post
title: Messaging Plugin for Xamarin 4 Beta
comments: true
tags: [xamarin, mobile]
image: https://raw.githubusercontent.com/cjlotz/Xamarin.Plugins/master/Messaging/Plugin.Messaging.png
---

I've just published another beta build of the next major version of the [Messaging Plugin](https://github.com/cjlotz/Xamarin.Plugins) to [NuGet](https://www.nuget.org/packages/Xam.Plugins.Messaging/4.0.0-alpha2).  

New feature added in this build:
- Android+UWP: Add support for sending SMS without user interaction via the `ISmsTask.CanSendSmsInBackground` and `ISmsTask.SendSmsInBackground`.  Thanks to [soroshsabz](https://github.com/soroshsabz) for the initial contribution.

I'm hoping that this is the final build before publishing v4 of the plugin.  Take it for a spin and [let me know if you run into any issues](https://github.com/cjlotz/Xamarin.Plugins/issues)