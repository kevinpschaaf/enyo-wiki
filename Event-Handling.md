> New for Enyo 2.0.1+

Enyo uses a message passing strategy to communicate between objects indirectly. We use the term _event_ for messages because it dovetails with common DOM usage. When using the _dom_ package in Enyo (default in core), DOM events and custom events are unified.

## Handler Syntax

An event handler in Enyo looks like this:

	myEventHandler: function(inSender, inEvent) {
		// return true; // return true to indicate this event was handled, and stop propagation
	}

`inSender` is the enyo.Component that passed the event to `this`. 

`inEvent` is an object that contains event data. For DOM events, this is the standard DOM event object. For custom events, it's a custom object.

The handler can return a truthy value to stop propagating the event. 

**Note:** the meaning of the return value is different from the classic DOM convention (historically, return value determines whether the default action occurs). If you need to control the default action on a DOM event, use the modern equivalent `inEvent.preventDefault()`. 

**Note:** `inEvent.stopPropagation()` will not prevent propagation on events in Enyo, return true from the handler instead.

Because events can travel, the sender of an event is different from the originator of an event. The originator of an event is available as `inEvent.origin`.

For example, when clicked, a button originates an _onclick_ which bubbles up the control chain. The button's parent my bubble the event up to the button's grandparent. From the grandparent's perspective, the origin is the button and the sender is the button's parent.

## Attaching Handlers to Events

There are two primary ways of listening to an event in a Component. 

The first way is to set a handler name on an owned object, like so:

	components: [
		{name: "thing", ontap: "thingTap"}
	],
	thingTap: function(inSender, inEvent) {
		// do stuff
	}

The second way is to name a catch-all handler in the handlers block, like so:

	handlers: {
		ontap: "anythingTap"
	},
	anythingTap: function(inSender, inEvent) {
		// do stuff
	}

**Note:** `enyo.Control` creates a default handler mapping of `ontap` to `tap`. Therefore, to handle generic taps on a Control, you need only implement a `tap` method.

	tap: function(inSender, inEvent) {
		// tap events come here
	}

**Note:** If you use both methods at once, you will receive the event in both places by default. However, you can control this behavior by preventing propagation in `thingTap`. E.g.

	components: [
		{name: "thing", ontap: "thingTap"}
	],
	handlers: {
		ontap: "anythingTap"
	},
	thingTap: function(inSender, inEvent) {
		// taps on _thing_ will bubble up to _anythingTap_ also, unless I stop propagation here
		return true; // handled here, don't propagate
	}
	anythingTap: function(inSender, inEvent) {
		// do stuff
	}

If you need more fancy handling, you can use the `inSender` and `inEvent.origin` properties to help you discern the provenance of the event.
	
## Sending Events

There are two common ways of sending an event. You can _bubble_ an event up, or _waterfall_ an event down.

`this.bubble(inEventName <, inEvent, inSender>)`
`this.waterfall(inEventName <, inEvent, inSender>)`

`bubble` sends the event up the object chain. `waterfall` sends the event down through the object tree.

`inEventName` is the event name. 

`inEvent` is an optional object containing event specified information (this is the same object listeners receive as `inEvent` although it may be decorated, e.g. with the `origin` property). 

`inSender` should almost always be omitted, although you could use it to force a particular sender for the next handler.

**Note:** `inEventName` should include the _on_ prefix. In other words, a handler map like this:

	handlers {
		onActivate: "activate"
	}

can capture an event sent like this:

	this.bubble("onActivate");

**Note:** In Enyo, DOM events are allowed to bubble all the way to `document` where they are handled by `enyo.dispatcher`. The dispatcher figures out where to send the event, and provides hooks for various bits of event processing. Whenever possible, dispatcher avoids disturbing original DOM events. Value-add events from dispatcher are sent as synthesized events, such as `ontap`, `ondown`, `onup`, `ondragstart`, `ondrag`, `ondragfinish`, `onenter`, and `onleave`. Most of these DOM-like events work cross-platform, so client code need not worry about touch vs mouse interfaces.
