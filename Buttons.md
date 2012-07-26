# Buttons

The Onyx library provides a rich assortment of buttons for use in your Enyo
applications.  This document surveys the types of buttons that you are most likely to use.

## onyx.Button

`onyx.Button` derives directly from `enyo.Button` and provides the same basic functionality, while adding a modicum of visual styling.

    {kind: "onyx.Button", content: "tap me"}

![tap me button](https://github.com/enyojs/enyo/wiki/assets/buttons-1.png)

When an `onyx.Button` is tapped,
it generates an `ontap` event; you may respond to the event by specifying a handler method, e.g.:

    {kind: "onyx.Button", content: "tap me", ontap: "buttonTapped"},

    ...

    buttonTapped: function(inSender, inEvent) {
        // respond to the tap event
    }

In addition, you may customize the look of a button by specifying foreground and background colors, or by applying one of Onyx's built-in button styles:

    {kind: "onyx.Toolbar", components: [
        {kind: "onyx.Button", content: "tap me"},
        {kind: "onyx.Button", content: "purple", style: "background-color: purple; color: #F1F1F1;"},
        {kind: "onyx.Button", content: "yes", classes: "onyx-affirmative"},
        {kind: "onyx.Button", content: "no", classes: "onyx-negative"},
        {kind: "onyx.Button", content: "onyx-blue", classes: "onyx-blue"}
    ]}

![Buttons in a Toolbar](https://github.com/enyojs/enyo/wiki/assets/buttons-2.png)

You may also place an image inside a button, with or without accompanying text, as in the following examples:

    {kind: "onyx.Button", ontap:"buttonTapped", components: [
        {kind: "onyx.Icon", src: "https://github.com/enyojs/enyo/wiki/assets/fish_bowl.png"}
    ]}

![Fish Bowl Button](https://github.com/enyojs/enyo/wiki/assets/buttons-3.png)

    {kind: "onyx.Button", ontap:"buttonTapped", components: [
        {tag: "img", attributes: {src: "https://github.com/enyojs/enyo/wiki/assets/fish_bowl.png"}},
        {content: "Go Fish"}
    ]}

![Go Fish Button](https://github.com/enyojs/enyo/wiki/assets/buttons-4.png)

## onyx.IconButton

Similar effects may be achieved using `onyx.IconButton`, a subkind of `onyx.Icon`.  For instance, the code

    {kind: "onyx.IconButton", src: "assets/my_icon.png"}

yields an icon that acts like a button, but is not displayed against the shaded rectangular background generally associated with buttons.

One may also use `onyx.Icon` to create a button with both text and an image:

    {kind: "onyx.Button", ontap: "buttonTap", components: [
        {kind: "onyx.Icon", src: "assets/my_icon.png"},
        {content: "tap me"}
    ]}

A key difference between `onyx.IconButton` and `onyx.Button` is that the image associated with the IconButton's `src` property is assumed to be a 32x64-pixel strip, with the top half showing the button's normal state and the bottom half showing its state when active.  (By contrast, when you activate an `onyx.Button` that contains an image, the state change is reflected in the button's background, but not in the image itself.)