# Lightgallery Markdown Caption Extension

Markdown extension to wrap images in a lightbox with a caption.

Will only wrap images which are wrapped in a hyperlink who's href matches the image source. Any text in the same paragraph immediately after the hyperlink is used as a caption in lightbox.

Example:

```
[![Alt](/img/pic1.png)](/img/pic1.png)
Caption
```

This will output:

```html
<p>
  <div class="lightgallery">
    <a href="../img/pic1.png" data-sub-html="Caption">
      <img alt="Alt" src="../img/pic1.png" />
    </a>
    Caption
  </div>
</p>
```

Sometimes, when there is a small image, you may wish to force the caption to be on the next line due to the way markdown is rendered in some contexts. In this case, if there is no text immediately after the hyperlink tag, this extension will try to use the image Alt tag as the caption.

```
[![Lightbox Caption](/img/pic1.png)](/img/pic1.png)

Markdown Caption
```

Which will output:

```html
<p>
  <div class="lightgallery">
    <a href="../img/pic1.png" data-sub-html="Lightbox Caption">
      <img alt="Lightbox Caption" src="../img/pic1.png" />
    </a>
    Markdown Caption
  </div>
</p>
```

Alternatively, you may also use a `<br>` tag and avoid needing to specify the alt tag:

```
[![](/img/pic1.png)](/img/pic1.png)
<br>Caption
```

This extension is a modification of [lightgallery-markdown](https://github.com/g-provost/lightgallery-markdown), designed to be more backwards compatible with markdown, markdown using this extension will gracefully degrade.

The extension is made to work with [lightgallery.js](https://github.com/sachinchoolur/lightgallery.js) a full featured JavaScript lightgallery/lightbox with no dependencies.

**To test the extension:**

```python
import markdown
from lightgallery import LightGalleryExtension

print markdown.markdown('[![](/img/pic1.png)](/img/pic1.png)', extensions=[LightGalleryExtension()])
```


## Install

```bash
$ cd lightgallery-markdown-caption

$ python setup.py install
```

## How to make it works with Mkdocs

**1.** Create a **theme** folder will the following structure in your Mkdocs folder

```
theme/
|_ css/
|_ fonts/
|_ img/
|_ js/
````

**2.** Go to [lightgallery.js GitHub](https://github.com/sachinchoolur/lightgallery.js) or [JSDELIVR](https://www.jsdelivr.com/package/npm/lightgallery.js) to download the following files to the **theme** sub-folders as listed below

- dist/js/lightgallery.min.js **->** theme/js/
- dist/css/lightgallery.min.css **->** theme/css/
- dist/fonts/lg.* **->** theme/fonts/
- dist/img/loading.gif **->** theme/img/

**3.** Create a **theme/main.hml** file and add the following code to the file

```html
{% extends "base.html" %}

{% block styles %}
    {{ super() }}
    <link rel="stylesheet" href="{{ base_url }}/css/lightgallery.min.css">
{% endblock styles %}

{% block libs %}
    {{ super() }}
    <script src="{{ base_url }}/js/lightgallery.min.js"></script>
{% endblock libs %}

{% block scripts %}
    {{ super() }}
    <script>
    var elements = document.getElementsByClassName("lightgallery");
    for(var i=0; i<elements.length; i++) {
       lightGallery(elements[i]);
    }
    </script>
{% endblock scripts %}
```

**4.** Modify the **mkdocs.yml** file to add the following settings

```
# Documentation and theme
theme:
  custom_dir: 'theme'
```


```
# Extensions
markdown_extensions:
  - lightgallery_caption
```

## License

MIT License
