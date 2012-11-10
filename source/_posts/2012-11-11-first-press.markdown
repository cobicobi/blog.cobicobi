---
layout: post
title: "First Press"
date: 2012-11-11 01:13
comments: true
published: true
categories: [octopress]
---
I will share my experience setting up this blog using [Octopress](http://octopress.org/). Octopress is a blogging framework designed for [Jekyll](http://github.com/mojombo/jekyll), the blog aware static site generator. Why I'm using it? Well, I'm just curious about how it works. Plus, I like the idea of serving my blog as static files, which means I can host it anywhere. Basically, I was just followed this great little [docs](http://octopress.org/docs), and with a little bit changes here and there, this blog is already up and running. So, let's get started.

**Disclaimer**: This post is not intended as a complete guide to set up Octopress, you can refer to the official documentation for that. This post mainly address some issue I found when following the documentation, and how I work it around. And, I’m no Ruby guy, so don’t expect me to explain anything detailed about Ruby.

### Requirements
First thing first, you must install Ruby 1.9.3, at least that's what the docs stated, so I don't know if others version will work or not. There are many ways to install it, but I use [rbenv](http://rbenv.org). From what I understand, it's similar to Python's [virtualenv](http://www.virtualenv.org/en/latest/), which is to provide a way to easily install and switch between diferrent versions of Ruby. The guide from the [docs](http://octopress.org/docs/setup/rbenv/) itself is pretty straight forward, except I can't get it work after following all the steps. Something like "ruby 1.9.3-p194 is not installed" keeps appearing. I found out the solution by install the version 1.9.3-p194, whatever that version number means. Just execute these commands and you'll be all set.

{% codeblock %}
rbenv install 1.9.3-p194
rbenv rehash
{% endcodeblock %}

### Installing
After you get Ruby installed, the next step is setting up Octopress itself.

{% codeblock %}
git clone git://github.com/imathis/octopress.git octopress
cd octopress
ruby --version         # Should report Ruby 1.9.3
gem install bundler
rbenv rehash
bundle install         # Install dependencies
rake install           # Install the default Octopress theme
{% endcodeblock %}

Those steps will get you a copy of Octopress, and now you will need to configure it. Open up the `_config.yml` file. In `_config.yml` you must change the `url`, and you’ll probably want to change the `title`, `subtitle`, `author` and enable some 3rd party services. As you can see, I integrate my Twitter feed in this blog and this is simply done by enabling the 3rd party service.

### Adding Content
Done with all the configuration thingy? Now you can start adding the content. As stated in the [docs](http://octopress.org/docs/blogging/), to create a new post you can simply execute:

{% codeblock %}
rake new_post["title"]
{% endcodeblock %}

but if you use Linux like me (I use Ubuntu by the way), you will need to escape the '[' and ']', so instead you will need to execute:

{% codeblock %}
rake new_post\["title"\]
{% endcodeblock %}

This command will create a file named `title.markdown` in `source/_posts` folder. The filename will determine your url. With the default permalink settings the url would be something like `http://site.com/blog/2012/11/08/title/`. The created file is a [markdown](http://daringfireball.net/projects/markdown/). The documentation said there are more format supported other than markdown but I have no idea what it is.

After prepared the content, you can generate all the posts and pages into `public/` directory. The files in this directory is the only thing you need for deployment. You can also preview it first in a testing webserver using provided command.

{% codeblock %}
rake generate   # Generates posts and pages into the public directory
rake watch      # Watches source/ and sass/ for changes and regenerates
rake preview    # Watches, and mounts a webserver at http://localhost:4000
{% endcodeblock %}

### Deploying
Deploying is very easy. If you want to deploy using git based deployment workflow, the [docs](http://octopress.org/docs/deploying/) covers how to deploy into Heroku. I have tried it before and you can see the result in this link [http://cobipress.herokuapp.com](http://cobipress.herokuapp.com). For this blog, I simply upload the files in `public/` directory into my hosting server as the site is just a bunch of generated static files. 

### Using themes
Octopress provides theme support and adding more themes and installing it is a breeze. I use the [bootstrap-theme](https://github.com/bkutil/bootstrap-theme) with a bit modifications. Simply get the theme and copy into `.theme/` directory.

{% codeblock %}
git clone git://github.com/bkutil/bootstrap-theme.git bootstrap-theme
cp -R bootstrap-theme .themes/bootstrap
rake install['bootstrap']
rake generate
{% endcodeblock %}

### Updating Octopress
To keep Octopress up-to-date, you just need to pull from the master branch in the git repo:

{% codeblock %}
git pull octopress master     # Get the latest Octopress
bundle install                # Keep gems updated
rake update_source            # update the template's source
rake update_style             # update the template's style
{% endcodeblock %}

Read more in the [docs](http://octopress.org/docs/updating/).


Well, that's it. All in all, it was a good experience for me because I learned a couple of new things. Yay.
