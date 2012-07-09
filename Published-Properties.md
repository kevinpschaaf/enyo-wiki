# Published Properties

_enyo.Object_ implements the Enyo framework's property publishing system.
Published properties are declared in a hash called _published_ within a call
to _enyo.kind_. Getter and setter methods are automatically generated for
properties declared in this manner. Also, by convention, the setter for a
published property will trigger an optional _&lt;propertyName&gt;Changed_ method
when called.

In the following example, _myValue_ becomes a regular property on the MyObject
prototype (with a default value of 3), and the getter and setter methods are
generated as noted in the comments:

	enyo.kind({
		name: "MyObject",
		kind: enyo.Object,

		// declare 'published' properties
		published: {
			myValue: 3
		},

		// These methods will be automatically generated:
		//	getMyValue: function() ...
		//	setMyValue: function(inValue) ...

		// optional method that is called whenever setMyValue is called
		myValueChanged: function(inOldValue) {
			this.delta = this.myValue - inOldValue;
		}
	});

Since we have declared a _changed_ method (i.e., _myValueChanged_) to observe
set calls on the _myValue_ property, it will be called whenever _setMyValue_ is
called, as illustrated by the following:

	myobj = new MyObject();
	var x = myobj.getMyValue(); // x gets 3

	myobj.setMyValue(7); // myValue becomes 7; myValueChanged side-effect sets delta to 4

_Changed_ methods are called whenever setters are invoked, whether or not the
actual value has changed.

Published properties are stored as regular properties on the object prototype,
so it's possible to query or set their values directly:

	var x = myobj.myValue;

Note that when you set a property's value directly, the _changed_ method is not
called.