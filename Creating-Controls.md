# Creating Controls

A `Control` is a component that controls a DOM node (i.e., an element in the user interface). Controls are generally visible and the user often interacts with them directly. Things like buttons and input boxes are obviously controls, but in Enyo a control may become as complex as an entire application.

## The Basics

In the following example, we define a `Circle` control, which we will put to use inside a `TrafficLight` control:

	enyo.kind({
		name: "Circle",
		kind: "Control",
		published: {
			color: "magenta",
			bgColor: "black"
		},
		handlers: {
			down: "downHandler",
			up: "upHandler"
		},
		content: "Hi",
		style: "padding: 2px 6px; border: 3px solid; border-radius: 20px; cursor: pointer;",
		create: function() {
			this.inherited(arguments);
			this.colorChanged();
		},
		colorChanged: function() {
			this.applyStyle("border-color", this.color);
		},
		bgColorChanged: function() {
			this.applyStyle("background-color", this.bgColor);
		},
		downHandler: function(inSender, inEvent) {
			this.applyStyle("background-color", "white");
		},
		upHandler: function(inSender, inEvent) {
			this.applyStyle("background-color", "black");
		}
	});

The `Circle` is a `Control` kind and therefore inherits and extends the behavior of `Control`. Since a control is a component, it can publish properties, as is done here.

## Manipulating a Control's DOM Node

A control exposes functionality to manipulate its DOM node. Notice the use of content and style in the `Circle` object. The `content` property sets the HTML that the control will render. Since the `Control` object publishes the `content` property, you can call `setContent` and `getContent`. The `style` property specifies CSS styles the control will use to decorate its node. You can also specify classes, attributes, or even a tag type. For example:

	{tag: "input", classes: "rounded", attributes: {value: "foo"}}

These properties can be set either on control configuration blocks or in kind definitions.

There are also methods available to modify these properties; for example, `applyStyle` sets a specific style property to a given value. There are many other such methods, including `addStyles`, `addClass`, `setAttribute`, `show`, `hide`, and `render`. See the API documentation for more info.

## Controls in Controls: It's Those Turtles Again

Here is our aforementioned `TrafficLight` control:

	enyo.kind({
		name: "TrafficLight",
		kind: "Control",
		style: "position: absolute; padding: 4px; border: 1px solid black; background-color: silver;"},
		components: [
			{kind: "Circle", color: "red", ontap: "circleTap"},
			{kind: "Circle", color: "yellow", ontap: "circleTap"},
			{kind: "Circle", color: "green", ontap: "circleTap"}
		],
		circleTap: function(inSender, inEvent) {
			var lights = {red: "tomato", yellow: "#FFFF80", green: "lightgreen"};
			if (this.lastCircle) {
			  this.lastCircle.setBgColor("black");
			}
		 	this.lastCircle.setBgColor(lights[inSender.color]);
			this.lastCircle = inSender;
		}
	});

Since they are components, controls may contain other controls. A control will render any controls contained inside itself. Thus, a `TrafficLight` will render with three `Circle` instances inside it and, if our styling is correct, will look like an actual traffic light. Note that if the control contains other controls and also specifies a value for `content`, it will render the child controls and ignore the value of `content`.

## DOM and DOM-like Events

Enyo controls can handle common DOM events. In a component configuration block, specify the handler for a DOM event just as you would for any other Enyo event--with a named delegate. The `TrafficLight` kind does this for its circle controls by setting `ontap: "circleTap"`. Since `TrafficLight` owns its circles, it will process their events; thus, `circleTap` should be the name of a handler method inside `TrafficLight`.

The `ontap` event is not strictly a DOM event; it's part of the set of cross-platform gesture events Enyo exposes, and therefore it behaves like a DOM event and we call it DOM-like. These events are provided so that users can write a single set of event handlers for applications that run on both mobile and desktop platforms.

Now, the `Circle` kind handles some DOM events itself. It reacts when the user presses down and then releases. DOM events that a kind should handle are specified in the `handlers` block. The `Circle` kind specifies handlers for the `down` and `up` events. These handlers are string delegate names of methods in the `Circle` kind that will handle the events. Again, the `down` and `up` events are not strictly DOM events, but are DOM-like.

As with all events, the first argument sent to the event handler is the `inSender` object, the Enyo control that generated the event. DOM events pass the event object as the second argument.

Since DOM events bubble up through the DOM, they may be handled by any control in the control hierarchy. For example, a user of `TrafficLight` could implement an `ontap` event handler and receive taps on the circles inside the TrafficLight. Note that DOM event bubbling may be aborted in Enyo by returning `true` from an event handler. In addition to the `target` property, which is set on all event objects, Enyo specifies a `dispatchTarget` property, which is set to the Enyo control containing the event target.

The following DOM events are handled by Enyo:

* mousedown
* mouseup
* mouseover
* mouseout
* mousemove
* click
* dblclick
* change
* input
* keydown
* keyup
* keypress
* resize
* load
* unload
* message

The following DOM-like cross-platform events are generated by Enyo:

* down
* up
* tap
* move
* enter
* leave
* hold
* release
* holdpulse
* flick
* dragstart
* drag
* dragfinish
* drop
* dragover
* dragout

If there are additional DOM events (e.g., mousewheel) that you want Enyo to handle, do the following:

	document.onmousewheel = enyo.dispatch;