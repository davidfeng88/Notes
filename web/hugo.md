# Hugo

## Basics

### Common commands

| Task | Command |
| :--- | :--- |
| Install Hugo via brew | `brew install hugo` |
| Verify that Hugo is installed | `which hugo` or `hugo version` |
| Create a new site | `hugo new site davidfeng.us` |
| Local preview at `localhost:1313` | `hugo server` \(add `-D` to include drafts\) |

### Use a theme

1. Select a theme from [Hugo Themes](https://themes.gohugo.io/).
2. Download the theme as a git submodule: `git submodule add https://github.com/Track3/hermit.git themes/hermit`
3. Copy `themes/hermit/exampleSite/config.toml` to the top level.

### Customize a theme

To customize a theme, e.g. you want to make some changes to a `config.toml`, an archetype, a template/layout, or a static file \(favicon\), either change the theme directly \(in the `themes/hermit` directory\), or copy the file you want to change to the same location on the top level and change it. Hugo will prioritize the files on the top level.

* In `config.toml`, I changed the following fields: baseURL, title, googleAnalytics, disqusShortname, author name, homeSubtitle, justifyContent, social icons' names and urls, menu items. Usually Hugo server needs to be restarted after modification of `config.toml`.
* In `archetypes`, I removed image, toc, tags, comments fields in the front matter.
* I created `layouts/shortcodes/pen.html` and copied a shortcode for codepen from [here](https://github.com/jorinvo/hugo-shortcodes/blob/master/shortcodes/pen.html).
* I generated favicon files [here](https://realfavicongenerator.net/) and put them under `static/`.

### Create content

Content lives in `content/` directory. There are two type of pages in Hugo: list page and single page. Usually directories correspond to list pages and individual markdown files are single pages.

#### Single pages

| Markdown file | Command to generate | URL |
| :--- | :--- | :--- |
| `content/a.md` | `hugo new a.md` | `localhost:1313/a/` |
| `content/a/index.md` | `hugo new a/index.md` | `localhost:1313/a/` |
| `content/dir1/dir2/b.md` | `hugo new dir1/dir2/b.md` | `localhost:1313/dir1/dir2/b/` |

The `hugo new path/file` command populates the markdown file according to the archetypes.

I usually use an index.md page as my single page so that I can put related assets \(e.g. images\) in the same directory.

#### List pages

If you create a file at `content/dir1/dir2/_index.html`, `localhost:1313/dir1/dir2/` will points to a HTML file generated based on the `_index.html`. `content/dir1/dir2/` becomes a section and the list page contains links to all pages in this section. It seems all pages in this section will NOT appear in the list page of the parent section anymore. Hugo automatically creates list page for top level directories in `content/`.

### Publish and Deploy

In each markdown file, remove the `draft: true` in the front matter to publish.

The `hugo` command will generates a `public/` folder, which you can put on the server. Don't forget to add `public/` to `.gitignore` file.

Alternatively, if you host your site on [Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/), when you push your changes to GitHub, Netlify will rebuild and publish your site.

## Advanced

### Template

To create a template is basically to create you own theme. You can either put the files in `themes/your_theme/layouts/`, or in the `layouts/` at top level which override the same file in the themes.

* Default template: the `list.html` and `single.html` in `layouts/_default/`.
* Homepage template: `layouts/index.html`.
* Section template: if you want to have a special template for the list pages in `posts/`, create a file at `layouts/posts/list.html` for it.

#### Base template

We can put common parts of `list.html` and `single.html` into a base template, i.e. `baseof.html`:

```markup
<!-- Common parts before "main" block -->
{{ block "main" . }}
<!-- The "main" block from single.html or list.html will be inserted here -->
{{ end }}
{{ block "footer" . }}
    This is the default baseof footer
{{ end }}
```

`single.html`:

```markup
{{ define "main" }}
    This is the single template
{{ end }}
<!-- We can optionally override the default baseof footer here -->
```

`list.html`:

```markup
{{ define "main" }}
    This is the list template
{{ end }}
```

#### Partial templates

`layouts/partials/`. For example, we can have a `header.html` file for header template.

```markup
<!-- layouts/partials/header.html -->
<h1>{{ .Title }}</h1>

<!-- layouts/_default/single.html -->
{{ partial "header" . }} // dot is the parameter passed into the partial
```

Custom variables:

```markup
<!-- layouts/partials/header.html -->
<h1>{{ .myVar }}</h1>

<!-- layouts/_default/single.html -->
{{ partial "header" (dict "myVar" "myValue" "myVar2" "myValue2") }} // we pass in a dict instead of dot
```

### [Hugo Variables](https://gohugo.io/variables/)

Only available in templates \(`layouts/`\), not available in `content/`.

General form: `{{ .Title }}`. Common variables: `.Date`, `.URL`, `.Content`.

#### Custom variables

* In front matter in markdown: `myVar: "myValue"`, you can access it in the template by `.Params.myVar`. If you did not define it, nothing shows up. No errors. For example:

```markup
<h1 style="color: {{ .Params.color }};">title</h1>
```

* In template: for example:

```markup
{{ $myVar := “aString” }} // define the value, could be string, number, or boolean
{{ $myVar }} // access the value
```

### [Hugo Functions](https://gohugo.io/functions/)

Again, functions are only available in `layouts/`, not in `content/`.

General form: `{{ funcName param1 param2 }}`. Examples:

```markup
{{ truncate 10 "this is a long string" }}
{{ singularize "dogs" }}

<!-- in list.html -->
{{ range .Pages }}
  <ul>
    <li><a href=“{{.URL}}”>{{ .Title }}</a></li> // here the Title is from each individual page
  </ul>
{{ end }}

<!-- conditions -->
{{ $var1 := “dog” }}
{{ $var2 := “cat” }}
{{ if eq $var1 $var2 }}
    // do something
{{else}}
    // do something
{{end}}

<!-- more complex conditions -->
{{ if and (…) (…) }}
    // do something
{{ end }}
```

A real example - highlight current page's title in a menu:

```markup
{{ $title := .Title }}
<ul>
{{ range .Site.Pages }}
    <li><a href="{{ .URL }}" style="{{ if eq .Title $title }} background-color: yellow;{{ end }}">{{ .Title }} </a></li>
</ul>
```

### Archetypes

Under `archetypes` directory, the `default.md` defines the front matter and common content of the markdown files when you create it via `hugo new`.

The `archetypes/dir1.md` file applies to new files created by `hugo new dir1/new-file.md`.

### Shortcodes

We can embed resources from other websites \(e.g. YouTube\) using shortcodes without writing a long iframe HTML code. For example, to embed a YouTube video at [https://www.youtube.com/watch?v=2xkNJL4gJ9E](https://www.youtube.com/watch?v=2xkNJL4gJ9E), All I need to do is to add `{{</* youtube 2xkNJL4gJ9E */>}}` in the markdown file. Hugo will expand that "shortcode" into the iframe HTML code. 

Hugo has built-in shortcodes for Twitter, Instagram, YouTube, Vimeo, etc. See [the official doc](https://gohugo.io/content-management/shortcodes/) for more.

#### [Custom shortcode / shortcode template](https://gohugo.io/templates/shortcode-templates/)

Another way of thinking shortcodes is to treat them as functions \(in the context of programming\). The general form of shortcode is:

```markup
{{</* my-shortcode params */>}}
```

which is basically just

```go
myFunc(params)
```

To use the custom shortcode \(call the function\) in the markdown files in `content/`, we need to add a shortcode template in `layouts/shortcodes/shortcode-name.html` \(define the function\).

In the markdown file, a shortcode can take in positional \(e.g. `{{</* youtube 2xkNJL4gJ9E */>}}`\) or named parameters \(e.g. `{{</* img src="a.png" */>}}`\).

In the template, use `{{.Get 0}}` or `{{.Get "src"}}` to get the passed in values.

In the following case, the template cannot use quotes to wrap color, so \` is used instead.

```markup
<p style="color:{{.Get `color`}}">Text Text</p>
```

#### Double tag shortcode

Use a double tag shortcode if you want to pass in a large block of text.

In the markdown file, call it as follows:

```markup
{{</* my-shortcode */>}}
    This is some text
{{</* /my-shortcode */>}}
```

In the template, access the text between the opening and closing tags via `{{.Inner}}`.

If the inner text needs further markdown processing \(e.g. `**bold**`\), call the shortcode with % % instead of &lt; &gt;:

```markup
{{%/* my-shortcode */%}}
    **bold**
{{%/* /my-shortcode */%}}
```

### Taxonomies

Pages in `content/` can be grouped by taxonomies. The default two taxonomies are tags and categories. In `config.toml`, add the following lines:

```text
[taxonomies]
  tag = "tags"
  category = "categories"
```

To disable category, use `category = ""`. To add a custom taxomony \(e.g. mood\), add `mood = "moods"`.

In content markdown files, the front matters can contain taxonomy information:

```yaml
tags: ["tag1", "tag2"]
categories: [“cat1”]
```

In the single page, `tag1` links to `localhost:1313/tags/tag1/`, which is a list page that shows all single pages with `tag1` as one of their tags.

### Data Files

`data/` directory: we can put json, yaml, and toml files. For example, we can have a `states.json`, which contains an array of "state" objects, which have a "name" field. and in templates:

```markup
{{ range .Site.Data.states }}
  {{ .name }}<br>
{{ end }}
```

## Recommended Resources

* [Giraffe Academy Hugo Tutorial](https://www.youtube.com/watch?v=qtIqKaDlqXo&list=PLLAZ4kZ9dFpOnyRlyS-liKL5ReHDcj4G3)

