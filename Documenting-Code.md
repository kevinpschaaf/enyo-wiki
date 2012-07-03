# Documenting Code

## How to document code in Enyo

The [Enyo API Viewer](http://enyojs.com/api) enables a live view of
documentation drawn from the source code of Enyo core and its related libraries.

To enable the viewer to show documentation, a certain format must be followed.

## What is documented

The API Viewer looks at all publicly visible kinds, functions, and properties,
and displays them along with their documentation comments.

In addition, the ancestry of kinds is also shown, including inherited methods
and properties.

When contributing a new kind, be sure to include a summary of the kind, along
with documentation for all events, published properties, and public methods.

## Markdown

All documentation for the viewer is written in
[Markdown](http://daringfireball.net/projects/markdown/), a lightweight
formatting language.

Enyo uses a special commenting syntax that allows for differentiation between
comments that are to be included in documentation and those that are not.

To have a Markdown-formatted comment appear as documentation in the API Viewer,
add a `*` to the comment syntax:

- Single Line Comment

		//* This is an Enyo Documentation comment in _Markdown_.

- Multiline Comment

		/**
			This is an Enyo Documentation comment in _Markdown_.
		*/

## Visibility

There are two visibility states in Enyo, denoted by pragmas that control the
visibility of the properties they precede.

- Public
	- Default status
	- Any public code will be shown in the API Viewer.
	- Pragma: `//* @public`
- Protected
	- Hidden from the API viewer; usually, functions and properties are not meant to be modified.
	- Pragma: `//* @protected`
