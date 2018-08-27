---
layout: default
title: "Steps to Jekyll hosting on S3 and CloudFront with SSL with Codeship Deployment"
comments: true
date: 2018-08-25 16:40:09
---

The Steps I took to get a new Jekyll site running on Amazon S3
==============================================================

Here are the steps I went through to get a new / fresh blog up using Jekyll and deployed to Amazon S3 using Codeship with CloudFront and SSL.

Creating the Jekyll site
------------------------

It's probably best to follow the [Jekyll quick start quide](https://jekyllrb.com/docs/quickstart/), but here's basically what I went through...

* Create a GitHub repo (or something similar)
* clone the new repo to your local machine

      git clone https://github.com/<GITHUB USER>/<GITHUB REPO NAME>.git
      cd <GITHUB REPO NAME>
      rvm use 2.5.1
      gem install jekyll bundler
      jekyll new . --force
      bundle install
      jekyll serve

* A new Jekyll site should now be available on [http://localhost.com:4000](http://localhost.com:4000)

### Theme info ###

I was interested in trying out the new gem-based theme thing, so I followed [these instructions](https://github.com/yizeng/jekyll-theme-simple-texture):


Where the site files will live
------------------------------

1. Create a S3 bucket.
2. Setup **Static Web Hosting** on the new bucket
    * **index.html** as index document
    * **error.html** as error document

Setting up the domain
---------------------
