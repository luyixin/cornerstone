# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [1.1.3] - 2017-11-17
### Added
- Exporting of triggerEvent function, mainly for use by image loader libraries

### Changed
- Switched rescaleImage to use image columnPixelSpacing instead of relying on metadataProvider (thanks @adreyfus)
- Switched this changelog to try to follow http://keepachangelog.com/en/1.0.0/


## Version 1.1.2

- Changed the CustomEvent polyfill check to use typeof. It was not working on IE.

## Version 1.1.1

- Added the CustomEvent polyfill for browsers which doesn't support it.
- Fixed lint issues that were being displayed during tests.

## Version 1.1.0

- Major changes:

In 1.1.0 we have added several rendering functions to support the use of layers for composite images.

- *renderPseudoColorImage*: This renders a grayscale image as a color image using a color lookup table after first applying the modality and VOI LUT transformations. This is the described approach for pseudo-color image display in the DICOM standard (http://dicom.nema.org/medical/dicom/current/output/chtml/part04/sect_N.2.html).

Previously, users had to use convertToFalseColorImage which converted the entire grayscale image to a color image, and then rendered the RGB pixels. Now, you can set the 'colormap' property on the viewport and the image will be displayed in false color. The False Color Mapping and Composite Images examples have been updated accordingly.

- *renderLabelMapImage*: In medical imaging it is very common to have results stored as label maps. Pixel values are set to arbitrary numbers which represent something specific in an image (e.g. 1 for heart, 2 for lung). These label maps do not require any modality LUT or VOI LUT transformations and can just be mapped to RGBA pixels through color lookup tables. Support for label maps has now (finally!) been added to Cornerstone. Just set the viewport property 'labelmap' to true and add a colormap and you can display a label map.

- *renderGrayscaleImage*: Now has support for rendering using either just the alpha channel or RGBA channels. This is required for proper layer support.
- Removed redundant code in renderWebImage which was being run before renderColorImage was being called. This should hopefully fix performance issues for Web images that have been reported (#164).
- More work towards dropping jQuery from @maistho: Trigger all jQuery events inside triggerEvent function (#185)
- Minor code cleanup, converted LookupTable to use ES6 Class syntax

## Version 1.0.1

- Switch package.json 'main' to minified version to reduce bundle sizes
- jQuery removed from tests (thanks @maistho)

## Version 1.0.0

- Updated to 1.0.0 because 0.13.1 introduced a breaking change with jQuery injection. This doesn't break usage if you are using HTML script tags, but if you are using Cornerstone in a module system, Cornerstone may not properly find jQuery.

The solution for this is to inject your jQuery instance into Cornerstone as follows:

````javascript
cornerstone.external.$ = $;
````

An example commit doing this in the OHIF Viewer Meteor application is here: https://github.com/OHIF/Viewers/commit/012bba44806d0fb9bb60af329c4875e7f6b751e0#diff-d9ccd906dfc48b4589d720766fe14715R25

We apologize for any headaches that the breaking change 0.13.1 may have caused for those using module systems.

## Version 0.13.2 (deprecated due to breaking change)

- Added native CustomEvents that are triggered parallel to the jQuery events. This is part of a transition to drop the jQuery dependency entirely.
- Added the EventTarget interface to cornerstone.events. You can now use cornerstone.events.addEventListener to listen to events. The parallel events have the same names as the current events, but are all lower case.

e.g. CornerstoneImageRendered has a native CustomEvent name 'cornerstoneimagerendered'

## Version 0.13.1 (deprecated due to breaking change)

- Updated dependencies
- Added JQuery as an injection dependency
- Properly reexport reference to isWebGLInitialized from WebGL module

## Version 0.13.0

- Removed the all `always` from imageCache's method decache to better stability
- Stop putImagePromise() if the image has been purged before being loaded
- Changed Webpack building file to output files as `cornerstone-core` for commonjs, commonjs2 and amd
- Add jquery names for commonjs, commonjs2 and amd
- Updated to Webpack 3
- Bug fixes for drawing composite images
- Bug fix for missing intercept value after false color mapping
- Math.floor instead of Math.round, to have correct values when v < Range[0] or v > Range[1] for linearIndexLookupMain
- Avoid setting canva's size with the same value, because it flashes the canvas with IE and Edge
- Remove parseInt to convert the values of pixelData
- Updated generateLut to generateLutNew
- drawCompositeImage synchronizes viewport.hflip
- Also synchronize vflip
- Trigger CornerstoneImageCacheMaximumSizeChanged and CornerstoneImageCacheChanged events when changing conerstone image cache
- Force cornerstone to generate a new LUT and use the color renderer after applying false color mapping

## Version 0.12.2

- Fix VOILUT rendering for Agfa images (issue #106)

## Version 0.12.1

- Fix NPM dependencies, added this changelog
- Fix viewport sync in composite image example

## Version 0.12.0

- Add layer API support for drawing composite images
- Remove globals from eslintrc.js
- Fix broken event firing for WebGL texture cache

## Version 0.11.1

- Add a 'CornerstonePreRender' event that is fire before the image is rendered
- Switch module imports to include '.js' extensions to improve support for Chrome native modules

## Version 0.11.0

- Switch events to fire on cornerstone.events instead of cornerstone. This was broken when Cornerstone was loaded as a module.
  Note: If you are currently using $(cornerstone).on(...) to monitor image load progress or cache changes, you need to change this to
  $(cornerstone.events).on(...)

- Add previous image information in CornerstoneNewImage event
- Fix build issues on Windows
- Fix missing devDependency for Istanbul in package.json
- Add a number of unit tests, ESLint fixes, and JSDoc comments

## Version 0.10.10

- Fix issues with package.json and Webpack configuration

## Version 0.10.9

- Migrate to new build process with Webpack and Babel. Remove grunt.
- Migrate to ES6 modules (@brunoalvesdefaria, @zachasme, @lscoder, @jpambrun, @jasonklotzer)
- Fix issue #115: Grayscale not inverting properly
- Fix typo in Date.now for performance timing fallback when window.performance is not available

## Version 0.10.8

- Performance improvements for LUT generation (@jpambrun)

## Version 0.10.7

- Revert fix for #101 since it's reducing performance
