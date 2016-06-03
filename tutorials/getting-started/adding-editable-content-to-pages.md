---
title: "Adding Editable Content To Pages"
---

## Adding Editable Content to Pages

Central to Apostrophe is the philosophy that editors can edit their content in context. In order for you to enable this functionality in your templates, Apostrophe provides `singleton` and `area` helpers that you can use in your templates.

### Singletons

Singletons are a slot on a page that allow an editor to add a single "widget" of a specific type. Let's add a singleton to our `home.html` template!

```markup
{% block main %}
  <div class="main-content">
    {{ apos.singleton(data.page, 'textBlock', 'apostrophe-rich-text', {
      toolbar: [ 'Bold', 'Italic' ]
    }) }}
  </div>
{% endblock %}
```

Let's deconstruct the arguments we are passing here.

##### `data.page`

The page object you want to save the singleton to. In this case, we are editing content on the current page, so the content should be saved to that page.

##### `'textBlock'`

The name of the area. This denotes what property the singleton will be saved to in the page object. In this case, it would be saved to `data.page['textBlock']`.

##### `'apostrophe-rich-text'`

The widget type we want to allow in the singleton. In this case, we are using the standard rich text widget.

##### `{ toolbar: [ 'Bold', 'Italic' ] }`

An "options" object that allows us to pass type-specific options to the widget. Rich text widgets support a `toolbar` option.

If we restart our server and refresh our site's home page while logged in, we'll see a gray block with a button prompting us to add rich text. Once you start adding text, your edits save automatically. **There is no "save" button because you don't need one.** All edits are saved in the background.

<img src="/images/tutorials/developer/boilerplate_singleton.png" class="shadow">

### Areas

Oftentimes, we'll want to enable an editor to add several widgets of different types to build out a column of content in a page. For this, we can use the `apos.area` helper.

```markup
{{ apos.area(data.page, 'body', {
  widgets: {
    'apostrophe-rich-text': {
      toolbar: [ 'Bold', 'Italic' ]
    },
    'apostrophe-images': {}
  }
}) }}
```

The first two arguments for this helper work the same way as for `apos.singleton`. The third option is an options object. We can see that that options object supports a `widgets` option, which takes a key-value map of available widget names and their respective options.

### Widget Types

Apostrophe offers a range of widgets, and you can easily create your own. Here are some of the most popular widgets. Later on we'll also talk about "pieces" and how they allow you to create and reuse the same content around the site via pages and widgets.

#### `apostrophe-rich-text`

The `apostrophe-rich-text` widget provides a rich text editor. It is based on [CKEditor 4](http://ckeditor.com/). You can use options to configure it. Here's an example with the two most commonly used options:

```markup
  {{ apos.singleton(data.page, 'textBlock', 'apostrophe-rich-text', {
    toolbar: [ 'Styles', 'Bold', 'Italic' ],
    styles: [
      { name: 'Title', element: 'h3' },
      { name: 'Meta', element: 'h5' }
    ]
  }) }}
```

*All of these widgets work in areas, too.* We're just using singletons for the examples.

##### `toolbar`

`toolbar` determines which controls appear in the editor's toolbar. Here are the frequently used controls:

`Styles`, `Bold`, `Italic`, `Link`, `Unlink`, `Anchor`, `Table`, `BulletedList`, `Blockquote`, `Strike`, `Subscript`, `Superscript`

**For good responsive development you should avoid offering controls for font sizes, colors, etc.** Let your design's styles provide those things in a thoughtful way.

##### `styles`

`styles` specifies an array of valid CKEditor styles, which will appear on the "Style" dropdown menu if it is included in the `toolbar`. Each style has a `name` property and an `element` property. Most semantic HTML5 elements are allowed here.

#### `apostrophe-images`

The `apostrophe-images` widget lets you add one or more images. If multiple images are added in a single widget, they are presented as a slideshow.

Here is an example with the popular options:

```markup
  {{ apos.singleton(data.page, 'heroPic', 'apostrophe-images', {
    minSize: [ 700, 350 ],
    aspectRatio: [ 2, 1 ],
    limit: 1
  }) }}
```

##### `minSize`

`minSize` ensures the user can't pick images smaller than the given dimensions `[ width, height ]`.

##### `aspectRatio`

`aspectRatio` will crop the images to display the largest portion that has the given aspect ratio `[ x, y ]`, throwing away some of the image if necessary. If the user clicks the cropping button while picking the image, the cropping controls will also enforce the aspect ratio.

##### `limit`

`limit` prevents the user from selecting more than the given number of images. A `limit` of `1` is appropriate where you don't want a slideshow treatment.

#### `apostrophe-files`

The `apostrophe-files` widget lets you add download links to access various documents, such as PDFs or Word files. If multiple files are added in a single widget, they are presented as a list.

Here's a simple example:

```markup
  {{ apos.singleton(data.page, 'resume', 'apostrophe-files', {
    limit: 1
  }) }}
```

##### `limit`

`limit` prevents the user from selecting more than the given number of files.

##### More about files and Apostrophe

Users editing these widgets have the option of uploading files or reusing them. By default the office-oriented file types accepted by Apostrophe are:

`[ 'txt', 'rtf', 'pdf', 'xls', 'ppt', 'doc', 'pptx',
'sldx', 'ppsx', 'potx', 'xlsx', 'xltx', 'csv', 'docx', 'dotx' ]`

In addition to GIF, JPEG and PNG files.

This can be adjusted by configuring the `apostrophe-attachments` module.


### More Widgets

Is that all? Definitely not! We'll discuss "pieces" and other strategies for creating custom widgets in a later tutorial.

Oh, and there's the `apostrophe-html` widget. If you absolutely must.