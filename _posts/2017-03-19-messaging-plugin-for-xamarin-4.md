---
layout: post
title: Messaging Pluging for Xamarin 4
categories: [xamarin, plugin]
---

I've just published a early alpha build of the next major version of the [Messaging Plugin](https://github.com/cjlotz/Xamarin.Plugins) to NuGet.  Visit the [Change Log](https://github.com/cjlotz/Xamarin.Plugins/blob/master/Messaging/ChangeLog.md) to see what's changed.  Here are some highlights:
- Add support for sending SMS to multiple recipients
- Android: Add support for using `FileProvider` to add attachments using content Uri's on Android Nougat and later
- Android: Add new `EmailMessageBuilder.UseStrictMode` to filter list of apps to only email apps and not other text messaging or social apps. (**Unfortunately adding attachments when using StrictMode does not seem to work, and is therefore currently not supported.**)
- **Breaking Change**: Remove iOS Classic support
- **Breaking Change**: Remove Windows Phone 8.0 and 8.1 support
- **Breaking Change**: Reworked `EmailMessageBuilder.WithAttachment` platform API to provide consistent API for adding attachments

Due to the breaking changes introduced, the version of the plugin has been bumped to v4.0.0.  Take it for a spin and [let me know if you run into any issues](https://github.com/cjlotz/Xamarin.Plugins/issues)