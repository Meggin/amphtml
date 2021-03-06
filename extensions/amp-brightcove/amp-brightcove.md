<!---
Copyright 2015 Brightcove. All Rights Reserved.

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

### <a name="amp-brightcove"></a> `amp-brightcove`

An `amp-brightcove` component displays the Brightcove Player as used in Brightcove's [Video Cloud](https://www.brightcove.com/en/online-video-platform) or [Perform](https://www.brightcove.com/en/perform) products.

Example:
```html
<amp-brightcove
    data-account="12345"
    data-player="default"
    data-embed="default"
    data-video-id="1234"
    layout="responsive"
    width="480" 
    height="270">
</amp-brightcove>
```

The `width` and `height` attributes determine the aspect ratio of the player embedded in responsive layouts.

#### Attributes

**data-account**

The Brightcove Video Cloud or Perform account ID.

**data-player** or **data-player-id**

The Brightcove player ID. This is a GUID, shortid or "default". The default value is "default".

`data-player` is preferred. `data-player-id` is also supported for backwards-compatibility.

**data-embed**

The Brightcove player ID. This is a GUID or "default". The default value and most common value is "default".

**data-video-id**

The Video Cloud video ID. Most Video Cloud players will need this.

This is not used for Perform players by default; use it if you have added a plugin that expects a `videoId` param in the query string.

**data-playlist-id**

The Video Cloud playlist ID. For AMP HTML uses a video ID will normally be used instead. If both a playlist and a video are specified, the playlist takes precedence.

This is not used for Perform players by default; use it if you have added a plugin that expects a `playlistId` param in the query string.

**data-param-***

All `data-param-*` attributes will be added as query parameter to the player iframe src. This may be used to pass custom values through to player plugins, such as ad parameters or video ids for Perform players.

Keys and values will be URI encoded. Keys will be camel cased.

- `data-param-language="de"` becomes `&language=de`
- `data-param-custom-ad-data="key:value;key2:value2"` becomes `&customAdData=key%3Avalue%3Bkey2%3Avalue2`

#### Player configuration

The script linked below should be added to the configuration of Brightcove Players used with this component. This allows the AMP document to pause the player. Only the script need be added; no plugin name or JSON are needed.

 * http://players.brightcove.net/906043040001/plugins/postmessage_pause.js
