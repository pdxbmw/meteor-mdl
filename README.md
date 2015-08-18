Material Design Lite wrapped for Meteor
==============================================================================
Contains a custom build of `material.js` having `componentHandler` available globally.

> Material Design Lite lets you add a Material Design look and feel to your websites. It doesn’t rely on any JavaScript frameworks and aims to optimize for cross-device use, gracefully degrade in older browsers, and offer an experience that is immediately accessible.
> - [getmdl.io](http://www.getmdl.io/index.html)

[Visit MDL's website](http://www.getmdl.io/started/index.html) to learn how to get started with MDL.

[![Build Status](https://travis-ci.org/Zodiase/meteor-mdl.svg?branch=master)](https://travis-ci.org/Zodiase/meteor-mdl)

Looks like some other people also wrapped MDL, why use this one?
------------------------------------------------------------------------------
Because the ones I tried don't work. I'm trying to use MDL in some of my Meteor projects and there are issues that won't be fixed by just adding `material.js`.

What I've experienced so far:

* Can't export `componentHandler` with the official build since it's defined with the keyword `var`. To really ***use*** MDL, you need `componentHandler`.
* When using Iron:Router, template rendering happens ***after*** `document.load` event, so MDL upgrades nothing by itself.
* Even when not using Iron:Router, it's lucky that `document.load` works; when any MDL components get re-rendered by Blaze, they are screwed again. Because MDL won't automatically re-scan DOM for potential upgrades. Check out my [blaze demo](https://github.com/Zodiase/meteor-mdl/tree/master/demo/blaze) without using this package to see how bad it is.

Features
------------------------------------------------------------------------------
* Exports `componentHandler` globally (Client Only).
* Exports `Mdl` globally for changing settings in runtime.
	* If `MDl.envConfig['blazeFix'] === true` then the Blaze patch is effective. And only in this case:
		* `MDl.envConfig.patchers.blaze.getUpgradeStyle()` returns the current auto-upgrading style.
		* `MDl.envConfig.patchers.blaze.setUpgradeStyle('fullUpgrade' | 'mutationOnly' | 'none')` sets the auto-upgrade style.
			* `'fullUpgrade'` uses `componentHandler.upgradeAllRegistered()` when any mutation is observed.
			* `'mutationOnly'` uses `componentHandler.upgradeElements(mutation.target)` on mutations when any mutation is observed.
			* `'none'` does nothing.
	* If `MDl.envConfig['ironRouterFix'] === true` then the Iron:Router patch is effective.
		* This only happens when the blaze patch is not available.
		* This is unfortunately not a perfect fix.
* Supports [Iron:Router](https://github.com/iron-meteor/iron-router).*

Install
------------------------------------------------------------------------------
```Bash
$ meteor add zodiase:mdl
```

Known Issues
------------------------------------------------------------------------------
* ***Do not use MDL components at the top level of any templates or template event handlers may not work correctly.***

Change Log
------------------------------------------------------------------------------
* v1.0.3
  * Updated MDl to v1.0.3.
* v1.0.2-2
  * Improved support for webkit browsers.
  * Added TrueType font for Safari.
  * Fixed blaze patcher for Safari.
* v1.0.2-1
  * Included Material Icons Typeface file in the package so your app can go completely offline.
* v1.0.2
	* Updated MDl to v1.0.2.
	* Converted demos into integration tests.
* v1.0.1-1
	* Added `material-grid.css`.
* v1.0.1
	* Updated MDl to v1.0.1.
	* Minor performance tweaks.
* v1.0.0-4
	* Updated `material.js` with another custom build, adding the ability to do `componentHandler.upgradeAllRegistered(rootNode)`. Check out [my MDL fork](https://github.com/Zodiase/material-design-lite) for details.
	* Added a namespace `MDl` while maintaining `componentHandler` available globally. You can also use `Mdl.componentHandler` if it looks better.
	* `MDl.envConfig` stores runtime status generated by the package and public APIs for extensive features (to be added).
* v1.0.0-3
	* Slightly improved blaze patcher.
* v1.0.0-2
	* Fixed a bug causing build to fail. 
* v1.0.0-1
    * Started using `MutationObserver` to detect changes in DOM and then call MDL's `componentHandler.upgradeAllRegistered()`.
    * If `MutationObserver` is not available, falls back to the old (buggy) approach.

Content
------------------------------------------------------------------------------
| File                | Description                                     |
| ------------------- | ----------------------------------------------- |
| package.js          | package descriptions.                           |
| material.css        | css file built straight from MDL.               |
| material-icons.css  | css file built straight from MDL.               |
| material.js         | custom built for exporting componentHandler.    |
| envConfigs.js       | defines global vars shared among other scripts. |
| export.js           | used for exporting componentHandler.            |
| patchers/*          | helpers for adding more MDL's auto-upgrades.    |
| tests/*             | unit tests.                                     |
| integration-tests/* | integration-tests/demos.                        |

Versioning
------------------------------------------------------------------------------
The version of this package matches MDL's version to make it easy to perceive what version of MDL it contains.
In addition, it has a fourth version number indicating revisions not related to MDL.

License
------------------------------------------------------------------------------
([MDL source](https://github.com/google/material-design-lite) is owned by Google and licensed under an Apache-2 license.)

© Xingchen Hong, 2015. Licensed under an Apache-2 license.


------------------------------------------------------------------------------
\* See [issues](https://github.com/Zodiase/meteor-mdl/issues) for more detail.
