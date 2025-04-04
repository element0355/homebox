# Tips and Tricks

## Custom Fields

Custom fields are a great way to add any extra information to your item. The following types are supported:

- [x] Text
- [ ] Integer (Future)
- [ ] Boolean (Future)
- [ ] Timestamp (Future)

Custom fields are appended to the main details section of your item.

::: tip
Homebox Custom Fields also have special support for URLs. Provide a URL (`https://google.com`) and it will be automatically converted to a clickable link in the UI. Optionally, you can also use Markdown syntax to add a custom text to the button. `[Google](https://google.com)`
:::

## Managing Asset IDs

Homebox provides the option to auto-set asset IDs, this is the default behavior. These can be used for tracking assets with printable tags or labels. You can disable this behavior via a command line flag or ENV variable. See [configuration](/en/quick-start.md#env-variables-configuration) for more details.

Example ID: `000-001`

To search for an Asset ID: type `#` in the search bar followed by the ID you're searching for, e.g. `#000-001`.

Asset IDs are partially managed by Homebox, but have a flexible implementation to allow for unique use cases. IDs are non-unique at the database level, so there is nothing stopping a user from manually setting duplicate IDs for various items. There are two recommended approaches to manage Asset IDs:

### 1. Auto Incrementing IDs

This is the default behavior likely to experience the most consistency. Whenever creating or importing an item, that item receives the next available ID. This is recommended for most users.

### 2. Auto Incrementing IDs with Reset

In some cases, you may want to skip some items such as consumables, or items that are loosely tracked. In this case, we recommend that you leave auto-incrementing IDs enabled _however_ when you create a new item that you want to skip, you can go to that item and reset the ID to 0. This will remove it from the auto-incrementing sequence, and the next item will receive the next available ID.

::: tip
If you're migrating from an older version, there is an action on the user's profile page to assign IDs to all items. This will assign the next available ID to all items in order of their creation. You should __only do this once__ during the migration process. You should be especially cautious with this if you're using the reset feature described in [option number 2](#2-auto-incrementing-ids-with-reset)
:::

## QR Codes

:label: 0.7.0

Homebox has a built-in QR code generator that can be used to generate QR codes for your items. This is useful for tracking items with a mobile device. You can generate a QR code for any item by clicking the QR code icon in the top right of the item details page. The same can be done for the Labels and Locations page. Currently, support is limited to generating one-off QR Codes.

However, the API endpoint is available for generating QR codes on the fly for any item (or any other data) if you provide a valid API key in the query parameters. An example url would look like `/api/v1/qrcode?data=https://homebox.fly.dev/item/{uuid}`. Currently, the easiest way to get an API token is to use one from an existing URL of the QR Code in the API key, but this will be improved in the future.

:label: v0.8.0

In version 0.8.0 We've added a custom label generation. On the tools page, there is now a link to the label-generator page where you can generate labels based on Asset ID for your inventory. These are still in early development, so please provide feedback. There's also more information on the implementation on the label generator page.

[Demo](https://demo.homebox.software/reports/label-generator)

:label: v0.18.0

Homebox has a built-in QR code reader that can be used to scan QR codes for your items. This is useful for tracking items with a mobile device.

:label: v0.18.0

Homebox also has a built-in one off label generator for those with proper label makers. This can be accessed via the "Labels" button on the right hand side under the main details on the item page. Locations can also be printed in the same way, although the labels button is located next to the edit icon.

## Scheduled Maintenance Notifications

:label: v0.9.0

Homebox uses [shoutrrr](https://containrrr.dev/shoutrrr/) to send notifications. This allows you to send notifications to a variety of services including Discord, Slack, IFTTT, generic webhooks, and SMTP-based email. 

On your profile page, you can create a new **Notifier** using a supported shoutrrr notification URL to send notifications when a maintenance event is scheduled. 

For the full list of services and how to configure the service URL, refer to the [Services Overview](https://containrrr.dev/shoutrrr/services/overview/) in shoutrrr's documentation. 

**Notifications are sent on the day the maintenance is scheduled at or around 8am.**

As of `v0.9.0`, there is limited support for complex scheduling of maintenance events. If you have requests for extended functionality, please [open an issue on GitHub](https://github.com/sysadminsmedia/homebox/issues/new?template=feature_request.yml) or reach out on Discord. We're still gauging the demand for this feature.


## Custom Currencies

:label: v0.11.0

Homebox allows you to add additional currencies to your instance by specify a JSON file containing the currencies you want to add.

**Environment Variable:** `HBOX_OPTIONS_CURRENCY_CONFIG`

### Example

```json
[
  {
    "code": "AED",
    "local": "United Arab Emirates",
    "symbol": "د.إ",
    "name": "United Arab Emirates Dirham"
  },
]
```

## Copy to Clipboard

The copy to clipboard functionality requires a secure context (HTTPS or localhost) to work due to browser security restrictions. If you're accessing Homebox through HTTP, the copy button will not function.

To enable this feature:
- Use HTTPS by setting up a reverse proxy (like Nginx or Caddy)
- OR access Homebox through localhost
