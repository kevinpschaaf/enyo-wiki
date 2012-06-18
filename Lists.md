# Lists

Most applications need to display a list of items of one kind or another. Sometimes the items in the list are very simple like the name of a contact, and other times they are complex like a drawer containing prompts and buttons.

It's quite a challenge to create a `List` control supporting a large number of items that can be rendered and scrolled with good performance across the range of devices Enyo supports.  For this reason, Enyo has two broad strategies for dealing with lists of data.  When an application needs a relatively small number of items (~100) that are relatively complex, an `enyo.Repeater` should be used.  When an application needs a large number of relatively simple items (into the millions), an `enyo.List` should be used.

## Repeater

A `Repeater` does just what its name implies--it repeats the set of controls that are contained within it.  The set of controls in a repeater is created and rendered for each item in the repeater.  Any control may be placed inside a repeater, and applications can interact with these controls normally.  The `count` property species the number of times the item controls are repeated; for each repetition, the `onSetupItem` event is fired.  Implement this event to customize settings for individual rows.  For example, given a setup like this...

	components: [
		{kind: "Scroller", fit: true, components: [
			{kind: "Repeater", count: 100, onSetupItem: "setupItem", components: [
				{name: "item", classes: "item", components: [
					{name: "input", kind: "Input", onchange: "inputChange"}
				]}
			]}
		]}
	]

...one might write event handlers like this:

	setupItem: function(inSender, inEvent) {
		// given some available data.
		var data = this.data[inEvent.index];
		// setup the controls for this item.
		inEvent.item.$.input.setValue(data.name);
	},
	inputChange: function(inSender, inEvent) {
		var data = this.data[inEvent.index];
		data.name = inSender.getValue();
		// sample of propagating change to some data store
		this.updateData(data);
	}

(**Note:** If the contents of a repeater should scroll, then the repeater should be placed inside an `enyo.Scroller`.)

## List

An `enyo.List` is designed to render a very large number of rows efficiently. To achieve this, a [flyweight pattern](http://en.wikipedia.org/wiki/Flyweight_pattern) is used.  This means the controls placed inside the list are created once, but are rendered for each list item.  For this reason, it's best to use only simple controls in an `enyo.List`, such as `enyo.Control` and `enyo.Image`.

Note that the `enyo.List` includes a scroller and therefore should *not* be placed inside an `enyo.Scroller`.

For the following list...

	components: [
		{kind: "List", fit: true, count: 100, onSetupItem: "setupItem", components: [
			{classes: "item", ontap: "itemTap", components: [
				{name: "name"},
				{name: "index", style: "float: right;"}
			]}
		]}
	]

...one might write event handlers like this:

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

It's possible for the list row to contain many controls with complex styling. However, only simple Enyo kinds like `Control` and `Image` should be used.  It is possible to alter the contents of a row in an `enyo.List`, but in order to do so effectively, one must understand the implications of the flyweight pattern used in lists.

## Flyweight

Any time a list row is rendered, the `onSetupItem` event is fired.  An application can therefore control the rendering of the controls in a row by calling methods on those controls within this event handler.  An application can update a specific row by forcing it to render using the list's `renderRow(inIndex)` method.

In addition, it is possible to create controls with more complex interactions that are specifically tailored to function correctly in a flyweight context. Events for controls in a list will be decorated with both the index of the row being interacted with and also the flyweight controller for the control (i.e., `event.index` and `event.flyweight`).  The list's `prepareRow(inIndex)` method can be used to assign the list's controls to a specific list row, allowing persistent interactivity with that row.  When the interaction is complete, the list's `lockRow` method should be called.  The `onyx.SwipeableItem` kind may be a useful reference when using these methods.
