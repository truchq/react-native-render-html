# v4.2.1

## Improvements

* Add typescript definitions #341 (thanks @jsamr and to everyone that helped him !)

# v4.2.0

**⚠️ `react-native-webview` is now a peer dependency**. As a result, you need to install it yourself. Please follow [the plugin's instructions](https://github.com/react-native-community/react-native-webview/blob/master/docs/Getting-Started.md).

---

- Fix deprecation warnings for componentWillReceiveProps, ...WillUpdate (thanks [@peacechen](https://github.com/peacechen))
- Fix crashes with `<iframe>` tag (thanks [@Ryabchikus](https://github.com/Ryabchikus))
- Upgraded demo app to RN 0.59.10 (fixes build issues on XCode 11) (thanks [@IjzerenHein](https://github.com/IjzerenHein))
- Use `baseFontSize` for `<a>` tags

# v4.1.1

## Important note

⚠️ **As of this release, `react-native-render-html` is now compatible with RN < 0.58 once again. Feel free to upgrade from `3.10.0` to `4.1.1` regardless of your RN version.** ⚠️

## Bugfixes

* Prevent crash when encountering css functions like `style="calc(100% - 20px);"`
* Prevent crash when encountering `normal` and `unset` keys

# v4.1.0

## Features

* Add a default renderer for `<pre>`. This lets you display preformatted text such as code inside your content with a monospace font. #196
* Add `allowFontScaling` prop. #157 (thanks @amhinson !)
* Support `srcdoc` in `<iframe>` tags. #148 (thanks @charpeni !)

![](https://i.imgur.com/SAjVPDC.png)

## Bugfixes

* Properly display an error when fetching a remote content to display has failed. #199 (thanks @Alex123456780)
* Fix the rendering of nested lists, like `<ul>` inside `<ol>` #173 (thanks @muhamad-rizki)
* Don't update `HTMLImage`'s state after it's been unmounted. This prevents warnings during development. 4fca299
* Don't strip line breaks in `<pre>` tags #196 (thanks @henry40408 !)

## Miscellaneous

* Remove useless `stream` dependency from `package.json` #190 (thanks @ramuse !)
* Fix typo in the documentation #205 (thanks @ajmeese7 !)

# v4.0.0

#### ⚠️ **This version requires react-native 0.58 and up. If you're using an older version, please stick to the version 3 of this plugin.** ⚠️ 

## Bugfixes

* Prevent crash on react-native 0.58 and above since `ViewStylePropTypes` and `ImageStylesPropTypes` have been deprecated. (thanks @ChrisEdson !)

# v3.10.0

## Features

* Add `renderersProps` prop. This lets you pass a set of props to your custom renderers, allowing you to style them furthermore without duplicating the renderers code. For instance, if you create a `blockquote` custom renderer, you can alter its color depending on the data you're rendering.

Example :

```javascript
<HTML html={...} renderers={renderers} renderersProps={{ color: 'blue' } />
<HTML html={...} renderers={renderers} renderersProps={{ color: 'red' } />

const renderers = {
    blockquote: (htmlAttribs, children, convertedCSSStyles, passProps) => {
        const { renderersProps } = passProps;
        // rendersProps : { color: blue/red }

        return ...
    }
}
```

## Rework

* The logic that applies text styling has been rewritten from scratch. The previous implementation had a lot of flaws that were hard to debug. This should be a much needed improvement addressing some of the oldest issues of this plugin. [The new algorithm is explained here](https://github.com/archriss/react-native-render-html/issues/102#issuecomment-375594792).

⚠️ *Although this shouldn't be a breaking change, your current rendering might be taking into account the previous buggy implementation. Upgrading to `3.10.0` might break some of your advanced text styling, just because it's now working as it should have from the beginning.* ⚠️ 

## Bugfixes

* Is some cases, text nodes used to be wrapped in additional `<p>` tags. This could have unintended style effects. Let's now wrap them in a new custom tag that behaves like an inline tag, but without styling : `textwrapper`

# v3.9.2

## Miscellaneous
 
* Minor fix with the npm release, no code change from the previous version.

# v3.9.1

## Features

* Handle absolute font sizes : medium, xx-small, small... (thanks @ikhsanalatsary) Closes #122
* Add `ptSize` prop (thanks @ikhsanalatsary)
* Improve `<iframe>` rendering, letting you set their dimensions through `tagsStyles` & `classesStyles`. These are overriden by `height` and `width` html attributes. Closes #110

## Bugfixes

* Addresses issues when `fontWeight` property mistakenly got converted from string to a number. This resulted in crashes due to the wrong type. (thanks @mchudy !) Closes #111
* Prevent a crash when rendering an `<img>` if you happen to have a style on your HTML container that's not an array (thanks @ikhsanalatsary) Closes #120
* Remove `!important` annotations that prevented some styles from being rendered Closes #121

## Miscellaneous

* Fix typos & `baseFontStyle` value in README (thanks @charpeni) Closes #116
* `staticContentMaxWidth` and `imagesMaxWidth` now have `Dimensions.get('window').width` as their default values

# v3.9.0

## Features

* Add `staticContentMaxWidth` prop, letting you set a maximal width for "non-responsive" renderers (only `<iframe>` for now)
* All attributes are now passed in `onLinkPress` : `evt`, `href`, `htmlAttribs` (thanks @barbogast !)
* Add `allowedStyles` prop, excluding everything but these ones (thanks @krystofcelba !)

# v3.8.1

## Bugfixes

* Properly merge `baseFontStyle` and default text styles, fixes #96 

# v3.8.0

## Features

* Add `getClosestNodeParentByTag` to the available utils functions
* Add `textSelectable` prop (thanks @hyb175 !)
> On iOS, you can copy the text, but not actually select what you want. This is a bug from react-native, see https://github.com/facebook/react-native/issues/13938
* Add default renderer for `<s>` (thanks @hyb175 !)

## Bugfixes

* The component should now re-render accordingly to your props updates ! Fixes #89, closes #83  as well as many other related issues
> This is very important, especially if you need to update your rendering
after it’s been displayed, or simply if you want to use hot reloading.
* `inherit` styles won't crash the rendering anymore, fixes #87

## Improvements

* `ignoredStyles` prop will now also remove styling passed directly through HTML attributes, see #86

## Miscellaneous

* Removed `lodash.isequal` dependency
* Rename iOS demo app with a more recognizable name

# v3.7.0

## Features

* You can now alter the `RNElements` (basically the representation of your DOM elements into native components) with the `onParsed` prop, letting you customize even more your content.

## Bugfixes

* Add missing `parentTag` to elements, fixing an important regression where your `tagsStyles` may not be applied. (thanks @Krizzu !)

## Miscellaneous

* Updated the demo with the latest version of `react-native` and its dependencies.

# v3.6.0

## New features

* Add `alterNode` prop that lets you change the values parsed from your HTML before it's rendered. It's extremely powerful as a last resort to add some very specific styling or circumvent rendering problems.
* You can now set your custom renderers as inline components. By default, your renderers will still behave as blocks.

## Fixes

* `<ul>` and `<ol>` styles aren't hardcoded anymore, you can now style them normally. (thanks @jonathonlui !)
* `<a>` tags will properly use your `ignoredStyles` prop (thanks @YeatsLu !)

# v3.5.1

* Fix : prevent crash with specific styling. This happens when the renderer is applying text-specific styles from a View wrapper to its Text children.

# v3.5.0

## New features

* Add `baseFontStyle` prop, **(replacing `baseFontSize` !)** allowing you to provide complete default styling to your text elements (#25)
* Add `listsPrefixesRenderers` prop, allowing you to customize the bullets and numbers rendered in your `<ul>` and `<ol>` lists
* Add `imagesInitialDimensions` prop
* Finished writing the base code for loading and parsing remote websites. Added a basic loader and error handlers.
* Add `remoteLoadingView` & `remoteErrorView` props
* Add `onParsed` prop, this is fired upon first rendering with the the parsing result of `htmlparser2` and of this module
* `HTMLImage`: render the `alt` attribute when images couldn't be displayed
* `HTMLImage`: `width` and `height` attribute now resize your image
* Add `debug` prop, printing the parsing result of `htmlparser2` and of this module after initial rendering

## Fixes

* Make `classesStyles` take precedence over `tagsStyles` (#35)
* Greatly improve text styling nested inside views
* In some cases, raw texts children weren't wrapped with their texts siblings, so their styling wouldn't apply properly
* Title tags like `<h1>`, `<h2>` and so on will always break line between each others
* `HTMLImage`: don't overscale images when `imagesMaxWidth` prop is set to a higher value than the original width of your images
* Correct some edge cases where random line breaks would randomly happen
* Properly render raw texts nested inside `<a>` tags
* `tagsStyles` is now applied `_constructStyles` so your custom renderers have proper styling

# v3.4.0

## New features

* Add `alterData` & `alterChildren` props, allowing you to change your HTML data before the intial rendering !

## Fixes

* Empty lists won't crash (thanks @peacechen !)
* `baseFontSize` prop won't override the `fontSize` set by either : the `style` attribute, the `tagsStyles` and `classesStyles` prop

# v3.3.0

## New features

* Add `customWrapper` prop

## Fixes

* Use `ViewPropTypes` to get rid of the deprecation warning

# v3.2.0

## New features

* Add `baseFontSize` prop so you can change the size of all your texts in a single prop without having to style every text tag (thanks @peacechen !)

## Fixes

* Texts elements that are siblings of `<br>` tags should receive the styling of their parent properly
* Line breaks in your HTML won't actually render line breaks in your native components, for instance :

```html
<p><b>Description</b><br>Some description...<br />
Item 1,
Item 2,
Item 3,
</p>
```

Item 1, 2, and 3 will be on the same line, regardless of the line breaks of the snippet.

# v3.1.0

## New features

* Add `decodeEntities` prop (`true` by default)

# v3.0.0

This is a major overhaul of the whole module, adding a lot of features, fixing numerous bugs, and... breaking some things. The whole codebase has been refactored and cleaned-up. The javascript style and linters have been updated accordingly.

## Demo

This component now comes with a demo that showcases every feature. It will be very useful to keep track of bugs and rendering differences between the different versions of react-native.

It will be mandatory to refer to an example of the demo or to provide one when submitting an issue or a pull request for a new feature.

## New features

* The whole parsing and rendering logic has been written from scratch. **It shoud now be able to render anything you throw at it without breaking a sweat** *(we mean without crashing)*
* Greatly improve & fix the default renderer of images, with support for broken links in a browser-like way
* Add support for `em` sizing !
* Add support for percentage sizing !
* Add `ignoredTags` prop, you're now able to get rid of those nasty `<script> or <blink>` (seriously ?) more easily
* Add `ignoredStyles` prop. Want to make sure no `background-color` is applied through the `style` attribute ? You got it
* Add more parameters to your custom renderers, like the CSS styling that has been converted
* (experimental), use a remote website as the source of parsed HTML !
* Add a default renderer for `<iframes>` & `<i>` tags
* Add `classesStyles` prop to style your components with their respective HTML classes
* Add `ignoreNodesFunction` prop so you can ignore HTML tags very precisely

## Fixes

* **You shouldn't encounter random line breaks in your texts** when using tags like `<em>`, `<i>`, `<strong>`... even if they're not wrapped in a parent like `<p>`
* Properly re-render everything if the HTML source  changes
* Images nested inside `<a>` should behave and receive taps properly
* Don't crash when tapping an `<a>` tag without `href` attribute
* Prevent the manual prop-checking handling the CSS to RN conversions from drowning you with YellowBoxes

# v2.1.0

Courtesy @duyphambhtech :)

* Remove `shallowCompare` and replace with `PureComponent`
* Fix crash when `<img>` tag has no src attribute
* Render `<br>` tag
* Render `<sub>` and `<sup>`
* Fix import `PropTypes` from `prop-types`
* Convert code to conform to Javascript Standard Style
* Fix crash calling `PropTypes` validators directly

# v2.0.0

Compatibility with react-native 0.43.2+ (react 16)

# v1.0.0

Initial release, usable with react >= 0.20 <= 0.42.2 (react 15)
