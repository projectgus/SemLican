# SemLican

A partial port of Matthias Pfefferle's [SemPress WordPress theme](https://github.com/pfefferle/SemPress) to the [Pelican static site generator](pelican.com). (*Naming things is hard*...)

## Support note

I did this for my blog https://projectgus.com, so the features it supports are generally the ones I am using. I'm interested in fixing bugs in the features that I use, and I'm happy to accept PRs for new features if they've been tested to work well. However in general **SemLican is unsupported and is probably not suitable for some use cases**.

## License

Like SemPress, SemLican is licensed under GPL 3.0 as per the license.txt file. SemPress is Copyright 2022 Matthias Pfefferle, and additions made in SemLican are Copyright 2022 Angus Gratton.

## Limitations

Compared to SemPress:

* The SemPress sidebar by default contains a search field and is otherwise fully configurable using WordPress Widgets. The SemLican sidebar is currently hard-coded to show five Recent Posts, and yearly archive links if `YEARLY_ARCHIVE_SAVE_AS` is enabled. It's possible to fork the theme and edit `templates/sidebar.html` to have whatever you want, of course.
* English translation only at this time
* Probably less Semantic microformats and microdata is included (PRs welcome to re-add the ones that I missed).
* The `archives.html` and `categories.html` list pages don't really have equivalents in WordPress. SemLican has some basic pages for these, but they're not well done.
* On filtered pages - i.e. "Period archive" pages (yearly, monthly, daily posts), Tag pages, and Category pages - there is no Archive section in the sidebar. This seems like a Pelican theme limitation, the `dates` object is already filtered to only include the articles that are shown on the page so there's nothing to generate the Archive from.
* No Category or Tag RSS/Atom feed URLs

Compared to most Pelican themes:

* The `authors.html` list page is not implemented.
* The `tags.html` list page is not implemented (individual tag URLs are implemented).
* If configured, "Period archives" pages (yearly, monthly, daily posts) show post summaries (like Wordpress) instead of a list of post titles (like Pelican)

## Required Plugins

Run `python -m pip install -r requirements.txt` to get all of these in one go.

* [neighbors plugin](https://github.com/pelican-plugins/neighbors)
* [webassets plugin](https://github.com/pelican-plugins/webassets)

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

  FEED_ALL_RSS = `feed.rss`
  FEED_ALL_ATOM = `/feed/atom.xml`  # TODO check these
  ```
* `YEAR_ARCHIVE_SAVE_AS` has to be set as shown above for the Archives in the sidebar to work
* `SEMLICAN_ISSO_DATA_URL` and `SEMLICAN_ISSO_SRC_URL` - If both are set then articles will include a comments section for the self-hosted comments software [Isso](https://github.com/posativ/isso/). `SEMLICAN_ISSO_DATA_URL` is the value for `data-isso=` and `SEMLICAN_ISSO_SRC_URL` is the value for `src=`, both in the [isso script tag](https://isso-comments.de/docs/reference/client-config/). CSS classes for Isso are provided already in the theme, and have been tweaked a little to look similar to SemPress (not the same).
* `SEMLICAN_HIDE_CATEGORIES` - if set to True, disable any "Posted in *category*" headers and footers on articles. Category pages are still generated but they're not linked from articles.
* `SEMLICAN_HIDE_AUTHOR` - if set to True, disable any byline of "by *author*" on articles. Author pages are still generated but they're not linked from articles.

On server:

* Ensure that the web server will search .html, .rss and .xml extensions as well as index.html, i.e
  - `/tag/name/` should serve `/tag/name.html` if it exists
  - `/tag/othername/` should serve `/tag/othername/index.html` if it exists
  - `/feed/` should server `/feed.rss` if it exists (assuming `FEED_ALL_RSS` set as shown above)
