# Making Use of PhoneGap's Native Functions

In this document, we examine the relationship between Enyo and PhoneGap, making reference to a simple app that consists of the following code inside an `index.html` file:

    <!DOCTYPE html>
    <html>
    <head>
        <meta name="viewport" content="width=device-width, height=device-height, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
        <meta http-equiv="Content-type" content="text/html; charset=utf-8"/>

        <title>Enyo and PhoneGap</title>

        <!-- Enyo -->
        <script src="enyo/enyo.js" type="text/javascript"></script>

        <!-- PhoneGap support package -->
        <script src="lib/extra/phonegap/package.js" type="text/javascript"></script>
    </head>
    <body>
        <script>
            // tell Enyo to listen for deviceready event
            enyo.dispatcher.listen(document, "deviceready");

            // Application kind
            enyo.kind({
                name: "App",
                components: [
                    {kind: "Signals", ondeviceready: "deviceReady"},
                    {content: "Hello, World!"}
                ],
                deviceReady: function() {
                    // respond to deviceready event
                }
            });

            new App().renderInto(document.body);
        </script>
    </body>
    </html>

## Enyo and PhoneGap

When your application is first launched, PhoneGap sends a `"deviceready"` event to the document body. This event tells us that the application has been launched and PhoneGap has been loaded. It is at this point that we can use Enyo to render our application.

Fortunately, Enyo 2 has added native support for listening to this event.  If you are using the [latest Enyo 2 build](https://github.com/enyojs/enyo) from GitHub, you already have the file that provides this ability, namely, `enyo/source/dom/phonegap.js`.

You will also need to download the `"phonegap"` support package found in the [enyojs/extra](https://github.com/enyojs/extra) repo.  Then be sure to include the downloaded package in your application:

    <script src="lib/extra/phonegap/package.js" type="text/javascript"></script>

In Enyo 2, the `Signals` kind is used to listen for the `deviceready` event.  To implement this listening in our app, we first add an Enyo listener to the document body of `index.html`:

    <body>
        <script type="text/javascript">
            // tell Enyo to listen for deviceready event
            enyo.dispatcher.listen(document, "deviceready");

            new App().renderInto(document.body);
        </script>
    </body>

Note that we must use `App().renderInto(document.body)` instead of `App.write()`.

Then, in order to respond to the event we are listening for, we add an `enyo.Signals` kind to our application:

    enyo.kind({
        name: "App",
        components: [
            {kind: "Signals", ondeviceready: "deviceReady"},
            {content: "Hello, World!"}
        ],
        deviceReady: function() {
            // respond to deviceready event
		}
    });