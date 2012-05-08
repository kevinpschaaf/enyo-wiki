## Lifecycle Methods in Enyo

### Trivial Kind

Remember that Enyo _kinds_ use regular JavaScript prototypes. A trivial kind has a simple life-cycle.

```javascript
	var MyKind = enyo.kind({
		kind: null, // otherwise it will default to 'Control'
		constructor: function() {
			// do any initialization tasks
		}
	});
```
That's it. Now `MyKind` is a function that you can use with the `new` operator to create instances. The `MyKind` function is a copy of a boilerplate function that will call your (optional) `constructor` function.

	myInstance = new MyKind();

Like all JavaScript objects, a kind-instance is garbage collected when there are no references to it. 

> The advanced features of Component and Control come from those kinds, `enyo.kind()` itself is fairly simple.

### Overrides: inherited()

It's possible to base a kind on another kind. The new kind inherits all the properties and methods of the old kind. If you override an old method, you can call it with `inherited()`.

	var MyNextKind = enyo.kind({
		kind: MyKind,
		constructor: function() {
			// do any initialization tasks before MyKind initializes
			//
			// do inherited initialization (optional, but usually a good idea)
			this.inherited(arguments);
			//
			// do any initialization tasks after MyKind initializes
		}
	});

_MyNextKind_ starts with all the properties and methods of _MyKind_, but then we override `constructor()`. Our new constructor can call the old constructor using the `this.inherited(arguments)` syntax. _MyNextKind_ can decide what things to do before or after calling the old constructor, or can choose to not call it at all.

This override system is the same for any method, not just for constructor.

> Using only raw JavaScript, you could call an inherited method like so

>		myKind.prototype.<method name>.apply(this, arguments);

> The _this.inherited_ syntax is shorter and removes the need for the super-kind name (_myKind_ in this example).

> Also note that `arguments` is a [JavaScript built-in](https://developer.mozilla.org/en/JavaScript/Reference/Functions_and_function_scope/arguments), whose value is a pseudo-array of the arguments passed to the currently executing function.

### Component: create() and destroy()

All kinds can have constructor methods as described above. The _Component_ kind adds new lifecycle methods, most importantly: `create()` and `destroy()`.

	var myComponent = enyo.kind({
		kind: enyo.Component,
		constructor: function() {
			// low-level or esoteric initialization, usually not needed at all
			this.inherited(arguments);
		},
		create: function() {
			// create is called *after* the constructor chain is finished
			this.inherited(arguments);
			// this.$ hash available only *after* calling inherited create
		},
		destroy: function() {
			// do inherited teardown
			this.inherited(arguments);
		}
	});

Technically, `create()` differs from `constructor()` in that the `constructor` call finishes completely (including any chain of inherited calls) before `create()` is called, but normal initialization can be done in either method. 

For convenience, most Components can simply ignore `constructor()` and rely on `create()`.

The other important feature of `create()` is that the `this.$` hash is ready after `Component.create()` has executed. That means that an override of create is where you generally put initialization of owned Components. For example:

	components: [
		{kind: "MyWorker"}
	],
	create: function() {
		this.inherited(arguments);
		// put MyWorker to work
		this.$.myWorker.work();
	}

Components frequently reference other components. In the example above, the `myWorker` instance is referenced by `this.$.myWorker`. The `destroy()` method cleans up these references and allows a Component to be garbage collected.  Component kinds also put general cleanup code in `destroy()`.

	destroy: function() {
		// stop MyWorker from working
		this.$.myWorker.stop();
		// standard cleanup
		this.inherited(arguments);
		// this.$ hash is empty now
	}

> If you've made a custom reference to a Component that is not cleaned up by a `destroy()` method, then calling `destroy()` will not actually make that object subject to garbage collection. For this reason there is a `this.destroyed` flag on each Component. If `this.destroyed` is true, the Component has been uninitialized and the reference should be removed.

### Control: hasNode() and rendered()

Control is a kind of Component, so controls have the same `constructor()`, `create()`, `destroy()` system described above, but Control introduces a few more methods for managing it's representation in DOM. 

Control is designed so that most methods and properties can be used without knowing whether there is corresponding DOM or not. For example, you can call

	this.$.control.applyStyle("color", "blue");

If the control is rendered, it will put that style in DOM, if not, it will store that information until the DOM is created. Either way, when you see it, the text will be blue.

There are certain things you cannot do unless DOM has been created. Obviously, you cannot do work directly on a DomNode if there is no DOM. Also, e.g., `getBounds()` returns values measured from DOM, so if there is no DOM, you won't get accurate bounds.

In a Control, if you need to do something right away after DOM is created, you can do that in the `rendered()` method:

	rendered: function() {
		// important! must call the inherited method
		this.inherited(arguments);	
		// this is the first moment bounds are available
		this.firstBounds = this.getBounds();
	}


Another important note here is that even though DOM elements have been created when `rendered()` is called, the visual representation of it in the browser is generally not visible yet. Most browsers will not update the display until Enyo yields the javascript thread. This means you have a chance to make changes to the DOM in `rendered()` without causing annoying flashes.

Whever you need to access a Control's DomNode directly, use the `hasNode()` method like this:

	twiddle: function() {
		if (this.hasNode()) {
			buffNode(this.node); // buffNode is made-up
		}
	}

> Remember that even if DOM is rendered, `this.node` may not have a value. A truthy result from `hasNode()` means `this.node` is initialized. Even in `rendered()`, you need to call `hasNode()` first.

>		rendered: function() {
			this.inherited(arguments); // important! must call the inherited method
			if (this.hasNode()) {
				buffNode(this.node);
			}
		});

### When are Controls Rendered?

In general, Controls are not rendered until you you explicitly say so. Most applications have a `renderInto()` or `write()` method at the very top that renders the Controls in the application.

Also, when creating Controls dynamically, Enyo will not render them until you call `render()` on those Controls, or one of their containers. This kind of code is common:

	updateControls: function() {
		// destroy controls we made last time 
		// ('destroyClientControls' destroys only dynamic controls, 'destroyControls' destroys everything)
		this.destroyClientControls(); 
		// create new controls
  		for (var i=0; i<this.count; i++) {
			// created but not rendered
			this.createComponent({kind: "myCoolControl", index: i});
 		}
		// render everything in one shot
		this.render();
	});

Destroying a Control will cause it to be removed from DOM right away, as this is a necessary part of the clean-up process. There is no 'unrender' method.

### A Note About Kind Names

The examples above use this syntax for creating kinds:

	var MyKind = enyo.kind(...);

Mostly in Enyo code you see this instead:

	enyo.kind({
		name: "MyKind"
		...
	});

Including a `name` property is optional. If you include it, Enyo will make a global reference to your kind using that name, and the name is available in instances as `this.kindName`. The `name` syntax is used mostly for these conveniences. For example, instead of

	var foo = {
		kinds: {
			MyKind: enyo.kind(...);		
		}
	}

one can write

	enyo.kind({
		name: "foo.kinds.MyKind",
		...
	});