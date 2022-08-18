---
layout: post
title:  "Building this site with Jekyll and GitHub Pages"
date:   2022-08-17 09:02:32 -0400
categories: jekyll
---
## How this site was born
I have a friend who wants to build a personal blog site and learn something about web development along the way. A static site would definitely suit his needs, so it was a great opportunity to try GitHub Pages and Jekyll.

I followed [the GitHub Pages documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll), to set up this site. It mostly worked as expected, but I did experience a few errors along the way. 

## Troubleshooting 
1. When installing Jekyll via `gem install jekyll`, I got the following error:
```
current directory: /Users/bencoomes/.rvm/gems/ruby-3.0.2/gems/eventmachine-1.2.7/ext
make DESTDIR\=
compiling binder.cpp
In file included from binder.cpp:20:
./project.h:116:10: fatal error: 'openssl/ssl.h' file not found
#include <openssl/ssl.h>
```
This [Stackoverflow post](https://stackoverflow.com/questions/68794527/rubygems-error-while-installing-jekyll-openssl-ssl-h-file-not-found) contains the solution: 
```
brew link --force opensll
```
then
```
gem install eventmachine -- --with-cppflags=-I/usr/local/opt/openssl/include
```
After running those commands, I was able to run `gem install jekyll` successfully.
1. When trying to host my site locally via `bundle exec jekyll server`, I got this error:
```
/Users/bencoomes/.rvm/gems/ruby-3.1.2/gems/jekyll-3.9.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
``` 
The [solution is to include `webrick` in the Gemfile](https://github.com/jekyll/jekyll/issues/8523) because `webrick` isn't included in Ruby 3.0 and later.