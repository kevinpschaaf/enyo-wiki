# Signals

`Signals` components are used to listen to global messages.

An object with a Signals component can listen to messages sent from anywhere by
declaring handlers for them.

DOM events that have no node targets are broadcast as signals. These events
include Window events, like `onload` and `onbeforeunload`, and events that occur
directly on `document`, like `onkeypress` if `document` has the focus. Setting
the `on<MessageName>` property to the name of an owner method causes that method
to be called when `on<MessageName>` is broadcast, for any `<MessageName>`. For
example:

    enyo.kind({
        name: "Receiver",
        components: [
            // 'onTransmission' is the message name and 'transmission' is the
            // name of a handler method in my owner.
            {kind: "Signals", onTransmission: "transmission"}
        ],
		transmission: function(inSender, inPayload) {
		}
    });

Note that, like all Enyo message handlers, the signal handler (`transmission`)
receives two parameters: a reference to the component that sent the message (in
this case, our own `Signals` object, `this.$.signals`), and any payload the
transmitter included in the broadcast.

To broadcast a message, call `send`, a static method on the `enyo.Signals`
kind:

    enyo.kind({
        name: "Sender",
        transmit: function(inPayload) {
            enyo.Signals.send("onTransmission", inPayload);
        }
    });

## Important Notes

* The signal name passed into `send` must exactly match the message name in the
    receiver `Signals` instance; both must include the _on_ prefix.

* All `Signals` instances that register a handler for a particular message name
    will receive the message.

* The `send` method is on the `enyo.Signals` kind itself, not an instance of a
    Signals component.

Do not abuse Signals. Coupling objects with global communication is considered
evil.