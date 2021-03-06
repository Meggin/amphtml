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

## <a name="amp-facebook"></a> `amp-facebook`

Displays a Facebook Post or Video. 

###Examples

####Embedding a post:
```html
<amp-facebook width=486 height=657
    layout="responsive"
    data-href="https://www.facebook.com/zuck/posts/10102593740125791">
</amp-facebook>
```

####Embedding a video:
```html
<amp-facebook width=552 height=574
    layout="responsive"
    data-embed-as="video"
    data-href="https://www.facebook.com/zuck/videos/10102509264909801/">
</amp-facebook>
```


### Attributes

**data-href**

The URL of the facebook post/video. For example, `https://www.facebook.com/zuck/posts/10102593740125791`.

**data-embed-as** (Optional)

Either `post` or `video`; default is `post`.

Both posts and videos can be embedded as a post. Setting `data-embed-as="video"` for Facebook videos only embeds the video player, ignoring the accompanying post card with it. This is recommended if you need better aspect ratio management for the video to be responsive.  

See the documentation for differences between [post embeds](https://developers.facebook.com/docs/plugins/embedded-posts) and [video embeds](https://developers.facebook.com/docs/plugins/embedded-video-player).
