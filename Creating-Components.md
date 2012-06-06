# Creating Components

The Component object (`enyo.Component`) is the basic building block of Enyo. Components encapsulate rich behavior and may be used together as modules to create complex applications. When coding an Enyo application, you'll create many of your own component and control kinds. Here are a few steps to get started.

## The Basics

A component is an Enyo kind that can publish properties, expose events, and contain other components. It may be useful to think about components as owning a set of content (other components) and providing inputs (methods and property setters) and outputs (events and property getters). A component controls the content it owns and sends messages to its owner in the form of events. Here's an example: 

	enyo.kind({
		name: "RandomizedTimer",
		kind: enyo.Component,
		minInterval: 50,
		published: {
			baseInterval: 100,
			percentTrigger: 50
		},
		events: {
	    	onTriggered: ""
		},
		create: function() {
	    	this.inherited(arguments);
		    this.start();
		},
		destroy: function() {
			this.stopTimer();
			this.inherited(arguments);
	  	},
		start: function() 
			this.job = window.setInterval(enyo.bind(this, "timer"), this.baseInterval);
		},
		stop: function() {
			window.clearInterval(this.job);
		},
	  	timer: function() {
	    	if (Math.random() < this.percentTrigger * 0.01) {
				this.doTriggered(new Date().getTime());
			}
		},
		baseIntervalChanged: function(inOldValue) {
			this.baseInterval = Math.max(this.minInterval, this.baseInterval);
			this.stop();
			this.start();
		}
	});

As the name implies, this is a simple randomized timer component. Its kind is `enyo.Component`, so it inherits and extends the behavior of the `enyo.Component` object. It generates an event when the timer fires and exposes a couple of properties to control frequency. As you can see, it's straightforward to expose properties and events.

## Properties

Properties are placed in a `"published"` block and may include a default value. For convenience, published properties are automatically provided with setter, getter, and changed methods. For example, `setBaseInterval` and `setPercentTrigger` can be used to change the timer frequency. When `setBaseInterval` is called, our implementation requires that we update the timer firing code. This is sometimes called a side effect of setting the property. It's implemented in the `baseIntervalChanged method`, which is called by `setBaseInterval` when the value of `baseInterval` changes. Note that it's often necessary to initialize a property value. We typically do this by calling the property changed method in `create`; since the need can vary by use case, we leave this initialization to the component writer's discretion.

## Events

Similarly, events are placed in an `"events"` block. To fire an event, we call the associated `"do"` method, another convenience provided by Enyo. For example, to fire the `onTriggered` event, we call `doTriggered`. Any arguments given will be passed along to the event handler. Here we are sending the current time. In a moment, we'll see how to handle this event.

	enyo.kind({
		name: "SimulatedMessage",
		kind: enyo.Component,
		components: [
			{name: "timer", kind: "RandomizedTimer", percentTrigger: 10, onTriggered: "timerTriggered"}
		],
		timerTriggered: function(inSender, inTime) {
			this.log("Simulated Service Message Occurred at " + inTime);
		}
	});

## Components in Components: It's Turtles All the Way Down

Now we've created another component kind named `SimulatedMessage`. As you can see, there's a `components` block and it contains a configuration object specifying a `RandomizedTimer`. When we create a `SimulatedMessage` instance, it will create the components in its components block. We say that it "owns" these components, and it's responsible for their lifecycle. It can refer to these objects by name using the `this.$` hash; for example, `this.$.timer.setPercentTriggered(50)`. Users of the `SimulatedMessage` message component need not concern themselves with the timer component. Its behavior is encapsulated inside `SimulatedMessage`. All components are considered to be private to their owner.

## Handling Events

Having addressed the issue of component ownership, we can turn our attention to the `onTriggered` event. Notice the string set for the `onTriggered` event in the `"timer"` configuration object. This is the name of the method in `SimulatedMessage` (the owner of the timer) that will be called to handle the event. Events are delegated to the generating component's owner by way of this named delegate string. This way we avoid the pain of having an add/remove listener mechanism. The first argument sent with every event is `"inSender"`, which is a reference to the component that generated the event. This argument facilitates code reuse since the same method can be used to handle multiple events distinguished by `inSender`. The other arguments vary by event. In this case we've sent the time at which the timer fired.

## Summary

To review, components are basic building blocks that encapsulate behavior (often by using other sub-components) and expose an interface in the form of methods, properties, and events.