# Global settings

To communication with Contentful you have to set at least `access_token` and `space_id`.
You can find these in your [app](https://app.contentful.com) at `APIs -> Content delivery API Keys`.

```json
{
  "source": "src",
  "destination": "build",
  "plugins": {
    "contentful-metalsmith": {
      "access_token" : "YOUR_CONTENTFUL_ACCESS_TOKEN",
      "space_id": "YOUR_CONTENTFUL_SPACE_ID",
      "host": "preview.contentful.com",
      "entry_key": "_key",
      "entry_extension": "md",
      "contentful": {
        "content_type": "2wKn6yEnZewu2SCCkus4as"
      },
      "autoLayout": {
         "layoutFileType": "html"
     }
    }
  }
}
```

## Parameters

### `access_token` *(optional)*

Global access token used to connect with the Contentful API.
You can define the `access_token` global in your `metalsmith.json` or define it separately in given source files.

*Recommended way here is to set the `access_token` and `space_id` of your mainly used space in the `metalsmith.json` or global config and overwrite it if needed in depending source files.*

**Side note:** When you decide to not set a global `access_token` you have to set it in every single source file.

See [source file settings](./source-file-settings.md) for further information.

### `space_id` *(optional)*

Global space id the data will be fetched from.
You can define the `space_id` global in your `metalsmith.json` or define it separately in given source files.

*Recommended way here is to set the `access_token` and `space_id` of your mainly used space in the `metalsmith.json` or global config and overwrite it if needed in depending source files.*

**Side note:** When you decide to not set a global `space_id` you have to set it in every single source file.

See [source file settings](./source-file-settings.md) for further information.

### `host` *(optional)*

In case you want to use the [Content Preview API](https://www.contentful.com/developers/docs/references/content-preview-api/) you can set the depending token
and change the `host` property to `preview.contentful.com`.

For using the [Content Delivery API](https://www.contentful.com/developers/docs/references/content-delivery-api/) you can ignore this option, as it is defaulting to **Content Delivery API**.

*Recommended way here is to set the `host` in the `metalsmith.json` or global config and overwrite it if needed in depending source files.*

### `entry_key` *(optional)*

If you want to transform Contentful data into pages you can specify a `entry_key` which will be used to replace the key of all file objects with the stored Contentful key path. You must specify a path to the file as if it was referenced from your `src` directory and exclude the extension.

For example, if you specify `entry_key`: `_key`, then create a Contentful entry with a `_key` property set to `pages/index` a file will be referenced in Metalsmith with `pages/index.md` (assuming you specify `entry_extension` as `md`)

### `entry_extension` *(optional)*

If you specify `entry_key`, you will need to specify the entry extension for all file keys. This will be appended to the key on Contentful entries that contain the `entry_key`.

### `filterTransforms` *(optional)*

If you want to use dynamic values in a source file's filter query, you can provide an object containing named functions that will be invoked during the metalsmith build. For example, if you provide a configuration like this:

```javascript
{
  // ...
  filterTransforms: {
    __NOW__() {
      return (new Date()).toISOString();
    },
  },
  // ...
}
```

Note that you will need to invoke metalsmith programatically & process configuration as javascript, not JSON, in this case.

You can then use the keyword `__NOW__` in your source file's `filter` values, like so:

```markdown
---
title: Articles with certain start and end dates
contentful:
  content_type: post
  filter:
    'startdate[lte]': '__NOW__'
    'enddate[gt]': '__NOW__'
layout: posts.html
---
```

### [`contentful` *(optional)*](source-file-settings.md)

### `common` *(optional)*

The results of queries placed in this property will be made available in all templates

For example, find your five latest entries so you can include them in various templates:

```javascript
{
  // ...
  "common": {
    "latest": {
      "content_type": "post",
      "limit": 5,
      "order": "sys.createdAt"
    }
  },
  // ...
}
```

In templates you can then access `common.latest` to get the raw results of the above query:

```
The first latest post's title, in handlebars syntax: {{ common.latest.items.0.fields.title }}
```

### `autoLayout` *(optional)*

Contentful pages can be automatically mapped to a layout with the same name as the content type in Contentful.

The layoutFileType variable needs is mandatory and controls which file type the layout files are using, typically html. 

```javascript
{
  // ...
  "autoLayout": {
      "layoutFileType": "html",
  },
  // ...
}
```

In templates you can then access `common.latest` to get the raw results of the above query:

```
The first latest post's title, in handlebars syntax: {{ common.latest.items.0.fields.title }}
```
