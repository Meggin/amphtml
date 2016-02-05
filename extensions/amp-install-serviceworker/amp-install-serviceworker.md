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

## <a name="amp-install-serviceworker"></a> `amp-install-serviceworker`

The `amp-install-serviceworker` component allows installing a [ServiceWorker](http://www.html5rocks.com/en/tutorials/service-worker/introduction/) for the current page.

The idea is that the ServiceWorker runs whenever the AMP file is served from the origin where you publish the AMP file. The ServiceWorker will not be loaded when the document is loaded from an AMP cache.

See [this article](https://medium.com/@cramforce/amps-and-websites-in-the-age-of-the-service-worker-8369841dc962) for how ServiceWorkers can help improve the overall AMP experience.

### Example

```html
  <amp-install-serviceworker
      src="https://www.your-domain.com/serviceworker.js"
      layout="nodisplay"
  </amp-install-serviceworker>
```

### Behavior

The component registers the ServiceWorker given by the `src` attribute. If the current origin is different from the origin of the ServiceWorker, the component does nothing (although it emits a warning in development mode).

### Attributes

**`src`**

URL of the ServiceWorker to register.

**`layout`**

Must have the value `nodisplay`.
