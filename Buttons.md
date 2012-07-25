# Buttons

The Onyx library provides a rich assortment of buttons for use in your Enyo
applications.  This document surveys the types of buttons that you are most likely to use.

## onyx.Button

`onyx.Button` derives directly from `enyo.Button` and provides the same basic functionality, while adding a modicum of visual styling.

    {kind: "onyx.Button", content: "tap me"}

![tap me button](https://github.com/enyojs/enyo/wiki/assets/buttons-1.png)

When an `onyx.Button` is tapped,
it generates an `ontap` event; you can respond to the event by specifying a handler method, e.g.:

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