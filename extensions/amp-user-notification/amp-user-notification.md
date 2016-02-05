<!--
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

## <a name="amp-user-notification"></a> `amp-user-notification`

Displays a dismissable notification to the user. 

### Usage
By supplying two URLs &mdash; one called before the notification is shown and one called after it is dismissed &mdash; it is possible to control per user (using the `ampUserId` value) whether the notification should be shown. For example, a notification might only be shown to users in certain geolocations, or you might prevent showing it again to a user who has dismissed it before.

The two URLs are provided by the `data-show-if-href` and `data-dismiss-href` attributes. An `id` is required, as multiple `amp-user-notification` elements are allowed and the `id` is used to differentiate them.

To close `amp-user-notification`, add an `on` attribute to a button with the following value scheme: `on="event:idOfUserNotificationElement.dismiss"` (see example below). This user action also triggers the `GET` to the `data-dismiss-href` URL. Be mindful of the browser caching the `GET` response; see details below. We recommend adding a unique value to the `GET` URL, such as a timestamp, as a query string field.

When multiple `amp-user-notification` elements are on a page, only one is shown at a time; when one is dismissed the next one is shown. The order in which notifications are shown is currently not deterministic. (TODO: follow up task #1229 to fix this).

Example:

```html
<amp-user-notification
    layout=nodisplay
    id="amp-user-notification1"
    data-show-if-href="https://foo.com/api/show-api?timestamp=TIMESTAMP"
    data-dismiss-href="https://foo.com/api/dismissed">
    This site uses cookies to personalize content.
    <a href="">Learn more.</a>
   <button on="tap:amp-user-notification1.dismiss">I accept</button>
</amp-user-notification>
```

### Attributes

**data-show-if-href** (Required)

AMP will make a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) GET request to this URL to determine whether the notification should be shown. It will append the `elementId` and `ampUserId` query string fields to the href provided on the `data-show-if-href` attribute. (See #1228 on why this is a GET instead of a POST.)

To prevent the browser from caching the GET response values, you should add a [TIMESTAMP URL replacement](https://github.com/ampproject/amphtml/blob/master/spec/amp-var-substitutions.md) value to the `data-show-if-href` attribute value. You can easily add it as a query string field, e.g., `data-show-if-href="https://foo.com/api/show-api?timestamp=TIMESTAMP"`.

 - `CORS GET request` query string fields
    - `elementId`
    - `ampUserId`

  Example:
    ```
      https://foo.com/api/show-api?timestamp=1234567890&elementId=notification1&ampUserId=cid-value
    ```

 - `CORS GET response` json fields
    The response must contain a single JSON object with a field "showNotification" of type boolean. If this field is `true` the notification will be shown; otherwise it will not be shown.

    - `showNotification`

    Example:
    ```json
    { "showNotification": true }
    ```

**data-dismiss-href** (Required)

AMP will make a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) POST request to this URL transmitting the `elementId` and `ampUserId` only when the user has explicitly agreed.

Use the `ampUserId` field to indicate whether the user has seen the notification before if you want to avoid showing it in the future. It is the same value that will be passed in future requests to `data-show-if-href`.

  - `POST request` json fields

    - `elementId`
    - `ampUserId`

    Example:
    ```json
    { "elementId": "id-of-amp-user-notification", "ampUserId": "ampUserIdString" }
    ```
  - `POST response` should be a 200 HTTP code and no data is expected back.

### JSON Fields

 - `elementId` (string): The HTML ID used on the `amp-user-notification` element.
 - `ampUserId` (string): This ID is passed to both this request and the dismiss request. The ID will be the same for this user going forward, but no other requests in AMP will send the same ID. You can use the ID on your side to lookup/store whether the user has dismissed the notification before.
 - `showNotification` (boolean): Boolean value indicating whether the notification should be shown. If `false` the promise associated with the element is resolved right away.

### Styling

The `amp-user-notification` component should always have `layout=nodisplay` and will be set to `position: fixed` after layout (default is `bottom: 0`, which can be overridden). If a page has more than one `amp-user-notification` element then the notifications are queued and only shown when the previous notification has been dismissed.

The `amp-active` (visibility: visible) class is added when the notification is displayed and removed when the notification has been dismissed. The `amp-hidden` (visibility: hidden) class is added when the notification has been dismissed.

You can, for example, hook into these classes for a "fade in" transition.

Example (without vendor prefixes):

```css
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  amp-user-notification.amp-active {
    opacity: 0;
    animation: fadeIn ease-in 1s 1 forwards;
  }
```

### Delaying Client ID generation until the notification is acknowledged

Optionally you can delay generation of Client IDs used for analytics and similar purposes until an `amp-user-notification` is confirmed by the user. See these docs for how to implement this:

 - [CLIENT_ID URL substitution.](../../spec/amp-var-substitutions.md#CLIENT_ID)
 - [`amp-ad`](../../builtins/amp-ad.md)
 - [`amp-analytics`](../amp-analytics/amp-analytics.md)
