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

## <a name="amp-mustache"></a> `amp-mustache`

The `amp-mustache` component allows rendering of [Mustache.js](https://github.com/janl/mustache.js/) templates.

### Syntax

Mustache is a logic-less template syntax. See the [Mustache.js docs](https://github.com/janl/mustache.js/) for more details. Some of the core Mustache tags are:

 - `{{variable}}`: variable tag. It outputs the the HTML-escaped value of a variable.
 - `{{#section}}{{/section}}`: section tag. It can test the existence of a variable and iterate over it if it is an array.
 - `{{^section}}{{/section}}`: inverted tag. It can test the non-existence of a variable.

### Usage

The `amp-mustache` template must be defined and used according to the [AMP Template Spec](../../spec/amp-html-templates.md).

First, the `amp-mustache` is declared/loaded:

```html
<script async custom-template="amp-mustache" src="https://cdn.ampproject.org/v0/amp-mustache-0.1.js">
</script>
```

Then, the Mustache templates can be defined in `template` tags:

```html
<template type="amp-mustache">
  Hello {{world}}!
</template>
```

How templates are discovered, when they are rendered, and how data is provided are all determined by the target AMP element that uses the template to render its content.

### Restrictions

Like all AMP templates, `amp-mustache` templates are required to be well-formed DOM fragments. This means that, among other things, you cannot use `amp-mustache` to:

 - Calculate a tag name; e.g., `<{{tagName}}>` is not allowed.
 - Calculate an attribute name; e.g., `<div {{attrName}}=something>` is not allowed.
 - Output arbitrary HTML using `{{{unescaped}}}`; the output of "triple-mustache" is sanitized to only allow formatting tags such as `<b>`, `<i>`, and so on.

Note also that, because the body of the template must be specified within the `template` element, it is impossible to specify `{{&var}}` expressions; they will always be escaped as `{{&amp;var}}`. The triple-mustache `{{{var}}}` must be used for these cases.
