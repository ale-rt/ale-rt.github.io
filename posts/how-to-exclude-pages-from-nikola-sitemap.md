Short answer
------------

You should have a line like this in your `conf.py` file:

```python
ROBOTS_EXCLUSIONS = ["/404.html", "/*/404.html"]
```

As you can see you the exclusion list works with both exact URLs or expressions containing wildcards.

Long answer &mdash; tl;dr
-------------------------

When you publish a site on **github pages** you can add a `404.html`. That page will be served each time you look for a not existing resource.

This can happen, for example, because you followed a broken link, mistyped a URL or the URL refers to a content that was moved or removed.

I use <a href="http://getnikola.com/">Nikola</a> to generate my site on github pages and of course I use it to generate my `404.html`.

Nikola is really smart and does a lot lot of stuff for you, for example it generates a <a href="/sitemap.xml">`sitemap.xml`</a> that keeps track of all the pages it creates and manages.

The `sitemap.xml` is used, for example, by search engine to index your website, so you really want to have it in order.

If you let Nikola handle the 404.html page you will have to instruct the software to threat it in a special way, in order to not avoid those lines in your `sitemap.xml`:

```xml
<url>
  <loc>http://ale-rt.github.io/it/404.html</loc>
  <lastmod>2014-12-08</lastmod>
</url>
```

Otherwise the web crawlers will get upset because the http status from github pages will be (like you can guess) 404 Not found!

Fiddling with the `ROBOTS_EXCLUSIONS` variable as shown at the beginning of the article will just make this work for you.

Final remark about wildcards
----------------------------

Not every robot understands wildcards. So, if your goal is just to exclude the `404.html` from the sitemap your are fine with the lines shown at the beginning of the article, otherwise you may want to be more explicit:

```python
ROBOTS_EXCLUSIONS = ["/404.html", "/it/404.html"]
```
