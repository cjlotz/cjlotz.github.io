---
layout: post
title: Building a non-trivial cross platform app using Xamarin
comments: true
tags: [xamarin, mobile]
---

Over the past year or two we've been using Xamarin to build a moderately complex mobile application. [On Key Work Manager](http://www.on-key.com/functionality/mobile-work-manager-app/) is a mobile work order management solution that enables your field service staff to manage their work assignments and forms part of the [On Key Enterprise Asset Management](http://www.on-key.com/) system.  

To illustrate why I say this is as a moderately complex app, here are some of the features:
- Location tracking
- Maps integration
- Electronic PDF form filling and generation
- Electronic signatures to support signing off job cards
- Barcode scanning of assets
- Document attachments including photos, in-app voice recordings etc.
- Local notifications
- Background services to synchronise data whilst the app is not running
- Full offline support - i.e. app functionality works in offline environments and only requires peridic connectivity to synchronise changes to server

Based on local market demands, we targeted Android first and published the [Android version](https://play.google.com/store/apps/details?id=com.pragmaproducts.workmanager) of the application mid 2016.  Last week we published the [iOS version](https://itunes.apple.com/us/app/on-key-work-manager/id1223460234) of the application.  

I've had one or two enquiries from people on what tools/libaries/frameworks we used to build the app.  This post will give a rundown of what we use.

## Frameworks ##

One of the principles we followed from the word go, was to try and maximize the re-use of shared logic across the different platforms. The team had experience using the **MVVM pattern** so we decided on using a framework that would supprt the MVVM pattern.  Xamarin Forms was still in its infancy at that stage so we decided to look for a framework that would allow us to build directly on top of the native platform thus giving us full access to the native platform performance, native iOS and Android  controls, native navigation mechanisms etc.

We ended up using a blend of the excellent [MvvmCross](https://www.mvvmcross.com/) and our own small homegrown framework that covered aspects like navigation and bootstrapping a bit differently as to how MvvmCross prescribes.  This is a **big shout out** to the community members behind MvvmCross like [Tomasz](https://twitter.com/Cheesebaron) and [Martijn00](https://twitter.com/mhvdijk). The fact that we were able to replace aspects of MvvmCross is also a compliment to the overall design of the framework.  MvvmCross v5 is shaping up to be an awesome release as well.

Using the MVVM pattern we are able to share up to 70-75% of the code base between the different platforms in a PCL library.

## Testing ##

We have an extensive set of NUnit unit tests covering the shared logic contained in our PCL library that we execute on our TeamCity CI server.  We are also in the early phases of using [Xamarin.UITest](https://www.nuget.org/packages/Xamarin.UITest/) to do automated UI testing.

## Plugins/3rd Party Components ##

We use the following plugins/components to support the app features:
- Location tracking: [Xamarin Geolocator plugin](https://www.nuget.org/packages/Xam.Plugin.Geolocator)
- Maps integration: [Xamarin External Maps plugin](https://www.nuget.org/packages/Xam.Plugin.ExternalMaps/)
- Photos: [Xamarin Media plugin](https://www.nuget.org/packages/Xam.Plugin.Media)
- App permissions: [Xamarin Permissions plugin](https://www.nuget.org/packages/Plugin.Permissions/)
- SMS, Mail, Phone: [Xamarin Messaging plugin](https://www.nuget.org/packages/Xam.Plugins.Messaging/)
- PDF form filling: Our own plugin around the [Xfinium.PDF Mobile component](https://components.xamarin.com/view/xfinium-pdf-mobile-professional-edition) ($$)
- Electronic signatures: Our own plugin around the [Signature Pad component](https://components.xamarin.com/view/signature-pad)
- Barcode scanning: Our own plugin around [ZXing.NET library](https://www.nuget.org/packages/ZXing.Net.Mobile/)
- SQLite offline database: Our own plugin around the [SQLite-net PCL library](https://www.nuget.org/packages/sqlite-net-pcl/)

## NuGet Packages/Libraries ##

Besides the frameworks/plugins/controls, we use the following NuGet packages:
- File handling: [PCLStorage](https://www.nuget.org/packages/PCLStorage/)
- Cryptography: [PCLCrypto](https://www.nuget.org/packages/PCLCrypto/)
- Dependency injection: [Autofac](https://www.nuget.org/packages/Autofac/)
- REST Api: [RestSharp.Portable](https://www.nuget.org/packages/FubarCoder.RestSharp.Portable.HttpClient/) and [Polly](https://www.nuget.org/packages/Polly/)
- iOS programmatic layouts: We use [Cirrious.FluentLayout](https://www.nuget.org/packages/Cirrious.FluentLayout/) to programmatically layout our iOS views in code using Autolayout constraints
- iOS Hamburger/Drawer menu: [SidebarNavigation](https://www.nuget.org/packages/SidebarNavigation/)

## Testing/Analytics/Crash Reporting ##

For ALPHA, BETA testing, crash reporting and application insights we use [HockeyApp](https://www.hockeyapp.net/)

I think that's about it!  Since we started using Xamarin, the whole ecosystem around Xamarin including the plugins, components have evolved quite nicely. This allows us to focus on building the features of our app instead of having to re-invent the wheel. Overall we are quite happy with how our code base has turned out and we are looking forward to building more apps using Xamarin.