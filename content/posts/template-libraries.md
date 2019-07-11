---
title: "Template libraries"
tags:  ["template"]
date: 2015-06-01
aliases:
  - /posts-output/2015-06-01-template-libraries.html
---
Which template library should you use for web application? Hiccup / Selmer / Enlive?

<!--more-->

## Hiccup

[https://github.com/weavejester/hiccup](https://github.com/weavejester/hiccup)

> Hiccup is a library for representing HTML in Clojure. It uses vectors to represent elements, and maps to represent the element's attributes.

In simple words, you are writing code in Clojure which is transformed into HTML.


```clojure
(html [:span {:class "foo"} "bar"])
```

result


```HTML
<span class="foo">bar</span>
```

**Use it when you need CMS behaviour. In other words when you need inject generated HTML by Clojure code.** On the beginning it can sounds odd, but after years of working with Clojure you will used to Clojure syntax and appreciate it more than anything else. Especially because it the same way how you write HTML in ClojureScript.

There is also [https://github.com/teropa/hiccups](https://github.com/teropa/hiccups) which lets you use Hiccup with ClojureScript. It’s worth to notice that the code is represented by clear data, so you can easily write functions to transform this data in the way that you want.

## Selmer

[https://github.com/yogthos/Selmer](https://github.com/yogthos/Selmer)

> A fast, Django inspired template system for Clojure.

Similar to Django (Python) and Blade (PHP).

It lets you mix HTML with dynamic content code.

```html
<html>
  {% include "content.html" %}
</html>
```

```html
<body>{{content}}</body>
```

```clojure
(render-file "index.html" {:content "Example content of site"})
```

result

```html
<html>
  <body>Example content of site</body>
</html>
```

As you can see it is "keep it simple" even with nested template files.

You can also do more dynamic things like in example below. It is code to show tags connected with article on this site.

```html
{% if post.tags|not-empty %}
    tags 
    {% for tag in post.tags %}
        <a href="{{tag.uri}}">{{tag.name}}</a>
    {% endfor %}
{% endif %}
```

**Use it to generate HTML without complex CMS behaviour.** You can use it not only for HTML! I use it for example to generate e-mails content. It also makes easier cooperating with frontend team without Clojure expierience.

## Enlive

[https://github.com/cgrand/enlive](https://github.com/cgrand/enlive)

> Enlive is a selector-based (à la CSS) templating library for Clojure.

In simple words you are using this like jQuery selectors. It parses clear HTML template, finds used selectors and changes the content. Templates are pure HTML. Dynamic part of site is defined in Clojure. Notice: HTML and dynamic content code in Clojure are separated.

Please keep attention on *title* and *:head :title* (à la CSS selector) in example below.

```clojure
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>This is a title placeholder</title>
  </head>
  <body>
  </body>
</html>
```


```clojure
(html/deftemplate main-template "templates/html_file_above.html"
  []
  [:head :title] (html/content "Enlive starter kit"))
```

result

```clojure
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Enlive starter kit</title>
  </head>
  <body>
  </body>
</html>
```

If you have some reasons to keep HTML templates separate from dynamic content it is a good choice. Also notice that frontend coders can use temporary content like in our example "This is a title placeholder". It makes their works easier during developing if they don't have coder skills, but makes coder work harder.

**Personally I has never been usefull to make my templates.**

## Conclusion

In practice, for all my use cases I use Hiccup with ClojureScript and Selmer with Clojure. I also use Selmer to generate content like e-mail body.