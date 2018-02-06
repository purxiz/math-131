# Render website locally

Install ruby `bundler` gem (only once_:

```
$ gem install bundler
```

From the root of the website, install dependencies:

```
$ bundle install
```

Generate the website (with live reload):

```
$ bundle exec jekyll liveserve
```

(open browser to `localhost:4000`)

# Markdown/Math issues.

Markdown is ill-suited to render math so you may encounter a couple of issues
with math. This is maily dues to the markdown processors (redcarpet, or
kramdown), that interprete `*`, and `_` as marker of italics, and/or bold font,
which braks math rendering. 

When using Kramdown, use `$$...$$` instead of `\[`, `\[` for block math delimiter, kramdown
will happily ignore what's inside the `$$`, and let mathjax do it's thing.

For inline math, some characters may need tobe escaped with an extra `\`, and
delimited need to be `\\(`, `\\)`. Pipes may need to be escaped: `\|` to avoid
creating markdown tables, this could be automatized via `sed`


If `*` is making issues add a trailing space.
