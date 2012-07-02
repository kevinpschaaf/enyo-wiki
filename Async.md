# Async

`enyo.Async` is the base kind for handling asynchronous operations.
	
`enyo.Async` is an **Object**, not a **Component**; thus, you may not declare an
`Async` in a `components` block. If you want to use `Async` as a component, you
should probably be using <a href="#enyo.WebService">enyo.WebService</a> instead.

An Async object represents a task that has not yet completed. You may attach
callback functions to an Async, to be called when the task completes or
encounters an error.

To use an Async, create a new instance of `enyo.Async` or a kind derived from
it, then register handlers in the `response` and `error` methods.

Start the async operation by calling the `go` method.

Handlers may either be methods with the signature `(asyncObject, value)` or new
instances of `enyo.Async` or its subkinds. This allows for chaining of Async
objects (e.g., when calling a Web API).

If a response method returns a value (other than undefined), that value is sent
to subsequent handlers in the chain, replacing the original value.

A failure method can call `recover` to undo the error condition and switch
to calling response methods.

The default implementation of `go` causes all the response handlers to be called
(asynchronously).

The following is a complicated example, which demonstrates many of the
aforementioned features:

    var transaction = function() {
        // Create a transaction object.
        var async = new enyo.Async();
        // Cause handlers to fire asynchronously (sometime after we yield this thread).
        // "initial response" will be sent to handlers as inResponse
        async.go("intial response");
        // Until we yield the thread, we can continue to add handlers.
        async.response(function(inSender, inResponse) {
            console.log("first response: returning a string, subsequent handlers receive this value for 'inResponse'");
            return "some response";
        });
        return async;
    };

Users of the `transaction` function can add handlers to the Async object
until all functions return (synchronously):

    // Get a new transaction; it's been started, but we can add more handlers
    // synchronously.
    var x = transaction();

    // Add an handler that will be called if an error is detected. This handler
    // recovers and sends a custom message.
    x.error(function(inSender, inResponse) {
        console.log("error: calling recover", inResponse);
        this.recover();
        return "recovered message";
    });

    // Add a response handler that halts response handler and triggers the
    // error chain. The error will be sent to the error handler registered
    // above, which will restart the handler chain.
    x.response(function(inSender, inResponse) {
        console.log("response: calling fail");
        this.fail(inResponse);
    });

    // Recovered message will end up here.
    x.response(function(inSender, inResponse) {
        console.log("response: ", inResponse);
    });