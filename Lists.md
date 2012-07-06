# Lists

Most applications need to display a list of items of one kind or another.
Sometimes the items in the list are very simple, like the name of a contact,
while other times they are complex, like a drawer containing prompts and
buttons.

It's quite a challenge to create a `List` control supporting a large number of
items that can be rendered and scrolled with good performance across the range
of devices that Enyo supports.  For this reason, Enyo has two broad strategies
for dealing with lists of data.  When an application needs a relatively small
number of items (up to ~100) that are relatively complex, an `enyo.Repeater`
should be used.  When an application needs a large number of relatively simple
items (into the millions), an `enyo.List` should be used.

## Repeater

`enyo.Repeater` is a simple control for making lists of items.  A repeater does
just what its name implies--it repeats the set of controls that are contained
within it.  The components of a repeater are copied for each item created, and
are	wrapped	in a control that keeps the state of the item index.  Any control
may be placed inside a repeater, and applications may interact with these
controls normally.

The `count` property specifies the number of times the item controls are
repeated; for each repetition, the `onSetupItem` event is fired.  You may handle
this event to customize the settings for individual rows, e.g.:

	{kind: "Repeater", count: 2, onSetupItem: "setImageSource", components: [
		{kind: "Image"}
	]}

	setImageSource: function(inSender, inEvent) {
		var index = inEvent.index;
		var item = inEvent.item;
		item.$.image.setSrc(this.imageSources[index]);
		return true;
	}

Be sure to return `true` from your _onSetupItem_ handler to prevent other event
handlers further up the tree from trying to modify your item control.

The repeater will always be rebuilt after a call to _setCount_, even if the
count didn't change.  This behavior differs from that of most properties, for
which no action happens when a set-value call doesn't modify the value.	 This is
done to accomodate potential changes to the data model for the repeater, which
may or may not have the same item count as before.

(**Note:** If the contents of a repeater should scroll, then the repeater should
be placed inside an `enyo.Scroller`.)

## List

`enyo.List` is a control that displays a scrolling list of rows.  It is
designed to render a very large number of rows efficiently, having been
optimized such that only a small portion of	the list is rendered at a given
time.  This is done using a
[flyweight pattern](http://en.wikipedia.org/wiki/Flyweight_pattern), in which
controls placed inside the list are created once, but rendered for each list
item.  For this reason, it's best to use only simple controls in a List, such as
<a href="#enyo.Control">enyo.Control</a> and <a href="#enyo.Image">enyo.Image</a>.

(Note that `enyo.List` includes a scroller; therefore, it should *not* be placed
inside an `enyo.Scroller`.)

A List's _components_ block contains the controls to be used for a single row.
This set of controls will be rendered for each row.	 You may customize the row
rendering by handling the `onSetupItem` event.  For example, given the following
list...

	components: [
		{kind: "List", fit: true, count: 100, onSetupItem: "setupItem", components: [
			{classes: "item", ontap: "itemTap", components: [
				{name: "name"},
				{name: "index", style: "float: right;"}
			]}
		]}
	]

...one might write event handlers like so:

	setupItem: function(inSender, inEvent) {
		// given some available data.
		var data = this.data[inEvent.index];
		// setup the controls for this item.
		this.$.name.setContent(data.name);
		this.$.index.setContent(inEvent.index);
	},
	itemTap: function(inSender, inEvent) {
		alert("You tapped on row: " + inEvent.index);
	}

As this example illustrates, the identity of the row from which the event
originated is available to us in the event's `index` property (i.e.,
`inEvent.index`).

It is possible to alter the contents of a row in an `enyo.List`, but in order to
do so effectively, one must understand the implications of the flyweight pattern
used in lists.

## Flyweight

Any time a list row is rendered, the `onSetupItem` event is fired.  An
application can therefore control the rendering of the controls in a row by
calling methods on those controls within this event's handler.  An application
can update a specific row by forcing it to render using the list's
`renderRow(inIndex)` method.

In addition, it is possible to create controls with more complex interactions
that are specifically tailored to function correctly in a flyweight context.
Events for controls in a list will be decorated with both the index of the row
being interacted with and the flyweight controller for the control (i.e.,
`event.index` and `event.flyweight`).  The list's `prepareRow(inIndex)` method
can be used to assign the list's controls to a specific list row, allowing
persistent interactivity with that row.  When the interaction is complete, the
list's `lockRow` method should be called.

The `onyx.SwipeableItem` kind may be a useful reference when using these methods.
