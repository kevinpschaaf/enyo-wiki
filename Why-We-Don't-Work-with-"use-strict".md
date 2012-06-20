Introduced with ECMAScript 5, the "use strict" pragma is a useful tool for limiting the kind of code that will run in a JavaScript VM to safer practices.  [John Resig's article on strict mode](http://ejohn.org/blog/ecmascript-5-strict-mode-json-and-more/) provides some useful background.

However, one of the language features that strict mode eliminates is the use of arguments.callee.  arguments is a special variable defined in every function that gives access to the arguments used to call the function.  This is often used to support variable argument lists.  JavaScript also provided other properties on this object, caller and callee, that are no longer allowed to be accessed in strict mode.  arguments.caller provided access to the function that called your code, while arguments.callee provided access to your own function object.

We current do our "call our superkind's implementation" via the code

    this.inherited(arguments, ...);

If you look in Enyo's Oop.js, the inherited method is implemented as

    enyo.kind.inherited = function(args, newArgs) {
        return args.callee._inherited.apply(this, newArgs || args);
    };

It looks at the "callee" property of that arguments object you passed in to it to find a special property called _inherited.  When you create a new kind, one of the magic bits we do is iterate through all the methods you've defined on the kind and add this property pointing to the same-named method on the parent kind.  If we didn't have access to arguments.callee, you'd have to change all your method definitions to have a second name, e.g.

    onSetupItem: function onSetupItem(...) {

and then the call to this.inherited would look more like

    this.inherited(onSetupItem, arguments);

making it longer to type.  That's a pretty big code change, and not one we're ready to make at this time.

If you're wondering why strict mode removes these, there are some good reasons.  One big one is that having 
access to the function that called you makes it easier to break out of your own code.  This is important 
if you have sensitive code that's calling code that you don't trust as much, so you don't want it to be able 
to inspect your state.  JS has a module pattern where you can have variables only visible to functions 
declared in the same scope, but with arguments.caller, you can pass in a callback to one of those and 
that callback can then go and look around in the closure.  arguments.callee is a problem because it lets you 
pass your own scope to other people;
[this message from es-discuss](https://mail.mozilla.org/pipermail/es-discuss/2009-March/008971.html)
summarizes the problem.