<!-- Next:
- write code & tests
- update dev-status in README & pyproject.toml + publish on Pypi + mention it on open-source.md blog page
  + document it on https://github.com/getpelican/pelican/wiki/Externally-hosted-plugins-and-tools & https://github.com/getpelican/pelican/wiki/Powered-by-Pelican
- relire: https://www.la-grange.net/2013/12/18/commentaire
-->
[![Pull Requests Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com)
[![TravisCI build](https://travis-ci.org/pelican-plugins/linkbacks.svg?branch=master)](https://travis-ci.org/pelican-plugins/linkbacks)
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)

[Pelican](https://getpelican.com) plugin implementing [Linkback](https://en.wikipedia.org/wiki/Linkback) protocols,
on the linking server side.

**Development status:** work-in-progress

Protocols currently implemented:
- [ ] XMLRPC Pingback: [protocol spec](http://www.hixie.ch/specs/pingback/pingback)
- [ ] [semantic RESTful Pingback](http://www.w3.org/wiki/Pingback)
- [ ] Trackback: [protocol spec](http://archive.cweiske.de/trackback/trackback-1.2.html)
- [x] [Webmention](https://indieweb.org/Webmention): [protocol spec](https://github.com/converspace/webmention)

❌ Refback: won't be implemented because it requires to retrieve the HTTP `Referer` header,
which cannot be done by Pelican, a static blog engine

❌ TalkBack: won't be implemented because it did not gain enough popularity / traction since its birth in 2011

Do not hesitate to suggest other protocols by submitting an _issue_.

## What are linkbacks?

> A linkback is a method for Web authors to obtain notifications when other authors link to one of their documents.
> This enables authors to keep track of who is linking to, or referring to, their articles.
> The four methods (Refback, Trackback, Pingback and Webmention) differ in how they accomplish this task.

I invite you to read this Wikipedia page for more information & links: [Linkback](https://en.wikipedia.org/wiki/Linkback)


## What does this plugin do?
For every hyperlink in your articles, this plugin will notify their hosting websites
(just those supporting a Linkback protocol) of those references.

This plugin **does not** perform inclusion of Linkbacks **in your articles / as comments**,
for every website referencing your content following a Linkback protocol,
because this cannot be performed by a static website generator like Pelican.


## Installation / setup instructions
To enable this plugin, `git clone` this repository and add the following to your `publishconf.py`:

    PLUGIN_PATH = 'path/to/this-repo'
    PLUGINS = ['linkbacks', ...]

`PLUGIN_PATH` can be a path relative to your settings file or an absolute path.

You will also need to install the Pypi dependencies listed in `pyproject.toml`:

    pip install poetry
    poetry install


### Cache
In order to avoid the repetitive CPU / bandwidth cost of repeatedly performing links parsing & linkback notifications,
this hook only proceed to do so once, the first time an article is published.

In order to do so, it uses a very simple and small cache that contains the list of all hyperlinks already parsed,
per article `slug`.


### Configuration
Available options:

- `LINKBACKS_CACHE_FILEPATH` (optional, default: relies on `appdirs.cache`, `~/.cache/pelican-plugin-linkbacks.json` under Unix) :
  the location of the JSON filepath serving as cache / state for this hook.
- `LINKBACKS_USERAGENT` (optional, default: `pelican-plugin-linkbacks`) :
  the `User-Agent` HTTP header to use while sending notifications.


## Contributing

Contributions are welcome and much appreciated. Every little bit helps. You can contribute by improving the documentation,
adding missing features, and fixing bugs. You can also help out by reviewing and commenting on [existing issues](https://github.com/pelican-plugins/linkbacks/issues).

To start contributing to this plugin, review the [Contributing to Pelican](https://docs.getpelican.com/en/latest/contribute.html) documentation,
beginning with the **Contributing Code** section.


## Linter & tests
To execute them:

    pylint *linkbacks.py
    pytest
