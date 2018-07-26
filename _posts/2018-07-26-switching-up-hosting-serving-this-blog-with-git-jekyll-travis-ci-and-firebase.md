---
layout: post
title: Switching up hosting & serving this blog with Git, Jekyll, Travis-CI, and Firebase
category: blog, hosting, security, git, jekyll, travis-ci, firebase, https, google
---

So I am firing this blog back up now that I am no longer silenced by my employment. Since the blog will be mostly text based articles, I don't think using a VPS is a good idea for hosting as it costs too much, and also unnecessary for a static site using Jekyll generated content. Another reason to switch from using GitHub Pages is the lack of ***https*** support, as I want my visitors to this blog to know that they can read it in confidence that nothing has changed in transit between the hosting and their browser of choice.

In the years since I last posted, I have worked at NASA as a contractor and the work is very interesting and cool at the same time yet challenging. My wife and I now have a dog (Wirehaired Pointing Griffon) and a baby and really enjoy our time with both of them. I also will talk about my adventures in my Jeep here as well from time to time, outline the different projects I have done with it and along the journey to the places it takes us. Oh and the Washington Capitals have finally won a Stanley Cup!

Anyhow here's what I did as far as setting up this website with ***Jekyll, Git, Travis-CI, and Firebase***.

1.  Setup your hosting with Firebase at <https://firebase.google.com/>
2.  Download and install Jekyll on your local machine, here's a great place to get started, and Jekyll docs are very useful. <https://jekyllrb.com/>
3.  Setup a GitHub account at <https://github.com>, the repo can be public, there's no need for a private repo anyways because a website still needs to see it for hosting to work correctly. Also by using GitHub we are able to backup the site at GitHub and on the local machine.
4.  Setup a free account using your GitHub credentials at Travis-CI <https://travis-ci.org> and create a new build, turn on the repository you want to use Travis-CI to build & deploy from and use the following code in the config:

{% highlight travisci linenos %}
{
  "os": "linux",
  "dist": "trusty",
  "sudo": false,
  "group": "stable",
  "script": [
    "set -e",
    "jekyll build"
  ],
  "install": [
    "npm install -g firebase-tools"
  ],
  "node_js": "7",
  "language": "node_js",
  "global_env": "NOKOGIRI_USE_SYSTEM_LIBRARIES=true",
  "after_success": [
    "firebase deploy --token \"$FIREBASE_API_TOKEN\""
  ],
  "before_install": [
    "rvm install 2.3",
    "rvm use 2.3",
    ". $HOME/.nvm/nvm.sh && nvm install 6.1 && nvm use 6.1",
    "gem install bundler",
    "bundle check || bundle install"
  ]
}
{% endhighlight %}

5.  To maintain operational security with your build & deploy it is highly recommended that you set your API token key for **$FIREBASE_API_TOKEN** in an environment variable and do not check the box for the token to show in the output log.
6.  Create content for your site using Jekyll on your local machine and when you are ready to publish & deploy to the public interwebs, just do a simple git push to publish it.
