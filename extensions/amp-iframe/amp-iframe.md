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

## <a name="amp-iframe"></a> `amp-iframe`

Displays an iframe.

The `amp-iframe` extension has several important differences from standard iframes, designed to make it more secure and avoid AMP files that are dominated by a single iframe:

 - `amp-iframe` may not appear close to the top of the document (except for iframes that use `placeholder`, described below). They must be either 600px away from the top or not within the first 75% of the viewport when scrolled to the top, whichever is smaller. **Note:** We are currently looking for feedback as to how well this restriction works in practice.
 - They are sandboxed by default (see below).
 - They must only request resources via HTTPS, from a data-URI, or via the `srcdoc` attribute.
 - They must not be in the same origin as the container unless they do not use `allow-same-origin` in the sandbox attribute. See the [iframe origin policy](../../spec/amp-iframe-origin-policy.md) document for further details on allowed origins for iframes.

Example:
```html
<amp-iframe width=300 height=300
    sandbox="allow-scripts allow-same-origin"
    layout="responsive"
    frameborder="0"
    src="https://foo.com/iframe">
</amp-iframe>
```

### Attributes

**src, srcdoc, frameborder, allowfullscreen, allowtransparency**

These attributes should behave like they do on standard iframes.

**sandbox**

Iframes created by `amp-iframe` always have the `sandbox` attribute defined on them. By default the value is empty, meaning that they are "maximum sandboxed". By using other sandbox values, you can set the iframe to be less sandboxed. All values supported by browsers are allowed. For example, setting `sandbox="allow-scripts"` allows the iframe to run JavaScript, while `sandbox="allow-scripts allow-same-origin"` allows the iframe to run JavaScript, make non-CORS XHRs, and read/write cookies.

If you are iframing a document that was not specifically created with sandboxing in mind, you will most likely need to add `allow-scripts allow-same-origin` to the `sandbox` attribute.

Note also that the sandbox applies to all windows opened from a sandboxed iframe. This includes new windows created by a link with `target=_blank` (add `allow-popups` to allow this to happen). Adding `allow-popups-to-escape-sandbox` to the `sandbox` attribute makes the new windows behave like non-sandboxed new windows. This is what you want and expect most of the time. However, as of this writing, `allow-popups-to-escape-sandbox` is only supported by Chrome.

See the [sandbox docs on MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#attr-sandbox) for further details on the sandbox attribute.

### Iframe Resizing

An `amp-iframe` must have static layout defined as any other AMP element. However, it is possible to resize an `amp-iframe` in runtime. To do so:

 1. The `amp-iframe` must be defined with the `resizable` attribute.
 2. The `amp-iframe` must have an `overflow` child element.
 3. The iframe document must send an `embed-size` request as a window message.

Note that the `resizable` attribute forces the `scrolling` attribute value to `no`.

Example of `amp-iframe` with `overflow` element:
```html
<amp-iframe width=300 height=300
    layout="responsive"
    sandbox="allow-scripts allow-same-origin"
    resizable
    src="https://foo.com/iframe">
  <div overflow tabindex=0 role=button aria-label="Read more">Read more!</div>
</amp-iframe>
```

Example of iframe resize request:
```javascript
window.parent.postMessage({
  sentinel: 'amp',
  type: 'embed-size',
  height: document.body.scrollHeight
}, '*');
```

When this message is received, the AMP runtime will try to accommodate it as soon as possible, but it will take into account where the reader is on the page, whether the scrolling is ongoing, and any other UX or performance factors. If the runtime cannot satisfy the resize request, the `amp-iframe` will show an `overflow` element. Clicking the `overflow` element  immediately resizes the `amp-iframe` because it is triggered by a user action.

Here are some factors that affect how fast the resize will be executed:

 - Whether the resize is triggered by a user action
 - Whether the resize is requested for a currently active iframe
 - Whether the resize is requested for an iframe below or above the viewport

### Iframe with Placeholder
It is possible to have an `amp-iframe` appear on the top of a document when the `amp-iframe` has a `placeholder` element:

```html
<amp-iframe width=300 height=300
   layout="responsive"
   sandbox="allow-scripts allow-same-origin"
   src="https://foo.com/iframe">
 <amp-img layout="fill" src="https://foo.com/foo.png" placeholder></amp-img>
</amp-iframe>
```

 - The `amp-iframe` must contain an element with the `placeholder` attribute (such as an `amp-img` element) which would be rendered as a placeholder until the iframe is ready to be displayed.
 - Iframe readiness can be determined by listening to the iframe's `onload` event or an `embed-ready` postMessage which would be sent by the iframe document, whichever comes first.

Example of iframe embed-ready request:
```javascript
window.parent.postMessage({
  sentinel: 'amp',
  type: 'embed-ready'
}, '*');
```

### Iframe Viewability

An iframe can send a `send-intersection` message to its parent to start receiving IntersectionObserver style [change records](http://rawgit.com/slightlyoff/IntersectionObserver/master/index.html#intersectionobserverentry) of the iframe's intersection with the parent viewport.

Example of iframe `send-intersection` request:
```javascript
window.parent.postMessage({
  sentinel: 'amp',
  type: 'send-intersection'
}, '*');
```

The iframe can listen to an `intersection` message from the parent window to receive the intersection data.

Example of iframe listening for an `intersection` message:
```javascript
window.addEventListener('message', function(event) {
  const listener = function(event) {
    if (event.source != window.parent ||
        event.origin != window.context.location.origin ||
        !event.data ||
        event.data.sentinel != 'amp' ||
        event.data.type != 'intersection') {
      return;
    }
    event.data.changes.forEach(function (change) {
      console.log(change);
    });
});
```

The intersection message would be sent by the parent to the iframe when the iframe moves in or out of the viewport (or is partially visibile) when the iframe is scrolled or resized.
