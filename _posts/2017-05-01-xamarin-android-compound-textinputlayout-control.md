---
layout: post
title: Xamarin Android Compound TextInputLayout Control
comments: true
tags: [xamarin, mobile, community, android]
---

I've been going through some Android layouts try and remove some duplication by creating re-usable [custom component controls](https://developer.android.com/guide/topics/ui/custom-components.html).  I'm pretty happy with how things are turning out and thought I'd share one or two of the controls. I will show how these controls can be used in vanilla Xamarin Android as well as in my favourite MVVM framework, [MvvmCross](https://www.mvvmcross.com/).

## Shared attributes ##

I want all my compound controls to support a common set of attributes to create a consistent look-and-feel. Every control will have a Header (i.e. description), Text (i.e. value) and Error properties.  For this I define a `IITComposite` interface.

<script src="https://gist.github.com/cjlotz/15852d8df465c91632c8eeefb9a24e44.js"></script>

All my custom compound controls will implement this interface.

## Custom TextInputLayout Control ##

The first compound control will be used to capture text input from the user.  Most of the functionality is already provided out-of-the-box via the `TextInputLayout` and `TextInputEditText` views of the `Android.Support.Design` library.  

Here's a screen shot of of these controls in action in one of my apps:  <img src="{{ site.url }}/public/images/blog/ITTextInputLayout.png" height="60%" width="60%"/>

Here's the corresponding layout for the password field:

<script src="https://gist.github.com/cjlotz/7a13d8eff732aa52fc9927f3b7dd4abd.js"></script>

People familiar with MvvmCross will understand the [binding syntax](https://www.mvvmcross.com/documentation/fundamentals/data-binding) being used by the `app:MvxLang` and `app:MvxBind` attributes.  The binding is what makes a MVVM framework so powerful and I'm not going to delve into the details here.  From the binding syntax we can see that the `Hint`, `Text` and `Error` properties of the views map nicely to the features required by my `IITComposite` interface.  It therefore makes sense for my compound view to re-use by inheriting and extending the existing `TextInputLayout` control.

### Control Layout ###

We start by defining the layout for the compound control that will be inflated in code within a `Resources\layout\control_textinput.axml` file:

<script src="https://gist.github.com/cjlotz/9d9a8692f9d78821ba9d47fb10c95cfb.js"></script>

As the compound control inherits from `TextInputLayout`, the `merge` tag is being used instead of the `android.support.design.widget.TextInputLayout` tag to prevent an extra instance of the control being inflated when my custom component control is included in a layout.

### Control Layout Attributes ###

To define the set of attributes that can be set for a control in layout, you have to create a `Resources\values\attrs.xml` file and include a definition for your control and its supported attributes within it. 

<script src="https://gist.github.com/cjlotz/23a0d47b87d6363a4f2de44e5986b4c7.js"></script>

Notice that the `ITTextInputLayout` adds support for its own  `ItcHeader` and `ItcError` attributes, but for the rest makes use of the default Android attributes already associated with TextInput controls.  

Given the above attributes definition, the layout for the same password field using the `ITTextInputLayout` control can be simplified to:

<script src="https://gist.github.com/cjlotz/9aecdd8002b429cd5aecf3851d19b76c.js"></script>

Notice that the control now supports the `IITComposite` interface through the `Header`,`Text` and `Error` properties.  Again, the code snippet illustrates these properties being set using the binding functionality of MvvmCross.  I can however also set them manually as shown by using the `app:ItcHeader` attribute to set the Header property in the following snippet:

<script src="https://gist.github.com/cjlotz/1cfa766a50448fc0d92b24708bcc8996.js"></script>

### Control Properties ###

With these attributes defined, we can now populate the properties for our control from the layout definition (lines 8-27):

<script src="https://gist.github.com/cjlotz/9a9c6668a608172a372ac892159c59a6.js"></script>

We first inflate our control and then assign the attribute values from the layout to the public properties of the control.  These properties are mapped to the internally inflated controls to drive the behavior of the compound control.

<script src="https://gist.github.com/cjlotz/53cb761b5e259bd136c1d27ca3a8f6df.js"></script>

### Control Events ###

To allow external consumers to be notified of the text being entered, we add a `TextChanged` event to the compound control.

<script src="https://gist.github.com/cjlotz/ea7b03ca03571bb665ac7cd5890187d7.js"></script>

We hook/unhook the `Control.AfterTextChanged` event via the `OnAttachedToWindow` and `OnDetachFromWindow` methods and raise our own `TextChanged` event whenever we get notified by the internal `TextEditInputView`.  

In a follow up post I will show how easy it is to create a custom MvvmCross binding for the `ITTextInputLayout.TextChanged` event to automatically bind the changes to a data context.

## Summary ##

That's about it for now.  You can find a gist for the complete code of the `ITTextInputLayout` control [here](https://gist.github.com/cjlotz/ce03548bea1ee16b6c0951a38404c414).  I also highly recommend reading through [the transcript of this talk](https://news.realm.io/news/gotocph-daniel-lew-efficient-android-layouts/) by Daniel Lew on how to create efficient Android layouts.
