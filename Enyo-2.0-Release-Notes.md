# Enyo 2.0 Release Notes

Welcome to Enyo 2.0.  The following items have changed since the 2.0 beta 5 release.

## Sampler

* A new Sampler app has been added, with its own new repository.

## Enyo

* In `enyo.platform`, added a regex to detect desktop versions of Safari.

* In `enyo.RichText`, fixed issue causing RichText objects to not be recognized as text fields.

* Changed `getSelected()` to `getValue()` in the code sample from the opening comments in `enyo.Select`.

* In `enyo.TouchScrollStrategy`, removed trailing comma from overscroll data.

* In `enyo.UiComponent`, blocked resize events for hidden UiComponents.

* In `enyo.WebService`, changed name of `callback` property to `callbackName` to match the name expected by JsonpRequest.

* In the Playground sample, a missing style has been added to `Playground.css`.

* In ScrollerSample, the Picker now uses `onSelect` instead of `onChange`.

## Layout

* In `enyo.Arranger`, fixed several issues relating to positioning and visibility.  Also implemented `destroy()` for each of the Arranger kinds, so they leave child controls in a reasonable state when changing arrangers.

* In FittableSample, fixed layout issues on iOS.

* Added new `FittableTests` sample.

* In `enyo.List`, fixed issue that could cause lists in nested panels to become unscrollable.

* Fixed issue causing List-Flickr to not load on IE9.

* In `enyo.Node`, added cross-platform CSS3 compatibility and an optional `onlyIconExpands` property.

* In `enyo.Panels`, blocked resize events for hidden panels.  Also eliminated visual artifacts (flashing) when deleting a panel.

* Fixed titles in panelMatters.

* In `PanelsSample.html`, added scroller to Arrangers menu to fix sizing issue in Safari on iOS; also fixed associated CSS file.

* Fixed titles in `PanelsSample.js`.  Also set arranger scroller size depending on media query (set max-height for smaller screens, allow natural sizing for larger screens). 

* In `enyo.PulldownList`, made Puller dynamically reference the list being pulled, allowing you to use multiple PulldownLists.

## Tools

* Fixed the `minify.js` build script for Node 0.8.2.

* In `AjaxTest.js`, fixed testCors bad response in Firefox by using YQL API call instead of YouTube API call.

## Documentation

The documentation available in the API Viewer has been reviewed, and significant additions have been made.