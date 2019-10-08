# datalist-polyfill

[![MIT license](https://img.shields.io/npm/l/datalist-polyfill.svg 'license badge')](https://opensource.org/licenses/mit-license.php)
[![datalist-polyfill on Npmjs](https://img.shields.io/npm/v/datalist-polyfill.svg 'npm version')](https://npmjs.com/package/datalist-polyfill "datalist polyfill – on NPM")
[![Total downloads ~ Npmjs](https://img.shields.io/npm/dt/datalist-polyfill.svg 'Count of total downloads – NPM')](https://npmjs.com/package/datalist-polyfill "datalist polyfill – on NPM")
[![jsDelivr CDN downloads](https://data.jsdelivr.com/v1/package/npm/datalist-polyfill/badge 'Count of total downloads – jsDelivr')](https://www.jsdelivr.com/package/npm/datalist-polyfill 'datalist polyfill – on jsDelivr')
[![dependencies Status](https://david-dm.org/mfranzke/datalist-polyfill/status.svg 'Count of dependencies')](https://david-dm.org/mfranzke/datalist-polyfill 'datalist polyfill – on david-dm')
[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/d1f98a2d1fd44c41b7ad5c7670d8cdcd)](https://app.codacy.com/app/mfranzke/datalist-polyfill?utm_source=github.com&utm_medium=referral&utm_content=mfranzke/datalist-polyfill&utm_campaign=badger)
[![Greenkeeper badge](https://badges.greenkeeper.io/mfranzke/datalist-polyfill.svg)](https://greenkeeper.io/)

**Update:** Safari now supports the `datalist` element at least basically, as [announced earlier this year with the latest release of Safari both for iOS and MacOS X](https://developer.apple.com/documentation/safari_release_notes/safari_12_1_release_notes#3130314). Yeah !!! Exciting news!
I'm planning to release a new major version soon to both cheer as well as accommodate their implementation.

This is a minimal and dependency-free vanilla JavaScript polyfill for the awesome datalist-functionality, that will bring joy and happiness into our lives :-)

- Supports all standard's functionality as well as mimics other browsers behavior.
- Mitigating the [different levels of support](https://caniuse.com/#feat=datalist) both by Safari and IE9+ as well as EDGE
- Released under the MIT license
- Made in Germany. And supported by so many great people from all over this planet - see "Credits" accordingly.

## Features

- Lightweight: 6.9 kB of minified JavaScript, around 2.6 kB gzipped
- Fully flexible to change the datalist entries / `<option>`s
- Supporting:
  - the relevant input field types: `text`, `email`, `number`, `search`, `tel` and `url` ...
  - ... while leaving the others like color or date related, as those would most likely need another polyfill to work correctly or have a meaningful UI
  - `input[type=email]` elements `multiple` attribute
  - properties `.options` for `datalist` elements and `.list` for `input` elements
  - right to left text-direction
  - non-touch and touch interactions
  - different types of `option` declarations
  - both Safari and Internet Explorer (IE9+) browsers
  - controlling the display of differing `value` and `label` values
  - on `input[type=url]` omitting the scheme part and performing intelligent matching on the domain name
  - substring matching on both the `value` and the `text` values
- Emits "input" event when item in the `datalist` is selected
- Enables core keyboard controls such as
  - on the `input`
    - up and down arrow keys
  - on the `select`
    - `ESC`
    - `ENTER`
    - `BACKSPACE`
    - pressing printable characters
- Implements the [WAI-ARIA design pattern](https://www.w3.org/TR/wai-aria-practices/)

## Core concepts

The polyfill was designed with the following concepts kept in mind:

- dependency-free
- Supporting DOM changes by event delegation and MutationObserver
- code accessibility / readability

## Installation

Just integrate the JavaScript file into your code - et voilà.

You may optionally load via NPM or Bower:

    $ npm install datalist-polyfill
    $ bower install datalist-polyfill

## API

Nothing really, just plug it in, it ~~will~~ should work out of the box.

This package is also enabling the [`.options` (for `datalist` elements)](https://developer.mozilla.org/en/docs/Web/API/HTMLDataListElement) and [`.list` (for `input` elements)](https://developer.mozilla.org/en/docs/Web/API/HTMLInputElement) properties according to the specs.

If you set a `title`-Attribute on the `<datalist>` HTML tag, it would get used as label for the first disabled entry within the polyfilling select on non-touch interactions.

### dynamic HTML (or DHTML, if you like to be a little bit nostalgic)

In case that you'd like to dynamically add or modify / create your HTML code, you're good to go with this polyfill, as it's based on event delegation and additionally using MutationObserver (IE11+) that makes your UI work easily - no refresh nor reinit function to call after DOM manipulation or something similar.

### Changes to the available `option` elements

If you'd like to make a change to the integrated list of `<option>` elements, feel free to either remove or add them right away - the list would get generated on the fly after the user typed in something into the `<input>` field, so you're covered on this.

You can also disable `<option>` elements by adding the `disabled` attribute to the `<option>` HTML tag if necessary.

### Differing `value` and `label` values

As the browser vendors (Google Chrome vs. the others) don't seem to be aligned on this topic, I've decided to enable the `label`-attribute to serve as the definitive label being displayed, even if a value is being defined differing from the label. On different `value` and `text` values, both of them would get displayed within the suggestions, as Google Chrome does it. But if you define a differing `label`-attribute, its value would get displayed exclusively (as all the other browsers do it) to give you some flexibility on how to define those suggestions. Check out the „Different ways of defining an option“ section on the demo page regarding this topic.

### `value` property on the `option` elements for IE 10 & IE 11 and EDGE

As explained in detail below in the section ["Microsoft Internet Explorer 10 & 11 and Microsoft EDGE"](#microsoft-internet-explorer-10--11-and-microsoft-edge), for fixing missing behaviour in IE 10+ and EDGE we're manipulating the `value` for the `option` elements in those browser so you can't access them securely as a getter, but would need to take the original values out of `data-originalvalue`.

### Microsoft Internet Explorer

#### Microsoft EDGE

Microsoft EDGE doesn't trigger the `input` event any more after selecting an item via mouseclick (on `input` elements other than type of `text`), even though that IE11 still did, nevermind ...

That for the optimizations on substring matching for Microsoft EDGE specifically by #GH-39 (as explained further in the following ["Microsoft Internet Explorer 10 & 11 and Microsoft EDGE"](#microsoft-internet-explorer-10--11-and-microsoft-edge) section) need to get restricted to `input[type="text"]` elements even only.

There might be possible solutions to even also achieve the expected behaviour on non-text-input elements - even though that I only could think about ugly solutions that I don't want to have within the polyfill and that might even also break existing CSS & JS architecture / selectors.

#### Microsoft Internet Explorer 10 & 11 and Microsoft EDGE

As mentioned with #GH-63, related to aspects reported via #GH-36 and #GH-39 (and in [Microsoft EDGEs platform issues](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/9573654/)), it doesn't work in IE 10 & 11 as well as in EDGE to "Search both the value and label, using substring matching; currently it searches both the value and label, but uses prefix matching".

As requested with #GH-36 we wanted to even also enrich the experience within the "newest" IE versions (10 & 11) and EDGE browsers that provided basic support, but not the substring matching for users input. In this case the technical solution has been to manipulate the values in a way that the browser could actually handle that functionality as well, by including the users input at the beginning of the value after a substring matching to the original value, followed by a unique string for preventing any inconsistencies, followed by the original value itself, in this case for the sorting of the entries (this is mainly done in the `updateIEOptions` function around line 191 to 200 of the code).

This actually leads to a different behavior for the developers on the `value` property of each `option` elements within the `datalist` element for IE & EDGE, but on the other hand provides a better UX for IE & EDGE users by a consistent behavior for the user.

#### Microsoft Internet Explorer 9

You'll need the declaration for the standard `hidden` attribute, that you might already have included in case you're using [`normalize.css`](https://github.com/necolas/normalize.css/). Otherwise just adapt it from there:

```css
/**
 * Add the correct
 * display in IE 10-
 */

[hidden] {
	display: none;
}
```

And you need to add a nesting `select` element wrapped by a conditional comment into the `datalist` element.
Please have a look at the [demo page](https://mfranzke.github.io/datalist-polyfill/demos/ie9/) accordingly, the code is being listed at the beginning.

## Demo

See the polyfill in action either by downloading / forking this repo and have a look at `demos/index.html` and `demos/ie9/index.html`, or at the hosted demo: <https://mfranzke.github.io/datalist-polyfill/demos/> and <https://mfranzke.github.io/datalist-polyfill/demos/ie9/>

## things to keep in mind

- The demo HTML code is meant to be simple - I do know that things like a surrounding `<form>` are missing, and I've left the latin letters and english expressions for the right to left text-direction example. But lets focus on the relevant tags that this polyfill is all about for the demo.
- iOS Safari handles the `label`-attribute different from Safari on Mac OS. This is being equalized during the handling of the `label`-attributes-value for differing `value` and `label` values.
- After I thought it through and did some experiments, I've finally chosen the `<select>` element to polyfill the `<datalist>`, as it brought most of the functionality, whereas I accepted that it doesn't behave and doesn't look equally.
  - As I wanted to mainly focus on native elements in the most low level / simple way instead of visually emulating a list and than afterwards regain all of the functionality via a lot of JavaScript logic, I've ended up with this element, that knows how to play nicely with nested `<option>` elements.
  - I tried its `multiple` attribute, as this is most likely already what you're up to regarding appearance, but it does violate the form-follows-function concept and results in - surprise - the possibility for multiple selections, which isn't always `<datalist>` elements kind of thing... Then the `size` attribute came to my attention, which much better fits the requirements and behaves as designed quite perfectly.
- Let the `datalist` element be a direct follower of the `input` element - and don't nest it into the `label` in case that you're doing so with the `input` (which you nevertheless shouldn't do in general, but hey, gods great zoo is great).
- If embedding a webview within an iOS app, you should be using `WKWebView` instead of `UIWebView`, as it supports `datalist` right natively and the latter even also leads to a JavaScript error (thanks to @jscho13 for mentioning this).

## Credits

Supported by Christian, Johannes, @mitchhentges, @mertenhanisch, @ailintom, @Kravimir, @mischah, @hryamzik, @ottoville, @IceCreamYou, @wlekin, @eddr, @beebee1987, @mricherzhagen, @acespace90, @damien-git, @nexces, @Sora2455 and @jscho13. Thank you very much for that, highly appreciated !

## Tested with

- Mac
  - Mac OSX 10.13, Safari 11
  - Mac OSX 10.12, Safari 10
  - Mac OSX 10.11, Safari 9
- iOS
  - iPhone 8 Simulator, Mobile Safari 11.0
  - iPhone 7 Plus Simulator, Mobile Safari 10.0
  - iPad Pro Simulator, Mobile Safari 9.3
- Windows
  - Windows 7 SP1, Internet Explorer 9.0.8112.16421
  - Windows 8.1, Internet Explorer 11.0.9600.19101

### Big Thanks

Cross-browser testing platform provided by [CrossBrowserTesting](https://crossbrowsertesting.com)

<a href="https://crossbrowsertesting.com" title="Visit crossbrowsertesting.com for their product portfolio">
<svg id="Layer_1" data-name="Layer 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 678.67 100.75"><defs><style>.cls-1{fill:#276791;}.cls-2{fill:#f58722;}.cls-3{fill:#cc2029;}.cls-4{fill:#156e39;}.cls-5,.cls-6{fill:#2c282c;}.cls-6{font-size:27px;font-family:OpenSans, Open Sans;}.cls-7{fill:none;stroke:#000;stroke-miterlimit:10;stroke-width:0.83px;}</style></defs><title>CBT_OS-logo</title><path id="Fill-1" class="cls-1" d="M303,56.53a55.52,55.52,0,0,1-6.39-17.48c-.19-1.23-.39-2.35-.48-3.48a47,47,0,0,1-.1-6.44h0l-.39.41-.09.11-.39.4s0,.11-.1.11l-1.16,1.53A46.13,46.13,0,0,0,287.61,43c.1.3.29.71.39,1a46.9,46.9,0,0,0,5.9,10.32c.48.72,1.07,1.33,1.55,1.94l3.39.31c1.06,0,2.42,0,4.16-.1"/><path id="Fill-2" class="cls-2" d="M308.22,59.08c-1.16.21-2.22.41-3.29.51a40.77,40.77,0,0,1-6.09.11h-.1c.77.71,1.55,1.43,2.42,2.14a42.53,42.53,0,0,0,10.74,6.65c.29-.1.68-.31,1-.41A42.51,42.51,0,0,0,322.55,62c.67-.52,1.25-1.13,1.93-1.64l.29-3.58a29.92,29.92,0,0,0,0-4.29,47.44,47.44,0,0,1-16.55,6.64"/><path id="Fill-3" class="cls-3" d="M320.8,29.44a55.44,55.44,0,0,1,6.39,17.48c.2,1.23.39,2.35.49,3.48a48.44,48.44,0,0,1,.09,6.43v.11c.68-.82,1.36-1.64,2-2.56A46.07,46.07,0,0,0,336.1,43c-.1-.31-.29-.62-.39-1a47.3,47.3,0,0,0-5.9-10.32c-.49-.72-1.07-1.33-1.55-1.94l-3.39-.31a27.08,27.08,0,0,0-4.07,0"/><path id="Fill-4" class="cls-4" d="M322.64,24.13a42.53,42.53,0,0,0-10.74-6.65,9.46,9.46,0,0,1-1,.41,43.53,43.53,0,0,0-9.77,6.24c-.58.51-1.26,1-1.84,1.63L299,29.34a28.51,28.51,0,0,0,0,4.19,49.47,49.47,0,0,1,16.55-6.75c1.16-.2,2.22-.4,3.29-.51a42.22,42.22,0,0,1,6.1-.1h0c-.78-.61-1.55-1.33-2.33-2"/><path id="CrossBrowserTesting" class="cls-5" d="M361.19,55.28a10,10,0,0,0,9.23-5.75l-4.11-2.12a5.93,5.93,0,0,1-5.23,3.41c-4.11,0-7-3.29-7-7.75s2.9-7.76,7-7.76a6.2,6.2,0,0,1,5.23,3.41l4.11-2.11a10.11,10.11,0,0,0-9.23-5.76A11.65,11.65,0,0,0,349.29,43c0,7.4,5.23,12.33,11.9,12.33Zm16.24-.47V43.54a5.29,5.29,0,0,1,4-1.88,3.9,3.9,0,0,1,1.23.11V37.43a6.65,6.65,0,0,0-5.12,2.7V37.78h-4.22v17Zm15.24.47c5.34,0,8.56-4.11,8.56-8.92s-3.22-8.93-8.56-8.93-8.45,4.11-8.45,8.93,3.11,8.92,8.45,8.92Zm0-4c-2.67,0-4.12-2.23-4.12-4.93s1.45-4.94,4.12-4.94,4.11,2.23,4.11,4.94-1.44,4.93-4.11,4.93Zm17.57,4c4.45,0,7-2.35,7-5.4,0-6.81-9.57-4.82-9.57-7.28,0-.94,1-1.65,2.56-1.65A6.78,6.78,0,0,1,415,43l1.67-3.17a10.13,10.13,0,0,0-6.45-2.35c-4.23,0-6.56,2.46-6.56,5.4,0,6.7,9.56,4.47,9.56,7.17,0,1-.89,1.76-2.67,1.76A9.07,9.07,0,0,1,405,49.41l-1.78,3.29a9.92,9.92,0,0,0,7,2.58Zm15.9,0c4.45,0,7-2.35,7-5.4,0-6.81-9.56-4.82-9.56-7.28,0-.94,1-1.65,2.55-1.65a6.79,6.79,0,0,1,4.79,2l1.67-3.29a10.15,10.15,0,0,0-6.46-2.35c-4.22,0-6.56,2.47-6.56,5.4,0,6.7,9.57,4.47,9.57,7.17,0,1.06-.89,1.76-2.67,1.76a9,9,0,0,1-5.56-2.35L419,52.58a9.89,9.89,0,0,0,7.11,2.7Zm145.48-.47v-19h6.45V31.32H560.28v4.35h6.45V54.81Zm17,.47a10.13,10.13,0,0,0,6.67-2.35L593.42,50a6.43,6.43,0,0,1-4.23,1.64,4.43,4.43,0,0,1-4.67-3.76h12V46.83c0-5.53-3.22-9.28-8.12-9.28s-8.34,4-8.34,8.92a8.28,8.28,0,0,0,8.57,8.81Zm3.67-10.57h-7.9a3.89,3.89,0,0,1,3.89-3.64,3.79,3.79,0,0,1,4,3.64Zm13,10.57c4.45,0,7-2.35,7-5.4,0-6.81-9.57-4.82-9.57-7.28,0-.94,1-1.65,2.56-1.65a6.78,6.78,0,0,1,4.78,2l1.67-3.17a10.13,10.13,0,0,0-6.45-2.35c-4.23,0-6.56,2.46-6.56,5.4,0,6.7,9.56,4.47,9.56,7.17,0,1-.89,1.76-2.67,1.76a9,9,0,0,1-5.56-2.35L598.2,52.7a10.41,10.41,0,0,0,7.12,2.58Zm15.46,0a5.28,5.28,0,0,0,3.56-1.05l-.89-3.41a2.44,2.44,0,0,1-1.45.47c-.89,0-1.44-.82-1.44-1.88V41.77h3.22v-4h-3.22v-4.7h-4.23v4.7h-2.67v4h2.67v8.81c-.11,3.06,1.45,4.7,4.45,4.7Zm8-19.26a2.59,2.59,0,0,0,2.55-2.7,2.49,2.49,0,0,0-2.55-2.59,2.54,2.54,0,0,0-2.45,2.59,2.65,2.65,0,0,0,2.45,2.7Zm2.11,18.79v-17h-4.23v17Zm19.35,0v-12c0-3.29-1.67-5.4-5.23-5.4A7.58,7.58,0,0,0,639.35,40V37.78h-4.22v17h4.22V43.3a4.87,4.87,0,0,1,3.67-2c1.78,0,3,.83,3,3.18V54.81ZM661,61.74c4,0,8.67-1.52,8.67-8v-16h-4.22V40a6.16,6.16,0,0,0-5-2.58c-4.11,0-7.11,3-7.11,8.69s3.11,8.69,7.11,8.69a6.17,6.17,0,0,0,5-2.58v1.64c0,3.41-2.45,4.35-4.45,4.35a6.11,6.11,0,0,1-5-2.11l-1.89,3.17a9.69,9.69,0,0,0,6.9,2.46Zm.78-11c-2.45,0-4.12-1.76-4.12-4.7s1.67-4.7,4.12-4.7a5,5,0,0,1,3.67,1.88v5.64a4.62,4.62,0,0,1-3.67,1.88Z"/><g id="Browser"><path id="Path" class="cls-5" d="M448.39,54.81c4.23,0,6.45-2.82,6.45-6.34,0-2.94-1.89-5.4-4.23-5.76,2.12-.47,3.78-2.46,3.78-5.4,0-3.17-2.11-6-6.45-6H436.27V54.81Zm-1.45-14.09h-5.89V35.78h5.89a2.4,2.4,0,0,1,2.56,2.47,2.29,2.29,0,0,1-2.56,2.47Zm.23,9.75h-6.12V45.06h6.12a2.68,2.68,0,0,1,2.89,2.7,2.62,2.62,0,0,1-2.89,2.71Z"/><path id="Path-2" data-name="Path" class="cls-5" d="M462.62,54.81V43.54a5.4,5.4,0,0,1,4-2,2.88,2.88,0,0,1,1.11.12V37.43a6.66,6.66,0,0,0-5.12,2.7V37.78H458.4v17Z"/><path id="Path-3" data-name="Path" class="cls-5" d="M477.86,55.28c5.34,0,8.57-4.11,8.57-8.92s-3.23-8.93-8.57-8.93-8.56,4.11-8.56,8.93,3.22,8.92,8.56,8.92Zm0-4c-2.67,0-4.11-2.23-4.11-4.93s1.44-4.94,4.11-4.94S482,43.65,482,46.36s-1.45,4.93-4.12,4.93Z"/><path id="Path-4" data-name="Path" class="cls-5" d="M508.22,54.81l4.9-17h-4.45l-3,11.51-3.56-11.51h-3.78l-3.45,11.51-3-11.51h-4.45l4.89,17h4.56l3.45-11.63,3.45,11.63Z"/><path id="Path-5" data-name="Path" class="cls-5" d="M521,55.28c4.45,0,7-2.35,7-5.4,0-6.81-9.56-4.82-9.56-7.28,0-.94,1-1.65,2.55-1.65a6.79,6.79,0,0,1,4.79,2l1.67-3.17A10.15,10.15,0,0,0,521,37.43c-4.22,0-6.56,2.46-6.56,5.4,0,6.7,9.57,4.47,9.57,7.17,0,1-.89,1.76-2.67,1.76a9,9,0,0,1-5.56-2.35l-2,3.17a10.77,10.77,0,0,0,7.22,2.7Z"/><path id="Path-6" data-name="Path" class="cls-5" d="M538.7,55.28a10.11,10.11,0,0,0,6.67-2.35L543.48,50a6.43,6.43,0,0,1-4.23,1.64,4.44,4.44,0,0,1-4.67-3.76h12V46.83c0-5.53-3.23-9.28-8.12-9.28s-8.35,4-8.35,8.92c0,5.29,3.67,8.81,8.57,8.81Zm3.78-10.57h-7.9a3.82,3.82,0,0,1,3.9-3.64,3.77,3.77,0,0,1,4,3.64Z"/><path id="Path-7" data-name="Path" class="cls-5" d="M553.82,54.81V43.54a5.4,5.4,0,0,1,4-2,3.52,3.52,0,0,1,1.22.12V37.43a6.63,6.63,0,0,0-5.11,2.7V37.78h-4.23v17Z"/></g><path class="cls-5" d="M356,84.51a12,12,0,0,1-3.79-.65c-.29-.09-.38-.22-.38-.38a1.35,1.35,0,0,1,.07-.31l.49-1.47c.06-.18.15-.34.38-.34a.84.84,0,0,1,.31.07A8,8,0,0,0,356,82c.76,0,1.72-.39,1.72-1.28,0-.69-.54-1-1.1-1.29l-2.43-1.12A3.52,3.52,0,0,1,351.91,75c0-2.32,2.32-3.63,4.44-3.63a8.74,8.74,0,0,1,3.57.73c.29.12.4.25.4.41a.88.88,0,0,1-.08.27l-.56,1.42c-.09.23-.2.32-.36.32a.82.82,0,0,1-.33-.09,6.89,6.89,0,0,0-2.46-.58c-.73,0-1.54.28-1.54,1.13,0,.58.72.94,1.18,1.14l2.08.94a3.45,3.45,0,0,1,2.36,3.28C360.61,83,358.52,84.51,356,84.51Z"/><path class="cls-5" d="M374,84.26c-.47,0-.58-.24-.6-.74l-.2-6.22h0l-2.73,6.56a.58.58,0,0,1-.62.4h-.67a.66.66,0,0,1-.72-.42l-3.14-6.54h0l-.11,6.4c0,.5-.24.56-.62.56H363c-.45,0-.63-.16-.61-.58l.49-11.42c0-.43.22-.58.56-.58h1.3a.71.71,0,0,1,.75.49l4,8.19h0l3.62-8.19a.75.75,0,0,1,.75-.49h1.26c.37,0,.49.13.51.46l.6,11.56c0,.36-.13.56-.49.56Z"/><path class="cls-5" d="M389.74,84.26h-2a.64.64,0,0,1-.7-.51l-.73-2.1H381.8l-.72,2.17a.53.53,0,0,1-.54.44h-2.07a.27.27,0,0,1-.29-.29.43.43,0,0,1,0-.22l4.31-11.61a.61.61,0,0,1,.62-.46H385a.63.63,0,0,1,.65.4l4.41,11.54a1.11,1.11,0,0,1,.05.28A.34.34,0,0,1,389.74,84.26Zm-4.24-4.79-1.14-3.57q-.19-.59-.33-1.05h0c-.09.33-.2.69-.33,1.07l-1.14,3.55Z"/><path class="cls-5" d="M398.44,83.86,395.92,80H394.8v3.75a.45.45,0,0,1-.49.51h-1.89c-.36,0-.5-.16-.5-.53V72.3c0-.49.1-.62.59-.62h2.76c1.54,0,3.39.13,4.49,1.36a4.08,4.08,0,0,1,1,2.7,3.9,3.9,0,0,1-2.16,3.7v0l3,4.25a.54.54,0,0,1,.1.3c0,.17-.14.24-.28.24h-2.13A.85.85,0,0,1,398.44,83.86Zm-2.94-10h-.7v4h.9a1.9,1.9,0,0,0,2.14-2C397.84,74.36,396.86,73.86,395.5,73.86Z"/><path class="cls-5" d="M412.35,72.14l-.14,1.43c0,.36-.27.45-.65.45h-3.19v9.77c0,.4-.06.47-.42.47H405.9c-.38,0-.41-.06-.41-.44V74h-3.32c-.36,0-.51-.18-.51-.54V72.14c0-.31.17-.46.49-.46h9.75C412.26,71.68,412.39,71.86,412.35,72.14Z"/><path class="cls-5" d="M418.26,84.26h-3.62c-.44,0-.55-.18-.55-.64V72.21c0-.44.18-.53.6-.53h2.94c3.22,0,4.8.94,4.8,3.08a2.54,2.54,0,0,1-2,2.61v0a3.06,3.06,0,0,1,2.94,3.15C423.41,83.55,420.87,84.26,418.26,84.26Zm-.43-10.4h-.89v2.82h.69c1.36,0,2-.38,2-1.52C419.6,74.29,418.93,73.86,417.83,73.86Zm1.77,5a4.17,4.17,0,0,0-1.76-.25l-.9,0v3.39h1.12a4.79,4.79,0,0,0,1.49-.18,1.51,1.51,0,0,0,.94-1.47A1.55,1.55,0,0,0,419.6,78.91Z"/><path class="cls-5" d="M432.58,71.68c.36,0,.47.18.45.42l-.16,1.47c0,.38-.28.45-.71.45h-4.39v2.7h4.12c.4,0,.54.2.54.49v1.32a.46.46,0,0,1-.52.53h-4.14v2.86h4.73c.33,0,.53.15.53.49v1.34a.44.44,0,0,1-.51.51h-7c-.42,0-.6-.11-.6-.56V72.19a.45.45,0,0,1,.47-.51Z"/><path class="cls-5" d="M446,84.26h-2a.65.65,0,0,1-.71-.51l-.72-2.1H438l-.73,2.17a.53.53,0,0,1-.54.44H434.7a.27.27,0,0,1-.29-.29.56.56,0,0,1,0-.22l4.31-11.61a.62.62,0,0,1,.62-.46h1.87a.63.63,0,0,1,.65.4l4.4,11.54a.83.83,0,0,1,.06.28A.34.34,0,0,1,446,84.26Zm-4.24-4.79-1.15-3.57c-.12-.39-.23-.74-.32-1.05h0c-.09.33-.2.69-.32,1.07l-1.15,3.55Z"/><path class="cls-5" d="M454.39,83.86,451.87,80h-1.12v3.75a.45.45,0,0,1-.49.51h-1.88c-.37,0-.51-.16-.51-.53V72.3c0-.49.11-.62.6-.62h2.75c1.54,0,3.39.13,4.49,1.36a4,4,0,0,1,1,2.7,3.91,3.91,0,0,1-2.15,3.7v0l3,4.25a.55.55,0,0,1,.11.3c0,.17-.15.24-.29.24h-2.12A.85.85,0,0,1,454.39,83.86Zm-2.93-10h-.71v4h.91a1.89,1.89,0,0,0,2.13-2C453.79,74.36,452.81,73.86,451.46,73.86Z"/><text class="cls-6" transform="translate(6 52.03)">Testing Courtesy of</text><line class="cls-7" x1="266.31" y1="65.96" x2="266.31" y2="20.62"/></svg>
</a>

## Prospects & functionality overview

The following problems are mainly reported and [listed on caniuse](https://caniuse.com/#feat=datalist) as well as due to issues flagged on Github.

<table>
  <tr>
    <th>Problem</th>
    <th>IE9</th>
    <th>iOS</th>
    <th>Safari < 12.1</th>
    <th>iOS WebView</th>
    <th>Safari >= 12.1</th>
    <th>IE11+</th>
    <th>EDGE</th>
    <th>Firefox</th>
    <th>Chrome</th>
    <th>Chrome WebView</th>
  </tr>
  <tr>
    <th align="left">Basic functionality</th>
    <td colspan="3" align="center">✔ <i>Polyfill</i></td>
    <td align="center">✔ via WKWebView</td>
    <td colspan="5" align="center">✔</td>
    <td align="center"><a href="https://github.com/mfranzke/datalist-polyfill/issues/33">#GH-33</a></td>
  </tr>
  <tr>
    <th align="left"><a href="https://bugs.chromium.org/p/chromium/issues/detail?id=773041">long lists of items are unscrollable resulting in unselectable options</a></th>
    <td colspan="8" align="center">✔</td>
    <td align="center"><a href="https://bugs.chromium.org/p/chromium/issues/detail?id=773041" target="_blank">fixed with v.69</a></td>
    <td align="center">✔</td>
  </tr>
  <tr>
    <th align="left"><a href="https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/9573654/">No substring matching for the suggestions</a></th>
    <td colspan="5" align="center">✔</td>
    <td colspan="2" align="center">✔ by <a href="https://github.com/mfranzke/datalist-polyfill/issues/39">#GH-39</a></td>
    <td colspan="3" align="center">✔</td>
  </tr>
  <tr>
    <th align="left"><a href="https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/20066595/">`datalist` popups gets &quot;emptied&quot; when receiving focus via tab</a></th>
    <td colspan="6" align="center">✔</td>
    <td align="center">✔ by <a href="https://github.com/mfranzke/datalist-polyfill/issues/49">#GH-49</a></td>
    <td colspan="3" align="center">✔</td>
  </tr>
</table>

## Outro

Personally I even also do like the "keep it simple" approach provided within the [W3C specs](https://www.w3.org/TR/html5/forms.html#the-datalist-element) even already.

But on the other hand this leads to an additional visible field, but doesn't emulate the (hopefully, fingers crossed) upcoming x-browser implementation and leaves unnecessary syntax for all of the clients that wouldn't even need it (anymore).

If you're trying out and using my work, feel free to contact me and give me any feedback. I'm curious about how it's gonna be used.

And if you do like this polyfill, please consider even also having a look at the other polyfill we've developed: <https://github.com/mfranzke/loading-attribute-polyfill>
