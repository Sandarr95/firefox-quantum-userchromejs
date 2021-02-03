# Firefox Quantum compatible userChrome.js

## What does this do?

Extensions that allow you to run arbitrary javascript in your browser context don't work anymore with Firefox 57 and later. This is a workaround which allows you to run arbitrary javascript in your browser context, as used to be enabled by the extension [userChromeJS](http://userchromejs.mozdev.org/).

It does not involve adding any extensions, and instead works only by adding (or changing) files in your Firefox user profile.

After I made this I learned of something called `autoconfig.js` which apparently serves a similar purpose; I haven't investigated it very much.

## Installation

Place the three `userChrome.*` files in a `/chrome` directory inside your Firefox profile. If you already have a `userChrome.css` file, you may instead add the contents of the file here anywhere in your existing file.

Replace the contents of `userChrome.js` with whatever you wish; it will execute in the browser context whenever you open a new browser window. The existing contents of that script (in this repo) are a small change to the fullscreen behavior of Firefox under macOS: it restores the pre-Lion behavior (and hides the toolbar and tabs when in fullscreen mode).

You may also place `userChrome.xul` and `.uc.js`, `.uc.xul` and `.css` files in the `/chrome` directory, these will be loaded along with `userChrome.js`. 
 - `.uc.js` files will execute javascript in the browser context, like `userChrome.js`. 
 - `.css` files will loaded as user sheets into all pages, including the ui (chrome:// urls), a sort of userChrome/userContent hybrid. Use `@-moz-document` rules to limit them to certain pages. (userChrome.css and userContent.css will behave as normal).
 - A special case, `.as.css` files, are loaded as agent sheets, allowing you to style anonymous content like scrollbars.
- `userChrome.xul` and `.uc.xul` files are [XUL](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XUL) overlays that will load into the browser. (Note: Firefox 61 [disabled](https://bugzilla.mozilla.org/show_bug.cgi?id=1448162) XUL overlays and Firefox 63 completely [removed](https://bugzilla.mozilla.org/show_bug.cgi?id=1449791) support for them).
 
To uninstall, remove the three files. If you have other content in the `userChrome.css` file you can remove just the part that you added during installation.

## Why did I make it?

I wanted to enable pre-Lion osx fullscreen mode, and couldn't find an easy way to do it in Firefox Quantum. It's possible with unpacking, altering, and repacking files in the `Firefox.app` package... but that's painful and might not survive updates. Then I came upon this method, realized this trick was far more general than just altering fullscreen mode, and factored it out so that the javascript part was easily modifiable by anyone.

## How does it work?

It relies on the fact that post-57 Firefox still allows a custom `userChrome.css` file, and a Firefox-specific CSS hack which can bind javascript to arbitrary DOM elements. I picked (somewhat at random) a DOM element in the browser whose existing XBL binding didn't already have a `<constructor>` tag, and added some JS there to load an external javascript file.

It's very possible that at some future time the Firefox team will remove some or all of the functionality that makes this possible, so enjoy it while you can.
