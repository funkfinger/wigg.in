---
layout: default
title: Steps to Jekyll hosting on S3 and CloudFront with SSL with Codeship Deployment
comments: true
date: 2018-08-25 16:40:09 +0000

---
Here are the steps I went through to get a new / fresh blog up using Jekyll and deployed to Amazon S3 using Codeship with CloudFront and SSL.

Creating the Jekyll site
------------------------

It's probably best to follow the [Jekyll quick start quide](https://jekyllrb.com/docs/quickstart/), but here's basically what I went through...

* Create a GitHub repo (or something similar)
* Clone the new repo to your local machine and create the new Jekyll site framework. You should follow the above quick start guide, but these are the things I did on my OS X machine (that is already well setup for software development)...

      git clone https://github.com/<GITHUB USER>/<GITHUB REPO NAME>.git
      cd <GITHUB REPO NAME>
      rvm use 2.5.1
      gem install jekyll bundler
      jekyll new . --force
      bundle install
      jekyll serve

* A new Jekyll site should now be available on [http://localhost.com:4000](http://localhost.com:4000)
* do the following to push the fresh Jekyll site to GitHub

      git commit -am 'adding in the jekyll site'
      git push


Where the files will live
-------------------------

The site files will live in an **Amazon Simple Storage Service** bucket. This bucket will then be attached to **Amazon CloudFront** so that it can support SSL (as well as have global content distribution).

1. Create a S3 bucket:
    * use the bucket name `static.YOUR-DOMAIN.com` - this will be where the non-CloudFront-ed content will live.
2. Setup **Static Web Hosting** on the new bucket
    * `index.html` as index document
    * `error.html` as error document
3. Set the bucket permissions under `Permssions > Bucket Polciy` add this:

        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "PublicReadGetObject",
                    "Effect": "Allow",
                    "Principal": "*",
                    "Action": "s3:GetObject",
                    "Resource": "arn:aws:s3:::static.YOUR-DOMAIN.com/*"
                }
            ]
        }

Create an IAM user for S3 access
--------------------------------

* In the AWS management console, go to IAM and add a user.
* Name the user and give the user `Progmatic Acces`
* Use the `Attach existing policies` link and add `AmazonS3FullAccess`
* Save the `CSV` file that this will generate somewhere safe. The values will be needed in the deployment phase.

Setting up the domain
---------------------

In **Route 53** click `Create Record Set` and create an `A - IPv4 address` with a name of `static` and that is an **alias** to the `static.YOUR-DOMAIN.com` S3 bucket.

**EASY:** Using forestry.io to deploy
---------------------------

An easy way to now deploy the site is using [forestry.io](forestry.io) which is a tool to edit Jekyll (and Hugo) sites online. Behind the scenes you are just editing and commiting the post files to GitHub.

1. Import the GitHub repo to forestry.io
2. Under the `Settings` side menu, click on `Deployment` and select `Amazon S3` filling in the correct bucket name (`static.YOUR-DOMAIN.com`) and the `Access Key` and `Secret` from the `IAM` `CSV`
3. If all goes swimmingly, you should be able to make an edit using the forestry.io online editor and the site should get deployed to your S3 bucket. Which should be available at the domain name [http://static.YOUR-DOMAIN.com](http://static.YOUR-DOMAIN.com)