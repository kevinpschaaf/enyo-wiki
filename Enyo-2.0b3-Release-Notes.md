It's late March and we're pleased to release the third beta version of Enyo 2.0.  Here's a summary of
changes in the core Enyo code along with the bundled libraries like Onyx and layout.

Enyo
----

For Enyo, one big change is the reintroduction of "allowHtml" into enyo.Control.  When it's false (the default value), the "content" is considered to be plain text and any HTML is escaped when the DOM node is rendered.  This is primarily for security reasons; you should only set "allowHtml" when you directly control the content or you can be sure it doesn't contain any malicious elements.

Other changes include:

* new method `enyo.now()` returns a Date object with the current time
* `enyo.Component.createComponents` now returns an array of the created items
* `enyo.Control` now has a `noDom` attribute that can be set to true to avoid having it 
  create a DOM node. This is useful for containers that hold other controls, but shouldn't be represented 
  in the output directly.
* `enyo.job` has been moved into the core code from the extra library
* new control __enyo.TextArea__ that's a UI control based on enyo.Input, but wraps the `<textarea>` HTML element instead.
* new control __enyo.RichText__ that's a multiline text input control supporting HTML formatting
* `enyo.Input` now supports a `type` property that's mapped to the HTML 5 type attribute
* unified platform detection code to better handle differences among desktop, iOS, Android, and webOS targets.
* bug fix to allow using prefixed package files (e.g. "foo-package.js") from a `<script>` tag; before only "package.js" 
  with no prefix was allowed
* the synthesized `flick` event now has coordinates of the start of the flick, not the end
* new 'holdpulse' event sent periodically when a touch or click is held on an element to allow triggering
  actions after a certain time.
* The `onrelease` event occurs only if a small hysteresis is satisfied
* When the fit attribute is true, `enyo.Control.write()` and `enyo.Control.renderInto()` now setup 
  the document `<body>` element to be able to properly host fitted content.
* `enyo.Scroller`: changed horizontal/vertical settings so they make more sense, window scrolling is now
  prevented on iOS when using a native touch region scroller and dragging beyond a scroll boundary. 
* The `enyo.Scroller.scrollTo()` method now animates when using a TouchScrollStrategy.
* `enyo.macroize` was optimized and a sister method, `enyo.quickMacroize()` was added that's faster but more limited.

Onyx
----

For Onyx, there are several new controls:

* `onyx.Slider`: an input that lets you pick a value from a range
* `onyx.ProgressBar`: read-only controls showing a variable value
* `onyx.ProgressButton`: progress bars with an internal cancel icon
* `onyx.TextArea`: Onyx styling around enyo.TextArea
* `onyx.RichEdit`: Onyx styling around enyo.RichEdit

Also changed in Onyx:

* More onyx.PopUp samples to the OnyxSampler sample application.
* Events capturing is released when an `onyx.Popup` is destroyed
* animations are stopped when `onyx.Animator` is destroyed
* `onyx.Icon` uses `enyo.path.rewrite` to rewrite the src value so you can use aliases
* Slideable: add onChange event.

Layout
------

The layout Fittable library has been refactored extensively with one API-visible change: the 
property `stretch` has changed to `noStretch` and now defaults to false.  When this is true,
the box will have a natural height or width instead of being stretched to fill the space.
For example, if you have a column of buttons in a FittableRows type, they would be stretched out
horizontally to fill the space unless noStretch was set to true on them.

The Fittable library now had additional examples included to show how the various properties and
kinds interact.