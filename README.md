# Initio theme for Hugo

![GitHub](https://img.shields.io/github/license/miguelsimoni/hugo-initio.svg?style=flat-square)
![GitHub top language](https://img.shields.io/github/languages/top/miguelsimoni/hugo-initio.svg?style=flat-square)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/miguelsimoni/hugo-initio.svg?style=flat-square)
![GitHub last commit (branch)](https://img.shields.io/github/last-commit/miguelsimoni/hugo-initio/master.svg?style=flat-square)
![GitHub closed issues](https://img.shields.io/github/issues-closed/miguelsimoni/hugo-initio.svg?style=flat-square)
![GitHub forks](https://img.shields.io/github/forks/miguelsimoni/hugo-initio.svg?style=flat-square)
![GitHub stars](https://img.shields.io/github/stars/miguelsimoni/hugo-initio.svg?style=flat-square)
![GitHub watchers](https://img.shields.io/github/watchers/miguelsimoni/hugo-initio.svg?style=flat-square)

[Hugo-Initio](https://miguelsimoni.github.io/hugo-initio-site/) is ported from the
[Initio](http://www.gettemplate.com/info/initio/) template by
[GetTemplate.com](http://www.gettemplate.com/) for [Hugo](https://gohugo.io/).

![screenshot](https://raw.githubusercontent.com/miguelsimoni/hugo-initio/master/images/tn.png)

## Original Template Info

**Licensing:** Creative Commons (for more options, go to the
[original template site](http://www.gettemplate.com/info/initio/))
**Released:** Feb 21, 2014
**Last Updated:** Feb 21, 2014
**Version:** 1.0
**Bootstrap:** 3.3.4 or higher
**Libraries:** jQuery
**Designer:** Sergey Pozhilov

## Installation

If you do not want to modify the theme and just pick up the latest changes then
run

```bash
cd /<your-hugo-site-directory>
git submodule add https://github.com/miguelsimoni/hugo-initio.git themes/hugo-initio
```

If you do want to make changes, then you should fork this and pull changes from
upstream as needed. This is more involved, but separates you theme changes from
the original fork

```bash
# Assuming that you have the github command line gh loaded
# See https://cli.github.com/manual/gh_repo_fork
gh repo fork https://github.com/miguelsimoni/hugo-initio --clone=false
cd _your-hugo-site-directory_
git submodule add git@github.com:<your org>/hugo-initio themes/hugo-initio
# now add this as upstream
cd themes/hugo-initio
git remote add upstream https://github.com/miguelsimoni/hugo-initio
```

More info: [hugo setup guide](https://gohugo.io/overview/installing/)

## Configuration and the Example Site

[Example Site](https://github.com/miguelsimoni/hugo-initio/tree/master/exampleSite)
is kept in a the directory. To use it, you need to
[copy](https://thenewstack.io/tutorial-use-hugo-to-generate-a-static-website/)
that up to the root

```bash
cp -rf ExampleSite/* ../../
```

But be careful this blows away the old config.toml file and add new content.

[config.toml](https://github.com/miguelsimoni/hugo-initio/tree/master/exampleSite/config.toml)
is the location for all configuration and described below

### Optional Sections

Hugo for noobs operates as a collection of partial templates, so these TOML
configurations enable different parts of the theme.

```toml
showSubheader = true
showServices = true
showRecentWorks = true
showDownload = true
showClients = true

footerEnableContact = true
footerEnableFollowme = true
footerEnableTextWidget = false
footerEnableFormWidget = false
```

### Social Networks Icons

You can add as many social networks as you want in the params.social array
following this template:

```toml
[[params.social]]
  title = "facebook"
  url = "https://www.facebook.com/nickname"
  icon = "fa-facebook-square"
  footer = true
  sharethis = true
  network = "facebook"
```

See the whole configuration in the
[config.toml](https://github.com/miguelsimoni/hugo-initio/tree/master/exampleSite/config.toml)
file.

### Comments

Powered by [Disqus](https://disqus.com) you can add your own comment sections.
You do need to login to Disqus and create one first.

```toml
[params.disqus]
    site = "your-disqus-short-name"
```

Disable the comments system by leaving the `params.disqus.site` empty.

### Google Analytics

Enable Google Analytics by entering your own tracker id.

```toml
[params.google.analytics]
    trackerID = "GA-000000000-0"
```

Disable the Google Analytics by leaving `params.google.analytics.trackerID` empty.

### Almost there

In order to see your site in action, you can run Hugo's built-in local server.

```bash
hugo server
```

## Netlify Hosting and Netlify CMS Integration

To use with [Netlify CMS](https://www.netlifycms.org/docs/add-to-your-site/),
follow the add to a site instructions by modifying the two files.

```bash
cd <your hugo root>
mkdir -p static/admin
cd static/admin
```

Then create an index.html for the admin page and the config.yaml and then the
config.yaml as described there.

The trickiest part is integration Netlify Identity, to do this, you need to
edit the [layouts/_default/baseof.html](layout/_default/baseof.html) this is part
of the complex look up set that Hugo uses and serves as the base. It is
complicated by [Michael Heap](https://michaelheap.com/creating-a-new-hugo-theme/)
gives a good overview of the build up of a page.

Note that with this template all the partial HTML pages are created by defining
functions like `main` and these are called by baseof.html

```bash
cd <your hugo root>
mkdir -p layouts/_defaults
cp themes/hugo-initiof/layouts/_defaults/baseof.html layouts/_defaults
```
Now you can add the Netlify Identity line in your own version of the
baseof.html

## Construction of partials in the theme (A Note)

Hugo is pretty hard to figure out because there are so many implicit lookups of
things. For instance just a lookup for the main index.html means dozens of
[Hugo Lookups](https://gohugo.io/templates/lookup-order/) but figuring this out
is critical to knowing where to place things.

As an example the base template for the home page. For a noob, the important
concepts are that there are markdown files that provide content, without any
HTML, these are then combined with a huge system of templates that show how to
lay things out. Every page has an output format, a suffix and a kind (normally
this is just the directory in which it lives), so when Hugo figures out how to
build the order of search is very important.

And with a theme, there are all the files in the main layout section which are
searched first and then the things in the themes.

That means in general, if you want to modify the layout, you should do it your
own layout and not in [layouts](layouts) in this theme, unless you want to push
those changes back to the upstream repo.

So if you want to make a local modification, copy the theme layout file and
then just put it up in the main website location and it should all work.

As an example if you want to integrate Netlify Identity into your site, then
you should

```bash
cd _your hugo site_
cp themes/hugo-initio/layouts/_default/baseof.html layouts/_default
```

Then you can edit your custom copy of baseof.html to your hearts content

Similarly, if for instance, you don't want to change baseof as that is too
much, you can take advantage of Partial Template [search](https://gohugo.io/templates/partials/)
which searchs both your local partials and the this theme. Again this means
that you don't have to modify the base theme which is great.

```bash
cd _your_hugo_site
mkdir -p layouts/partials/footer
cp themes/hugo-initio/layout/partial/footer/footer.html layouts/partial/footer```

And you can edit `footer.html` and it overrides the theme

## Deployment

There are quite a few hosting options available. The site does host properly on
Netlify.com. For other options checkout:

- [Hosting on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [More hosting and deployment options](https://gohugo.io/hosting-and-deployment/)

## Contributing

- Found a bug?
- Got an idea for a new feature?

Let me know it using the [issue tracker](https://github.com/miguelsimoni/hugo-initio/issues).
Or make it directly: [pull request](https://github.com/miguelsimoni/hugo-initio/pulls).

## License

This port is released under the MIT License. Check the
[original theme license](http://www.gettemplate.com/info/initio/)
for additional licensing information.

## Thanks

Thanks to [Steve Francia](https://github.com/spf13) for creating Hugo and the
awesome community around the project. And also thanks to [Sergey Pozhilov](http://www.gettemplate.com/)
for creating this awesome theme.
