# Tree plugin for [DocPad](http://docpad.org/)

[![Latest Release](http://img.shields.io/npm/v/docpad-plugin-tree.svg?style=flat)](https://www.npmjs.org/package/docpad-plugin-tree) [![Code Quality](http://img.shields.io/codeclimate/github/kasperisager/docpad-plugin-tree.svg?style=flat)](https://codeclimate.com/github/kasperisager/docpad-plugin-tree) [![Dependency Status](http://img.shields.io/gemnasium/kasperisager/docpad-plugin-tree.svg?style=flat)](https://gemnasium.com/kasperisager/docpad-plugin-tree) [![Dowloads](http://img.shields.io/npm/dm/docpad-plugin-tree.svg?style=flat)](https://www.npmjs.org/package/docpad-plugin-tree) 

[DocPad](http://docpad.org/) plugin that when given a collection will construct a hierarchical tree of documents. Perfect for navigation menus!

## Install

```sh
$ docpad install tree
```

## Usage

The plugin exposes a template helper, `tree(collection, context)`, that you can use for constructing a tree of documents from any given collection:

`tree(...)`  | Type     | Description
---          | ---      | ---
`collection` | `string` | The name of the collection used for constructing the tree. Default: `documents`
`context`    | `object` | The context in which this tree is to be constructed. Will "highlight" the current document path if set. You'll typically want to set it to `@document`. _Optional_

For each document in the collection, you can set the following meta information:

```yml
title: "Fishing with my uncle" # Title of the page
menu: "Fishing tricks"         # Title that will appear in the tree (optional)
order: 0                       # Sort order
hidden: false                  # Whether or not to hide the item from the tree
```

A constructed tree will look something like this:

```json
[
  {
    "title": "Some Page",
    "url": "/some-page",
    "order": 0,
    "hidden": false,
    "children": [
      {
        "title": "A child page",
        "url": "/some-page/a-child-page",
        "order": 0,
        "hidden": false
      }
    ]
  },
  {
    "title": "Current page",
    "url": "/current-page",
    "order": 0,
    "hidden": false,
    "active": true
  }
]
```

The tree can then be used to create, say, a nested navigation menu:

```html
<% menu = (items) => %>
  <ul class="nav nav-stacked">
    <% for item in items: %>
      <li<%= " class=active" if item.active %>>
        <a href="<%= item.url %>"><%= item.title %></a>
        <%- menu item.children if item.children %>
      </li>
    <% end %>
  </ul>
<% end %>

<%= menu @tree('html', @document) %>
```

---
Copyright &copy; 2014 [Kasper Kronborg Isager](https://github.com/kasperisager). Licensed under the terms of the [MIT License](LICENSE.md).
