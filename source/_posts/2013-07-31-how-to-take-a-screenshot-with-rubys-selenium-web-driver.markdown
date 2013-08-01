---
layout: post
title: "How to take a screenshot with Ruby's Selenium Web Driver"
date: 2013-07-31 21:30
comments: true
categories: [ruby, selenium, testing]
---

Today I came across a new project called [Huxley](https://github.com/facebook/huxley) that is described as a _test-like system for catching visual regressions in Web applications_. Though I've seen many frameworks that either focus on the micro level unit testing or the macro level functional and integration tests, this is the first time I've seen a testing framework that focuses on catching visual regressions on the web. It's written in python and under the hood, is using the [Selenium WebDriver](http://docs.seleniumhq.org/projects/webdriver/) to handle browser automation and take screenshots.

To be honest, I didn't even realize that Selenium WebDriver could take screenshots. That said, I realized I had more to learn about it. After looking through the api docs, I found that taking a screenshot, is very easy. 

Check it out.

If you do not have [Selenium Server](http://docs.seleniumhq.org/) installed, do it now. If you're on a Mac, you can easily install it with [homebrew](https://github.com/mxcl/homebrew).

```
brew install selenium-server-standalone
```

If you do not have the [Selenium WebDriver](http://rubygems.org/gems/selenium-webdriver) gem installed, get that as well.
```
gem install selenium-webdriver
```

Now for the code to take the screenshot
```ruby
require 'selenium-webdriver'

# Other drivers are available as well http://selenium.googlecode.com/svn/trunk/docs/api/rb/Selenium/WebDriver.html#for-class_method
driver = Selenium::WebDriver.for :firefox
driver.navigate.to 'http://www.google.com'

driver.save_screenshot('screenshot.png')

driver.quit
```

Couldn't be any easier. I love finding new bits of code that are this easy to use. Needless to say, I'm going to be delving a bit deeper into this and see if I can come up with any new tools.
