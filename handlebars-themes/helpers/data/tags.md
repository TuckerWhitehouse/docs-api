---
title: "tags"
keywords:
    - api
    - handlebars
    - themes
    - helpers
---

Usage: `{{tags}}` or `{{#foreach tags}}{{/foreach}}` in `tag.hbs` you can use `{{#tag}}{{/tag}}` to access tag properties

### Description

`{{tags}}` is a formatting helper for outputting a linked list of tags for a particular post. It defaults to a comma-separated list (without list markup) but can be customised to use different separators, and the linking can be disabled. The tags are output in the order they appear on the post, these can be reordered by dragging and dropping.

The `{{tags}}` helper does not output internal tags. This can be changed by passing a different value to the `visibility` attribute.

You can use the [translation helper](/docs/t) for the `prefix` and `suffix` attribute, [see translate the tags helper](/docs/i18n#section-usage-of-subexpressions-and-translations).

### Example code

The basic use of the tags helper will output something like 'my-tag, my-other-tag, more-tagging' where each tag is linked to its own tag page:

```html
{{tags}}
```

You can customise the separator between tags. The following will output something like 'my-tag | my-other-tag | more tagging'

```html
{{tags separator=" | "}}
```

Additionally you can add an optional prefix or suffix. This example will output something like 'Tagged in: my-tag | my-other-tag | more tagging'

```html
{{tags separator=" | " prefix="Tagged in:"}}
```

You can use HTML in the separator, prefix and suffix arguments. So you can achieve something like 'my-tag • my-other-tag • more tagging'.

```html
{{tags separator=" &bull; "}}
```

If you don't want your list of tags to be automatically linked to their tag pages, you can turn this off:

```html
{{tags autolink="false"}}
```

If you want to output a fixed number of tags, you can add a `limit` to the helper. E.g. adding a limit of 1 will output just the first tag:

```html
{{tags limit="1"}}
```

If you want to output a specific range of tags, you can use `from` and `to` either together or on their own. Using `to` will override the `limit` attribute.

E.g. using from="2" would output all tags, but starting from the second tag:

```html
{{tags from="2"}}
```

E.g. setting both from and to to `1` would do the same as limit="1"

`{{tags from="1" to="1"}}` is the same as `{{tags limit="1"}}`


## The `visibility` attribute

As of Ghost 0.9 posts, tags and users all have a concept of `visibility`, which defaults to `public`. The key feature build on this so far is Internal Tags, which are tags where the `visibility` is marked as `internal` instead of `public`. These tags will therefore not be output by the `{{tags}}` helper unless you specifically ask for them.

By default the `visibility` attribute is set to the string "public". This can be overridden to pass any other value, and if there is no matching value for `visibility` nothing will be output. E.g. you can set `visibility` to be "internal" to _only_ output internal tags. You can also pass a comma-separated list of values, or the value "all" to output all items.

```html
{{tags visibility="all"}}
```

### Advanced example

If you want to output your tags completely differently, you can fully customise the output by using the [foreach](doc:foreach) helper, instead of the tags helper. Here's an example of how to output list markup:

```html
{{#post}}
  {{#if tags}}
    <ul>
    {{#foreach tags}}
      <li>
        <a href="{{url}}" title="{{name}}" class="tag tag-{{id}} {{slug}}">{{name}}</a>
      </li>
    {{/foreach}}
    </ul>
  {{/if}}
{{/post}}
```

### List of Attributes

* **id** - the incremental ID of the tag
* **name** - the name of the tag
* **slug** - slugified version of the name (used in urls and also useful for class names)
* **description** - a description of the tag
* **feature_image** - the cover image for the tag ([img_url helper](doc:img_url))
* **meta_title** - the tag's meta title  ([meta_title helper](doc:meta_title))
* **meta_description** - the tag's meta description ([meta_description helper](doc:meta_description))
* **url** - the web address for the tag's page ([url helper](doc:url))


## primary_tag

To output just the singular, first tag, use the `{{primary_tag}}` helper to output a simple link. You can also access all the same attributes as above if you need more custom output.

```html
{{#primary_tag}}
<div class="primary-tag">
    <a href="{{url}}">{{name}}</a>
    <span class="description">{{description}}</span>
<div>
{{/primary_tag}}
```