---
layout: page-dev-2.0
title: Developers &#8250; Data sources
description:  Data sources can load data fromcertain locations like filesystem or database
header: 
  title: Data sources
  sub: Developers
prettify: true
---
Data sources can load data from certain locations like filesystem or database.
Every data source must extends `AbstractDataSource`. Data loaded from data sources are
called items. There is three type of them: content, layout and include.
Items must implements [`ItemInterface`](https://github.com/spress/Spress/blob/master/src/Core/DataSource/ItemInterface.php).

## Items {#items}

Items are the basic building blocks and consist of content and metadata attributes.
Each item has an identifier and a set of attributes (metadatas). Attributes consist of
*key-value* pairs located at the [front-matter](/docs/2.0/front-matter/) or in a separeted
metadata file which are stored in [YAML format](https://en.wikipedia.org/wiki/YAML).

### How to create an item?

```
use Yosymfony\Spress\Core\DataSource\Item;

$item = new Item('Test of content', 'myPage.md', ['title' => 'My page']);
```

`Item` is an implementation of `ItemInterface`.

To get and set attributes uses `getAttributes` and `setAttributes` methods:

```
use Yosymfony\Spress\Core\DataSource\Item;

$item = new Item('Test of content', 'myPage.md', ['title' => 'My page']);

$attributes = $item->getAttributes();
$attributes['parmalink'] = '/my-page'

$item->setAttributes($attributes);
```

First argument is the content, second is the identifier and last one are the attributes.

### Setting the path

The item's path defines the relative route of the item in the site.

### Binary content

If the extension of the item is included in the site configuration’s `text_extension` attribute, it is considered to be
textual. Otherwise it will be considered as binary. More details about [text_extension value](/docs/2.0/configuration/#deafult-configuration).


### Type of items

There is three type of item:

* **content**: `ItemInterface::TYPE_ITEM` (default) regular content like post or pages.
* **layout**: `ItemInterface::TYPE_LAYOUT` indicates how to organize the content.
* **include**: `ItemInterface::TYPE_INCLUDE` reusable content.

The Item's constructor accept a fifth argument for the type:

{% verbatim %}
```
use Yosymfony\Spress\Core\DataSource\Item;

$layoutItem = new Item('{{ page.content }}', 'page.html', [], false, ItemInterface::TYPE_LAYOUT);
```
{% endverbatim %}

### Snapshots of content and paths

## Predefined data sources

### FilesystemDataSource {#filesystemDataSource}

Spress comes with [`FilesystemDataSource`](https://github.com/spress/Spress/blob/master/src/Core/DataSource/Filesystem/FilesystemDataSource.php) to load your site.
Data sources are defined at `data_source` option at site configuration file:

```yaml
data_sources:
  filesystem:
    class: 'Yosymfony\Spress\Core\DataSource\Filesystem\FilesystemDataSource'
    arguments:
      source_root: '%site_dir%/src'
      include: '%include%'
      exclude: '%exclude%'
      text_extensions: '%text_extensions%'
      attribute_syntax: '%attribute_syntax%'
```
This is the **default configuration** and is not necessary to modify your `config.yml` file at your site.


### MemoryDataSource {#memoryDataSource}