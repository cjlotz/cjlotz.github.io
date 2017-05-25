---
layout: post
title: Xamarin Android Compound Control - Styling
description: Shows how to style the custom Xamarin Android Compound TextInputLayout
comments: true
tags: [xamarin, mobile, community, android]
---

Last time around I showed how to create a Xamarin Android compound `TextInputLayout` control with support for capturing user input with an associate `Header` and `Error` labels.  This post looks at how to style the same control in Android.

## Style attributes ##

As mentioned previously, I want all my compound controls to support a common set of attributes to create a consistent look-and-feel. Every control will have a `Header` (i.e. description), `Text` (i.e. value) and `Error` properties.  To style these attributes, we create three associated attributes in our `Resources/values/attrs.xml` file to assign the styles to:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <attr name="ItcTextAppearanceLabel" format="reference"/>
    <attr name="ItcTextAppearanceText" format="reference"/>
    <attr name="ItcTextAppearanceError" format="reference"/>
```

In addition to these attributes we also associate these new style attributes with our existing `ITTextInputLayout` definition in the attrs.xml:

```xml
<declare-styleable name="ITTextInputLayout" >
    <attr name="android:inputType"/>
    <attr name="android:maxLines"/>
    <attr name="android:text"/>
    <attr name="android:imeOptions"/>
    <attr name="ItcLabel"/>
    <attr name="ItcError"/>
    <attr name="ItcTextAppearanceLabel"/>
    <attr name="ItcTextAppearanceError"/>       		
</declare-styleable>  
```
Notice that we don't include the `ItcTextAppearanceText` reference style in the definition.  The reason being that we will re-use the already defined style attribute on the `android.support.design.widget.TextInputLayout` that our `ITTextInputLayout` control inherits from. 

To define the default, global look-and-feel for all our controls, we also create a styles container in the attrs.xml:

```xml
<declare-styleable name="ITControls" >		
    <attr name="ItcTextAppearanceLabel"/>
    <attr name="ItcTextAppearanceError"/>               
    <attr name="ItcTextAppearanceText"/>
    <attr name="ItcTextInputLayoutStyle" format="reference"/>   
</declare-styleable>  
```

## Widget and TextAppearance styles ##

With the attributes defined, we can now define a style for our compound control in the `Resources/values/styles.xml` file.  As our control extends the `android.support.design.widget.TextInputLayout` control, it is important to first consider what style attributes is already being used so that we can associate our own styles with the existing styles.  From the existing [Android styles definition](https://android.googlesource.com/platform/frameworks/support.git/+/master/design/res/values/styles.xml) we see that Android widget has the following styles definition:

```xml
<style name="Widget.Design.TextInputLayout" parent="android:Widget">
  <item name="hintTextAppearance">@style/TextAppearance.Design.Hint</item>
  <item name="errorTextAppearance">@style/TextAppearance.Design.Error</item>
  <item name="counterTextAppearance">@style/TextAppearance.Design.Counter</item>
  <item name="counterOverflowTextAppearance">@style/TextAppearance.Design.Counter.Overflow</item>
  <item name="passwordToggleDrawable">@drawable/design_password_eye</item>
  <item name="passwordToggleTint">@color/design_tint_password_toggle</item>
  <item name="passwordToggleContentDescription">@string/password_toggle_content_description</item>
</style>
```
The `hintTextAppearance` and `errorTextAppearance` is being used by the Android widget to style the label and error text.  With this in mind, we can now create a style definition for our own control:

```xml
<style name="Widget.InTouch.TextInputLayout" parent="Widget.Design.TextInputLayout">
  <item name="android:textColorHint">@color/accentColor</item>
  <item name="hintTextAppearance">?attr/ItcTextAppearanceLabel</item>
  <item name="errorTextAppearance">?attr/ItcTextAppearanceError</item>      
</style>
```
We override the default style styles and associate them with our new common-look-and-feel style attributes that we have defined.  With our control style defined, we also create the default text appearance styles to use for our common-look-and-feel:

```xml
<style name="TextAppearance.InTouch" parent="TextAppearance.AppCompat">
</style>

<style name="TextAppearance.InTouch.Label">
  <item name="android:textSize">14sp</item>
</style>

<style name="TextAppearance.InTouch.Label.Error">
  <item name="android:textSize">12sp</item>
  <item name="android:textColor">@color/errorColor</item>
</style>
```

## Themes ##

With the styles defined, we now set out to create a default Theme where we assign the default styles to the style attributes we created to create our common look-and-feel.  Themes are created in a `Resources/values/themes.xml` file:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="Theme.InTouch" parent="Theme.InTouch.Base">
  </style>

  <style name="Theme.InTouch.Base" parent="Theme.Design.Light.NoActionBar">

    <item name="colorPrimary">@color/primaryColor</item>
    <item name="colorPrimaryDark">@color/primaryColorDark</item>
    <item name="colorAccent">@color/accentColor</item>

	<!-- Composites -->
    <item name="ItcTextAppearanceLabel">@style/TextAppearance.InTouch.Label</item>
    <item name="ItcTextAppearanceError">@style/TextAppearance.InTouch.Label.Error</item>
    <item name="ItcTextInputLayoutStyle">@style/Widget.InTouch.TextInputLayout</item>   
  </style>
```
Notice that our theme associates the default text appearance with a specific style.  With the theme definition in place, we can now use it our applications in Xamarin by typically assigning the `Theme` property as an attribute on our startup `Activity` or on the `Application` instance as illustrated below:

```csharp
[Application(Name = "phil.AndroidApp", Theme = "@style/Theme.InTouch")]
public class AndroidApp : Application, Application.IActivityLifecycleCallbacks
{
```

## Control changes ##

With the style attributes and themes defined, we still need to enhance our control to make use of the styles defined.  To cater for the styles and themes we need to change our existing control to get the style definitions and use them for changing the look-and-feel of its internal widgets:

<script src="https://gist.github.com/cjlotz/a179b8552dc23fa1a3d590bef0ff1133.js"></script>

The three different constructors cater for different scenarios of creating our compound control.  The first constructor on Line 3 is typically used when I create a control programmatically.  The second constructor on Line 7 is called when I define my control in a layout **without** specifying a style.  

```xml
<intouch.controls.ITTextInputLayout
    android:id="@+id/layout_username"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:imeOptions="actionNext" ...
```

The third constructor is called when I define my control in a layout **with** specifying a style:

```xml
<intouch.controls.ITTextInputLayout
    style="@style/MyCustom.Style"
    android:id="@+id/layout_username"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:imeOptions="actionNext" ...
```

Notice how the constructors call into each other and the use of the `Resource.Styleable.ITControls_ItcTextInputLayoutStyle` style attribute on Line 7 that will use the style setup in our theme as the default if the user does not specify their own style for our control in a layout.  Our theme is setup to use `@style/Widget.InTouch.TextInputLayout` as style which makes use of the default look-and-feel style attributes to style the control.  

As we are re-using the existing styles defined on the Android `TextInputLayout` control, we simply have to call into the base class constructor to get these styles applied to the `Error` and `Hint` widgets.  Feel free to browse the [TextInputLayout.java source code](https://android.googlesource.com/platform/frameworks/support.git/+/master/design/src/android/support/design/widget/TextInputLayout.java) if you want to see how this is done.

## Summary ##

[Styles and Themes](https://developer.android.com/guide/topics/ui/themes.html) in Android along with style inheritance is a really powerful tool for creating a common look-and-feel across your application.  I hope this post highlighted some ways in which you can make use of this powerful functionality to create a common look-and-feel for your own compound controls.