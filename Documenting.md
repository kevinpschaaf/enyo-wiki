## How to document code in Enyo

The [Enyo API viewer](http://enyojs.com/api) enables a live view of documentation from the source code of Enyo, and Enyo libraries.

To enable the viewer to show documentation, a certain format must be followed.

## What is documented

The API Viewer looks at all publicly visible kinds, functions, and properties, and displays them with their documentation comments.

In addition, the ancestry of kinds is also shown, including inherited methods, and properties.

## Markdown

All documentation for the viewer is written in [Markdown](http://daringfireball.net/projects/markdown/), a lightweight formatting language.

Enyo uses a special commenting syntax that allows for differentiation between documentation comments and not.

To enable the document viewer to use a Markdown formatted comment as documentation, a `*` is added to the comment syntax.

- Single Line Comment

		//* This is an Enyo Documentation comment in _Markdown_

- Multiline Comment

		/**
			This is an Enyo Documentation comment in _Markdown_
		*/

## Visibility

There are two visibility states in Enyo, denoted by pragmas, and they signal the visibility of the subsequent properties.

- Public
	- Default status
	- Any public code will be shown in the API Viewer
	- Pragma: `//* @public`
- Protected
	- Hidden from the API viewer, and usually functions and properties not meant to be modified.
	- Pragma: `//* @protected`
