# Style Guide

We ask that developers respect the following guidelines when contributing pull requests to Enyo.

## Indentation

The Enyo source code uses leading tabs for indentation of code.

## Blocks

First curly brace at end of line, final curly brace on a line by itself:

    for (var i=0; i<100; i++) { // first curly brace at end of line
        ...
    } // final curly brace on a line by itself

Always use curly braces:

    // bad
    if (x) foo(); 

    // good
    if (x) {
        foo();
    }

Else clauses have the `else` keyword between the braces:

    if (x) {
        ...
    } else if (y) {
        ...
    } else {
        ...
    }

## Punctuation

We use a like-English rule whenever possible. For example, no spaces on the inside of associations, and one space after a colon:

	var x = (x + 7) * 3;
	var y = {neft: "zap"};
	var z = [1, 2, 3, (4 + 5)];

Single space after any keyword, except `function`:

	for (var i=0; i<100; i++) {
		...
	}

	while (x = 3) {
		... 
	}

	x = function() {
		...
	}