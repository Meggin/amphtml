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

## <a name="amp-dynamic-css-classes"></a> AMP Dynamic CSS Classes

The AMP Dynamic CSS Classes extension adds several dynamic CSS class names onto the HTML element.

### Behavior

The AMP Dynamic CSS Classes extension adds the following CSS classes onto the HTML element:

**amp-referrer-***

One or more referrer classes are set, one for each level of subdomain specificity. For example, `www.google.com` adds three
classes: `amp-referrer-www-google-com`, `amp-referrer-google-com`, and `amp-referrer-com`.

There are a few special cases:

- When the user arrives through a Twitter `t.co` short link, `twitter.com` is used as the referrer.
- When the string "Pinterest" is present in the User Agent string and there is no referrer, `www.pinterest.com` is used as the referrer.

**amp-viewer**

The `amp-viewer` class is set if the current document is being displayed inside a Viewer.
