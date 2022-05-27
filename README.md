# SemLican

An incomplete port of the [SemPress](https://github.com/pfefferle/SemPress) WordPress theme to the [Pelican static site generator](pelican.com).

## Support note

I did this for my blog https://projectgus.com so the features it supports are generally the ones I wanted. I'm interested in fixing bugs in the features I sue, and I'm happy to accept PRs for new features if they've been tested to work well, but in general **SemLican is unsupported and is suitable for all use cases**.

## Limitations

Compared to SemPress:

* The SemPress sidebar by default contains a search field and is otherwise fully configurable using WordPress Widgets. The SemLican sidebar is currently hard-coded to show five Recent Posts, and yearly archive links if ``YEARLY_ARCHIVE_SAVE_AS` is enabled. It's possible to fork the theme and edit `templates/sidebar.html` to have whatever you want, of course.
* English translation only at this time
* Probably less Semantic microformats and microdata is included (PRs welcome to re-add the ones that I missed).
* The `archives.html` and `categories.html` list pages don't really have equivalents in WordPress. Semlican has some basic pages for these, but they're not well done.
* On filtered pages - "Period archive" pages (yearly, monthly, daily posts), Tag pages, and Category pages - there is no Archive section in the sidebar (seems like a Pelican limitation, `dates` object is already filtered to only have what is shown on the page.)


Compared to other Pelican themes:

* The `authors.html` list page is not implemented.
* The `tags.html` list page is not implemented (individual tag URLs are implemented).
* If configured, "Period archives" pages (yearly, monthly, daily posts) show post summaries (like Wordpress) instead of a list of post titles (like Pelican)


## Configuration

Some of these settings are only necessary if migrating from Wordpress and trying not to break any inbound URLs.

In `pelicanconf.py`:

* Set `DEFAULT_DATE_FORMAT` if you want dates to match WordPress ones. I use `DEFAULT_DATE_FORMAT = '%B %d, %Y'`
* Set URLs to match Wordpress URLs, i.e.
  ```
  ARTICLE_URL = '{date:%Y}/{date:%m}/{slug}/'
  ARTICLE_SAVE_AS = '{date:%Y}/{date:%m}/{slug}/index.html'
  PAGE_URL = 'pages/{slug}/'
  PAGE_SAVE_AS = 'pages/{slug}/index.html'

  YEAR_ARCHIVE_SAVE_AS = '{date:%Y}/index.html'
  MONTH_ARCHIVE_SAVE_AS = '{date:%Y}/{date:%m}/index.html'
  ```
* `YEAR_ARCHIVE_SAVE_AS` has to be set as shown above for the Archives in the sidebar to work

On server:

* Ensure sure the web server will search .html extensions, i.e `/tag/name/` should serve `/tag/name.html` as well as `/tag/name/index.html`

## Recommended/Supported Plugins

* [neighbors plugin](https://github.com/pelican-plugins/neighbors) `python -m pip install pelican-neighbors`
