Since our original 2.0 beta 1 release at the end of January, there have 
been a number of changes to the API in the Enyo code.  Here's a summary of 
what has changed.

boot
----
Package loader now handles empty library paths

dom
---
Custom events have to have their value packaged in an object to allow annotation 
by the event handler system and to require the event handler prototype (inSender, 
inEvent).  Before, we allowed events to be thrown with a random argument list.

enyo.Control: 
* add get/setNodeProperty method for accessing properties of a Control's DOM node
* attributes are now handles like style and classes, allowing ones declared in the
  kind to be combined with the ones in the instance
* new default onTap handler called "tap"
* setting an attribute to null or empty string will result in it being removed

IE 8 compatibility changes for event dispatching

Scrollbar interaction fixes for dragging

Gesture events produced by Enyo now have a which property identifying the mouse button 
used (1 for left, 2 for middle, 3 for right). Touch-originated gestures will have which set to 1.

kernel
------

Event handlers for DOM events are no longer implicitly named as "handleEventName", but now 
are mapped through a handlers object, mapping event strings to handler methods.

Events now will bubble from children to parents and back down the system.  See 
[Event Handling][https://github.com/enyojs/enyo/wiki/Event-Handling] for details.

touch
-----
Lots of updates to enyo.Scroller to work better on iOS and Android devices.

ui
--

In preparation for our new Onyx UI widget set, we've been reimplementing a 
lot of the base user interface behavior and bindings associated with widgets 
into a new folder called "ui".

The new classes are:

* enyo.BaseLayout - derived from enyo.Layout, a layout kind using the enyo-fit style
* enyo.Input - a wrapper around the `<input>` tag
  * enyo.Checkbox - specialization for checkbox inputs
* enyo.DragAvatar - ???? used for drag and drop ????
* enyo.Group - a container that holds GroupItems
* enyo.GroupItem - a item that can be grouped
  * enyo.ToolDecorator - a group item that can be part of a toolbar
    * enyo.Button - a wrapper for the `<button>` tag
* enyo.Image - a wrapper around the `<img>` tag
* enyo.Repeater - a control for making lists of other controls
* enyo.Select - a wrapper for the `<select>` tag
* enyo.Selection - a component for managing the selected item in a lis