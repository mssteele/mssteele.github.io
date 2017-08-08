---
layout: post
title:  "Creating a blog on GitHub Pages"
date:   2017-08-08 19:13:39
categories: blog docker
---
Since I've just set this blog up, the first post is about how I did it so I don't forget!

## GitHub Pages

This website is hosted on [GitHub Pages](https://pages.github.com/), a free static site hosting service. It serves files from a GitHub repository, so editing and publishing are easy if you know how to use Git. The repository must be named *username*.github.io (where username is your GitHub username) in order to identify it as the site for your account.

## Jekyll

As well as being able to serve static files, such as an index.html page, GitHub Pages also supports [Jekyll](https://jekyllrb.com/). Jekyll is a static site generator that uses Markdown as input, along with a templating language called Liquid. Basically Markdown is easier to edit than raw HTML, and having a templating language means you don't have to replicate content such as headers and footers across pages. Jekyll also has native support for blogging concepts such as posts.

### GitHub Integration

You can use Jekyll with GitHub Pages without having to install anything locally. GitHub provides a Jekyll theme chooser to get you started quickly. See the [docs](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/) for more info. Once set up, GitHub will regenerate the static files for your site each time you push changes to your repository. To add this blog post to my site I created a new markdown file called `2017-08-08-creating-a-blog-on-github.markdown` in the `_posts` directory of my repository on my local machine, then pushed it to GitHub. Jekyll is then run to create the new static blog post page, and also add a link to the home page.

There are many themes and plug-ins for Jekyll, and GitHub pages does not support them all. It is easier to start with a supported theme and then customise it as required. Using a supported theme means you can benefit from GitHub's automated build process. The alternative is to run Jekyll locally to build your site, and then push the full set of statically generated files to GitHub when updating. GitHub would then simply be serving static files, and in fact you could use this approach with any static file generator you choose.

I currently use the default, GitHub supported theme, [minima](https://github.com/jekyll/minima), but I also run Jekyll locally in order to preview my changes before pushing them to GitHub. I use Docker to make this process easier, which I will discuss later on.

### Jekyll Structure and Configuration

See [here](https://jekyllrb.com/docs/structure/) for an overview of the typical directory structure of a Jekyll site.

Site configuration is stored in `_config.yml`; properties in this file can then be accessed by the template files.

Blog posts are placed inside a `_posts` directory, html layout files for the template engine are beneath a `_layouts` directory, etc.

Folders relating to a site's theme are not actually present when you create a new Jekyll project, because the default files are stored outside of the project. To override a particular theme file you need to create a local copy in the correctly named folder. To obtain the default files, visit the GitHub repository of the them. For example the default them is [minima](https://github.com/jekyll/minima). The default page footer can be obtained [here](https://github.com/jekyll/minima/blob/master/_includes/footer.html). 


## Running Jekyll locally using Docker

As I mentioned earlier, it is useful to be able to run Jekyll locally to preview changes before pushing to GitHub. I run Windows and setting up Jekyll [doesn't look completely straightforward](https://jekyllrb.com/docs/windows/). I thought I'd give Docker a go as a way to speed up the process (and also I've been wanting an excuse to play about with Docker).

I am not going to cover Docker itself in detail here, I might do a separate post on that. 



*TODO: continue...* 




## Overview of steps I took to set up my site

TODO: check this and expand...

 - Create repo mssteele.... on GitHub
 - Clone locally
 - Run `Jekyll new` using a Docker image
 - Run `Jekyll ...` to build and serve the site locally
 - Commit changes locally, and push to GitHub. Make sure Git ignore file set up - don't actually push the _site directory - GitHub still does the Jekyll build

## Useful Links

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#blockquotes)