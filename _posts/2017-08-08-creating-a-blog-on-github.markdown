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

## Using Jekyll with GitHub Pages

You can use Jekyll with GitHub Pages without having to install anything locally. GitHub provides a Jekyll theme chooser to get you started quickly. See the [docs](https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/) for more info. Once set up, GitHub will regenerate the static files for your site each time you push changes to your repository. To add this blog post to my site I created a new markdown file called `2017-08-08-creating-a-blog-on-github.markdown` in the `_posts` directory of my repository on my local machine, then pushed it to GitHub. Jekyll is then run to create the new static blog post page, and also add a link to the home page.

There are many themes and plug-ins for Jekyll, and GitHub pages does not support them all. It is easier to start with a supported theme and then customise it as required. Using a supported theme means you can benefit from GitHub's automated build process. The alternative is to run Jekyll locally to build your site, and then push the full set of statically generated files to GitHub when updating. GitHub would then simply be serving static files, and in fact you could use this approach with any static file generator you choose.

I currently use the default, GitHub supported theme, [minima](https://github.com/jekyll/minima), but I also run Jekyll locally in order to preview my changes before pushing them to GitHub. I use Docker to make this process easier, which I will discuss later on.

## Jekyll Structure and Configuration

See [here](https://jekyllrb.com/docs/structure/) for an overview of the typical directory structure of a Jekyll site.

Site configuration is stored in `_config.yml`; properties in this file can then be accessed by the template files.

Blog posts are placed inside a `_posts` directory, html layout files for the template engine are beneath a `_layouts` directory, etc.

Folders relating to a site's theme are not actually present when you create a new Jekyll project, because the default files are stored outside of the project. To override a particular theme file you need to create a local copy in the correctly named folder. To obtain the default files, visit the GitHub repository of the them. For example the default them is [minima](https://github.com/jekyll/minima). The default page footer can be obtained [here](https://github.com/jekyll/minima/blob/master/_includes/footer.html). 


## Running Jekyll Locally

As I mentioned earlier, it is useful to be able to run Jekyll locally to preview changes before pushing to GitHub. 

Once you have Jekyll installed there are only a couple of commands you need:

1. Create a new Jekyll project in the current directory using:

```sh
jekyll new .
```

2. Build the project, serve it up, and watch for file changes using: 

```sh
jekyll serve --force_polling --incremental
```

The site is then available to view at `http://localhost:4000/`

## Running Jekyll using Docker

I use Windows and setting up Jekyll [doesn't look completely straightforward](https://jekyllrb.com/docs/windows/). I thought I'd give Docker a go as a way to speed up the process (and also I've been wanting an excuse to play about with Docker for a while).

I am not going to cover Docker itself in detail here, I might do a separate post on that. Install [Docker for Windows](https://docs.docker.com/docker-for-windows/install/) if you don't have it, and make sure it is running.

### Create a new Jekyll project

To create a new Jekyll project in a new subdirectory of the current directory, create a file named `docker-compose.yml` with the following contents:

```yml
jekyll:
    image: jekyll/jekyll:pages
    command: jekyll new ./MyJekyllProject
    ports:
        - 4000:4000
    volumes:
        - .:/srv/jekyll
```

Then run this from the command line using:

```sh
docker-compose up
```

The new Jekyll project is created in the `MyJekyllProject` subdirectory.

Ensure a .gitignore file is created. This should contain an entry to exclude the `_site/` directory. This directory will contain the locally built site, and should not be pushed to GitHub as that will build the site itself.

### Serve a Jekyll site

To serve up the Jekyll site (and watch for changes), create a new `docker-compose.yml` file in the same directory as the newly created Jekyll project, with the following contents:

```yml
jekyll:
    image: jekyll/jekyll:pages
    command: jekyll serve --force_polling --incremental
    ports:
        - 4000:4000
    volumes:
        - .:/srv/jekyll
```

(I needed to add the `--force_polling` option to get the file watching working. It looks like this is only necessary when running on Windows.)
Then run this from the command line using:

```sh
docker-compose up
```

The site should now be available to view at `http://localhost:4000/`

## Steps I took to set up my site

The full list of steps I took to set up my blog are:

1. Created an empty repository on GitHub named `mssteele.github.io`
1. Cloned the repository to my local computer (it's empty, but this sets up the GitHub remote)
1. Created a new Jekyll site in a temporary directory by running `jekyll new` under Docker as described above
1. Copied the new Jekyll site into my empty local repository
1. Tested this locally using `jekyll serve` under Docker, as described above
1. Committed the local changes and pushed to GitHub
1. Customised the site a bit, added this blog post, pushed the changes, etc!


## Useful Links

[Creating and Hosting a Personal Site on GitHub](http://jmcglone.com/guides/github-pages/)

[Running Jekyll locally with Docker](https://kristofclaes.github.io/2016/06/19/running-jekyll-locally-with-docker/)

[Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)