---
layout: post
title: Creating a blog with jekyll
tag: jekyll
date: 2021-05-27 02:28 -0500
---
I like to keep clean my machine, so i try to use docker for every case. 

Jekyll is not an exception.

I found that a single docker-compose file works very well and avoid the installation of Ruby & Gem.

{% highlight docker %}
version: '3'

services:
    jekyll:
        image: jekyll/jekyll:latest
        command: jekyll serve --watch --force_polling --draft
        ports:
          - 4000:4000
        volumes:
          - .:/srv/jekyll
{% endhighlight %}

This docker-compose.yml takes charge of:

- Download and start the lastest jekyll image
- Execute jekyll watching the changes in your folder, also it will shows the draft post.
- Bind the default jekyll port to the port 4000 in the host
- Mount the current folder to /srv/jekyll in the container

This file will take care of Jekyll, but we need to start our jekyll project.

To start a new project, all you need to do is add this docker-componse.yml to an empty directory and run `docker-compose run jekyll /bin/bash` from the command line. This will give you access to the command line in the container. 

In the container you need to execute `jekyll new . --force`, the --force param is need it because jekyll ask for an empty folder to start a project (our folder already contains the docker-compose.yml).

When finish the execution, you are ready to start using jekyll, all you need to do is run `docker-compose up` and open your browser in [localhost:4000] localhost:4000

# jekyll-compose

Jekyll-compose give you the capability of create new post/draft via the command line.

The installation is very easy even that we are working in a docker image. You only need to add this line at the end of your Gemfile

    gem 'jekyll-compose', group: [:jekyll_plugins]

And then connect to the container to execute:

    $ bundle


You can do this direct from the host using

    $ docker-compose run jekyll bundle

Then you can create a post using:

```sh
    # In the container
    $ bundle exec jekyll post "My New Post"
    # In the host
    $ docker-compose run jekyll bundle exec jekyll post "My New Post"
```


# Related links

https://pages.github.com/ : Publish your jekyll project in github

https://www.markdownguide.org/basic-syntax : Guide for use markdown

https://github.com/jekyll/jekyll-compose : Jekyll-compose site