<!---
Copyright 2015 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

## <a name="`amp-font`"></a> `amp-font`

<table>
  <tr>
    <td width="40%"><strong>Description</strong></td>
    <td>Trigger and monitor the loading of custom fonts on AMP pages.</td>
  </tr>
  <tr>
    <td width="40%"><strong>Availability</strong></td>
    <td>Stable</td>
  </tr>
  <tr>
    <td width="40%"><strong>Required Script</strong></td>
    <td><code>&lt;script async custom-element="amp-font" src="https://cdn.ampproject.org/v0/amp-font-0.1.js">&lt;/script></code></td>
  </tr>
  <tr>
    <td width="40%"><strong>Examples</strong></td>
    <td><a href="https://github.com/ampproject/amphtml/blob/master/examples/font.amp.html">font.amp.html</a></td>
  </tr>
</table>

The following lists validation errors specific to the `amp-font` tag
(see also `amp-font` in the [AMP validator specification](https://github.com/ampproject/amphtml/blob/master/validator/validator.protoascii):

<table>
  <tr>
    <th width="40%"><strong>Validation Error</strong></th>
    <th>Description</th>
  </tr>
  <tr>
    <td width="40%"><a href="/docs/reference/validation_errors.html#tag-required-by-another-tag-is-missing">TAG_REQUIRED_BY_MISSING</a></td>
    <td>Error thrown when required <code>amp-font</code> extension <code>.js</code> script tag is missing or incorrect.</td>
  </tr>
  <tr>
    <td width="40%"><a href="/docs/reference/validation_errors.html#mandatory-attribute-missing">MANDATORY_ATTR_MISSING</a></td>
    <td>Error thrown when <code>font-family</code> attribute is missing.</td>
  </tr>
  <tr>
    <td width="40%"><a href="/docs/reference/validation_errors.html#implied-layout-isnt-supported-by-amp-tag">IMPLIED_LAYOUT_INVALID</a></td>
    <td>The only supported layout type is <code>NODISPLAY</code>. Error thrown if implied layout is any other value.</td>
  </tr>
  <tr>
    <td width="40%"><a href="/docs/reference/validation_errors.html#specified-layout-isnt-supported-by-amp-tag">SPECIFIED_LAYOUT_INVALID</a></td>
    <td>The only supported layout type is <code>NODISPLAY</code>. Error thrown if specified layout is any other value.</td>
  </tr>
</table>

###Behavior

The `amp-font` extension should be used for controlling timeouts on font loading. It allows adding and removing CSS classes from `document.documentElement` based on whether a font was loaded or is in an error-state. For example:

```html
  <amp-font
      layout="nodisplay"
      font-family="My Font"
      timeout="3000"
      on-error-remove-class="my-font-loading"
      on-error-add-class="my-font-missing"></amp-font>
  <amp-font
      layout="nodisplay"
      font-family="My Other Font"
      timeout="1000"
      on-load-add-class="my-other-font-loaded"
      on-load-remove-class="my-other-font-loading"></amp-font>
```

When the extension detects the loading of a font, it appends the optional attributes `on-load-add-class` and `on-load-remove-class`; when there is any error or timeout, it appends `on-error-remove-class` and `on-error-add-class`.

Using these classes, authors can monitor whether a font is displayed to achieve the following results:

- get a short (~ 3000ms) timeout in Safari, similar to other browsers
- implement FOIT, where the page renders with no text before the font comes in
- make the timeout very short, and only use a font if it was already cached

The `amp-font` extension accepts the `layout` value `nodisplay`.

###Attributes

**font-family**

The font-family of the custom font being loaded.

**timeout** (Optional)

Time in milliseconds (default: 3000ms) after which the page does not wait for the custom font to be available. If the timeout is set to 0 then `amp-font` loads the font only if it is already in the cache; otherwise the font is not loaded. If the timeout has an invalid value then the timeout defaults to 3000ms.

**on-load-add-class** (Optional)

CSS class that is added to the `document.documentElement`  after making sure that the custom font is available for display.

**on-load-remove-class** (Optional)

CSS class that is removed from the `document.documentElement` and `document.body` after making sure that the custom font is available for display.

**on-error-add-class** (Optional)

CSS class that is added to the `document.documentElement` if the timeout interval runs out before the font becomes available for use.

**on-error-remove-class** (Optional)

CSS class that is removed from the `document.documentElement` and `document.body` if the timeout interval runs out before the font becomes available for use.

**font-weight, font-style, font-variant**

These attributes should all behave like they do on standard elements.
