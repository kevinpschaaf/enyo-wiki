# Event Handling

Enyo employs a message-passing strategy for indirect communication between components.  We refer to these messages as _events_ to dovetail with common DOM usage.  In general, events bubble up the component tree from child to parent.  When using the `dom` package (part of the Enyo core), DOM events and custom events are unified.

The use of events is key to enforcing encapsulation in your component design.  In most cases, the children of a component should have no knowledge of their parent.  So rather than having a child call functions on its parent (and thus tightly bind itself to the parent), a child should instead send events up to its parent, which may choose to handle (or not handle) particular events bubbling up from the child.

## Sending Events

A component declares the events that it sends using an `events` block, e.g.:

	events: {
		onStateChanged:""
	}

Note that, by convention, event names always start with "on".

For every event registered in a component's `events` block, a helper function `do<EventName>(inEvent)` is created on the kind, which the component may call to send the event up the component tree.  This function takes an optional `inEvent` parameter that can contain event-specific information to be passed to the handler.  For example, to send the "onStateChanged" event from the example above, a component would call:

	this.doStateChanged(newState)  // parameter is specific to the "onStateChanged" event

Under the hood, the `do<EventName>` function wraps Enyo's generic `bubble` function for sending events up the component tree:

	this.bubble(inEventName <, inEvent, inSender>)

* `inEventName` is the event name (including the _on_ prefix). 

* `inEvent` is an optional object containing event-specific information (this is
the same object listeners receive as `inEvent`, although it may be decorated,
e.g., with the `originator` property).  Note, this must be a JavaScript object, and not a primitive.

* `inSender` should almost always be omitted, although you could use it to force
a particular sender for the next handler.

**Note:** Declaring an `events` block and using the `do<EventName>` helper function is preferable to using `enyo.bubble`, since the `events` block is more descriptive and serves to define the interface to your kind.

## Handling Events

An event handler is a function assigned to "catch" events bubbling up from children, and looks like this:

	myEventHandler: function(inSender, inEvent) {
		// can return true to indicate that this event was handled and
		// propagation should stop
	}

* `inSender` is the `enyo.Component` that passed the event to `this`. 

* `inEvent` is an object that contains event data.  For DOM events, this is the
standard DOM event object.  For custom events, it's a custom object.

The handler can return a truthy value to stop propagating the event.  Otherwise it will continue bubbling up the component tree. 

Note that the meaning of the return value is different from the classic DOM
convention (historically, the return value would determine whether the default
action occurs).  If you need to control the default action on a DOM event, use
the modern equivalent, `inEvent.preventDefault()`.

`inEvent.stopPropagation()` will not prevent propagation of events in Enyo;
return `true` from the handler instead.

Because events will propagate until stopped, an event's sender may be different from its
originator.  The originator of an event is available as `inEvent.originator`.

For example, when clicked, a button originates an _onclick_  event, which
bubbles up the control chain.  The button's parent may bubble the event up to
the button's grandparent.  From the grandparent's perspective, the originator is
the button and the sender is the button's parent.

## Attaching Handlers to Events

There are two common ways of handling events in a `Component`.  The first is to
set a handler name on an object owned by the component, like so:

	components: [
		{name: "thing", ontap: "thingTap"}
	],
	thingTap: function(inSender, inEvent) {
		// do stuff
	}

The second is to name a catch-all handler in the `handlers` block, like so:

	handlers: {
		ontap: "anythingTap"
	},
	anythingTap: function(inSender, inEvent) {
		// do stuff
	}

Note that if you use both event handling strategies at the same time, you
will receive the event in both places by default.  You may avoid this behavior
by preventing propagation in `thingTap`.  For example:

	components: [
		{name: "thing", ontap: "thingTap"}
	],
	handlers: {
		ontap: "anythingTap"
	},
	thingTap: function(inSender, inEvent) {
		// taps on _thing_ will bubble up to _anythingTap_ also,
		// unless I stop propagation here
		return true; // handled here, don't propagate
	}
	anythingTap: function(inSender, inEvent) {
		// do stuff
	}

If you need more sophisticated handling, you can use the `inSender` and
`inEvent.originator` properties to help you discern the provenance of the event.
	

## Signals

There may be times when two distantly-related components in your app need to communicate with each other.  Using standard events would require passing an event up to a common parent (in the worst case, the top-level app kind), and then passing that event back down to the target component, which could involve a significant amount of plumbing.  For these cases, `enyo.Signals` is provided as a means of broadcasting and subscribing to global messages, bypassing the normal component tree.  Signals are also useful for hooking up non-Enyo events (e.g., PhoneGap events) to be handled by an Enyo kind.

To broadcast an event, a sender simply invokes the static `send` function on `enyo.Signals`:

	enyo.Signals.send(inEventName, inEvent);

* `inEventName` is the event name (including the _on_ prefix). 

* `inEvent` is an optional object containing event-specific information.

To listen for a signal, a component should include a `Signals` instance in its `components` block, while also specifying an event handler for the signal in question.  For example, a kind defined as follows...

	enyo.kind({
		name:"MyKind",
		components: [
			{kind: "Signals", onSDKReady: "handleSDKReady"}
		],
		handleSDKReady: function() {
			// Do stuff
		}
	})

...will handle signals dispatched by a call like this:

	enyo.Signals.send("onSDKReady");

## DOM Events

In Enyo, DOM events are allowed to bubble all the way up to
`document`, where they are handled by `enyo.dispatcher`.  The dispatcher figures
out where to send the event and provides hooks for various bits of event
processing.  Whenever possible, the dispatcher avoids disturbing original DOM
events.  

There are a number of value-add events the dispatcher sends as synthesized events, such
as `ontap`, `ondown`, `onup`, `ondragstart`, `ondrag`, `ondragfinish`,
`onenter`, and `onleave`.  Most of these DOM-like events work cross-platform, so
client code does not need to distinguish between touch and mouse interfaces.

As a matter of convention, DOM events (and events synthesized based on DOM events) remain lowercase when dispatched as Enyo events (i.e. `ontap`), but custom events declared by Enyo kinds use camel case (i.e. `onStateChanged`).