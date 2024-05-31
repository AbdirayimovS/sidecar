Sidecar
=======

A plain but pretty [Pelican](https://getpelican.com/) theme with a little sidebar.

Features
--------

* **Nice-looking text** with lots of extra typographical features.

  Sidecar uses Oatcake, my CSS typography stylesheet.
  See [Oatcake's site](https://www.seanh.cc/oatcake/) for all the details.

* **Responsive design**: works great on both desktop and mobile.

* **Supports most of Pelican's features**, including
  pagination (if enabled by Pelican's [`DEFAULT_PAGINATION` / `PAGINATED_TEMPLATES` settings](https://docs.getpelican.com/en/latest/settings.html#pagination));
  [syntax highlighting](https://docs.getpelican.com/en/latest/content.html#internal-pygments-options) for code blocks;
  archives page, period archives pages, and author/authors, category/categories and tag/tags pages;
  [Atom and RSS feeds](https://docs.getpelican.com/en/latest/settings.html#feed-settings) (with feed autodiscovery links in the HTML `<head>`);
  and various Pelican settings including
  `SITENAME` (used in tab and feed titles),
  `SITEURL` (used in links),
  `DEFAULT_LANG` (used for the HTML `lang`)
  and `GITHUB_URL` (used for the GitHub ribbon, see below).

  Support for a couple of Pelican features is still missing, including
  [translations](https://github.com/seanh/sidecar/issues/1)
  and [author, category and tag feeds](https://github.com/seanh/sidecar/issues/18).

* **Standard, semantic HTML**:
  `<main>` for the main content of index and static pages,
  `<article>` for articles,
  `<footer>` for article footers,
  `<time>` for the publication dates in article footers,
  `rel="author"` for article author links,
  `rel="bookmark"` for article permalinks,
  `rel="tag"` for tag links,
  `rel="next"` and `rel="prev"` for pagination links.
  Links have nice `title`'s for tooltips.
  Pages have nice `<title>`'s for tab titles.

* Optional **tables of contents** for static pages and articles.

Usage
-----

1. Clone the theme to a local directory:

   ```terminal
   $ git clone https://github.com/seanh/sidecar.git
   ```

2. Set the [`THEME`](https://docs.getpelican.com/en/latest/settings.html#THEME)
   setting in your Pelican config to the path to your local clone of Sidecar.
   It can be either an absolute path or a path relative to your Pelican config
   file:

   ```python
   # pelicanconf.py

   THEME = "../sidecar"
   ```

Removing the archives page
--------------------------

Sidecar differs from Pelican's built-in themes in that it shows only the titles
of articles on index pages (like the front page). Sidecar doesn't support
showing full article contents or summaries on index pages.

This can make Pelican's archives page unnecessary, especially if you have
pagination disabled on the front page: the archives page is just the same as
the front page. So you might want to remove your site's archives page.
You can do this with a couple of settings in your Pelican config:

```python
# pelicanconf.py

# Disable the archives page.
ARCHIVES_SAVE_AS = ""
ARCHIVES_URL = ""
```

Article summaries are still used in RSS and Atom feeds.

<details>
  <summary><h4>Why Sidecar doesn't support article summaries or full bodies on index pages</h4></summary>

Pelican's built-in `simple` and `notmyidea` themes show summaries of articles
on index pages (like the front page) by default.
If an article has a handwritten `summary` in its metadata then that's used as
the article's summary.
Otherwise a summary is generated by truncating the article body after
`SUMMARY_MAX_LENGTH` (50) words and appending `SUMMARY_END_SUFFIX` (…).
If `SUMMARY_MAX_LENGTH` is set to `None` then the full bodies of articles are
shown on index pages, instead of just summaries.

Hand-writing a summary adds extra work to posting an article.

Automatically generating summaries by truncating articles after a fixed number
of words gives bad results, cutting off articles in the middle of a sentence,
list, heading, code block, or who knows what, sometimes catching an image or
video in the summary, etc.

Showing full article bodies on index pages also works poorly with my content:
many of my articles are very long and would push earlier posts many screenfuls
down. With so much text to scroll past it would be difficult to spot when one
article ends and the next begins.

I didn't want to offer an option of full bodies, summaries, or only titles
because this would make for more design work, having to come up with
good-looking index page designs for all three options when I'm only actually
going to use one of them on my site.

So I ended up deciding to support only article titles on index pages. I like
how quick and easy it is to find articles and navigate between them.  Tag and
category pages that're just a list of article titles are particularly nice for
getting an overview of everything posted in that tag or category.
</details>

Tables of contents
------------------

Sidecar uses [Tocbot](https://tscanlin.github.io/tocbot/) to generate tables of
contents with anchor links from
[AnchorJS](https://www.bryanbraun.com/anchorjs/).

Insert a div with the CSS class `toc` anywhere in a page or article and it'll
be turned into a table of contents based on the page or article's headers:

```html
<!-- Tocbot will turn this div into a table of contents. -->
<div class="toc"></div>
```

If you want to omit a particular heading from the table of contents add the CSS
class `notoc` to it:

```html
<h1 class="notoc">Heading</h1>
```

If you don't want a heading to have an anchor link either, add `noanchor` to it
(this will also remove the heading from the table of contents):

```html
<h1 class="noanchor">Heading</h1>
```

Customization
-------------

### GitHub ribbon

If Pelican's [`GITHUB_URL`](https://docs.getpelican.com/en/latest/settings.html#GITHUB_URL)
setting is set in your Pelican config then a GitHub ribbon linking to
`GITHUB_URL` will be added to the top-right corner of your site.
The idea is that you set `GITHUB_URL` to your GitHub profile page.
For example:

```python
# pelicanconf.py

GITHUB_URL = "https://github.com/seanh"
```

Alternatively, if you keep your site's source files in a GitHub repo, you can
set `GITHUB_REPO_URL` to the URL of that GitHub repo instead. If you use
`GITHUB_REPO_URL` instead of `GITHUB_URL` then on your static pages and
articles the GitHub ribbon will link directly to the page or article's source
file on GitHub:

```python
# pelicanconf.py

GITHUB_REPO_URL = "https://github.com/seanh/seanh.github.io"
```

This can be particularly useful for you as the author of the site because, if
you're logged in to GitHub, the GitHub pages for your files have
<kbd>Edit</kbd> buttons that you can use to edit your source files right in the
GitHub web interface. So if you spot a typo in a page or article, you can
quickly click on the GitHub ribbon then on GitHub's <kbd>Edit</kbd> button and
correct it.

<details>
  <summary>Fixing broken GitHub links</summary>

If Sidecar doesn't get the path to your content directory within your GitHub
repo correct you can fix it by adding a `RELPATH` setting to your Pelican
config file containing the path to your content directory relative to the root
of the repo, for example:

```python
# pelicanconf.py

RELPATH = "content"
```
</details>

### Customizing the sidebar contents

The contents of the sidebar respond to default Pelican settings:

* A home link is always included at the top of the menu (uses Pelican's [`SITEURL` setting](https://docs.getpelican.com/en/latest/settings.html#SITEURL))
* Any items from Pelican's [`MENUITEMS`](https://docs.getpelican.com/en/latest/settings.html#MENUITEMS) setting are included
* If [`DISPLAY_PAGES_ON_MENU`](https://docs.getpelican.com/en/latest/settings.html#basic-settings) is `True` then links to each of your site's static pages will be included
* If [`DISPLAY_CATEGORIES_ON_MENU`](https://docs.getpelican.com/en/latest/settings.html#basic-settings) is `True` then links to Pelican's category pages for each of your site's categories will be included

Alternatively, you can customize the sidebar directly by adding a
`SIDECAR_MENU` setting (list of strings) to your Pelican config. For example:

```python
# pelicanconf.py

SIDECAR_MENU = [
    "HOME",
    "MENUITEMS",
    "PAGES",
    "CATEGORIES",
    "TAGS",
    "ARCHIVES",
    "AUTHORS",
    '<a rel="external" href="https://example.com">Custom Link</a>',
]
```

Certain string values have special meanings in `SIDECAR_MENU`:

* `HOME`: inserts a link to your site's home page (uses Pelican's [`SITEURL` setting](https://docs.getpelican.com/en/latest/settings.html#SITEURL))

* `MENUITEMS`: inserts the items from Pelican's [`MENUITEMS` setting](https://docs.getpelican.com/en/latest/settings.html#MENUITEMS)

* `PAGES`: inserts links to each of your site's static pages

  You can customize the URL format of static pages with Pelican's matching [`PAGE_SAVE_AS` and `PAGE_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings).

* `CATEGORIES`: inserts links to Pelican's category pages for each of your site's categories.

  You can customize the URL format of category pages with Pelican's matching [`CATEGORY_SAVE_AS` and `CATEGORY_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings).

* `TAGS`: inserts links to Pelican's tag pages for each of your site's tags.

  You can customize the URL format of tag pages with Pelican's matching [`TAG_SAVE_AS` and `TAG_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings).

* `ARCHIVES`: inserts a link to your site's archives page.

  You must add a custom `ARCHIVES_URL` setting to your Pelican config for
  generation of this link to work correctly,
  and `ARCHIVES_URL` must match up with Pelican's [`ARCHIVES_SAVE_AS` setting](https://docs.getpelican.com/en/latest/settings.html#url-settings).
  For example:

  ```python
  ARCHIVES_SAVE_AS = "archive/index.html"
  ARCHIVES_URL = "/archive/"
  ```

* `AUTHORS`: inserts a link to your site's authors page.

  You must add a custom `AUTHORS_URL` setting to your Pelican config for
  generation of this link to work correctly,
  and `AUTHORS_URL` must match up with Pelican's [`AUTHORS_SAVE_AS` setting](https://docs.getpelican.com/en/latest/settings.html#url-settings).
  For example:

  ```python
  AUTHORS_SAVE_AS = "authors/index.html"
  AUTHORS_URL = "/authors/"
  ```

Items in `SIDECAR_MENU` that don't match any of the special strings above are
rendered directly. This lets you include your own raw HTML strings as menu
items. For example you could include a custom link. This lets you use HTML
attributes other than `href` in your sidebar links, which you can't do with
Pelican's `MENUITEMS`:

```python
# pelicanconf.py

SIDECAR_MENU = [
    ...
    '<a rel="external" href="https://example.com">Custom Link</a>',
]
```

The substring `{SITEURL}` will be replaced with Pelican's
[`SITEURL` setting](https://docs.getpelican.com/en/latest/settings.html#SITEURL).
For example:

```python
# pelicanconf.py

SIDECAR_MENU = [
    ...
    '<a rel="license" href="{SITEURL}/license/">License</a>',
]
```

### Customizing the article footer contents

You can customize the contents of the footers at the bottoms of articles by
adding a `SIDECAR_ARTICLE_FOOTER` setting (list of strings) to your Pelican
config. For example:

```python
# pelicanconf.py

SIDECAR_ARTICLE_FOOTER = [
    "AUTHORS",
    "TIME",
    "CATEGORY",
    "TAGS",
]
```

Certain string values have special meanings in `SIDECAR_ARTICLE_FOOTER`:

* `AUTHORS`: insert links to Pelican's author pages for the article's authors.

  You can customize the URL format of author pages with Pelican's matching [`AUTHOR_SAVE_AS` and `AUTHOR_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings).

* `TIME`: inserts the article's publication date/time.

  You can customize the format of dates with Pelican's [`DEFAULT_DATE_FORMAT` and `DATE_FORMATS` settings](https://docs.getpelican.com/en/latest/settings.html#time-and-date).

* `CATEGORY`: inserts a link to Pelican's category page for the article's category.

  You can customize the URL format of category pages with Pelican's matching [`CATEGORY_SAVE_AS` and `CATEGORY_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings).

* `TAGS`: inserts links to Pelican's tag pages for each of the article's tags, if any.

  You can customize the URL format of tag pages with Pelican's matching [`TAG_SAVE_AS` and `TAG_URL` settings](https://docs.getpelican.com/en/latest/settings.html#url-settings).

Items in `SIDECAR_ARTICLE_FOOTER` that don't match any of the special strings
above are rendered directly, so you include your own raw HTML strings in
article footers.
