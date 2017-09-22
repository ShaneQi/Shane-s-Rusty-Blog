---
title: Setup a Ruby on Rails App in Subdirectory.
permalink: setup-a-ruby-on-rails-app-in-subdirectory-16-03-11
date: 2016-03-11 03:19
---


Let’s say my Ruby on Rails app is mounted in `mydomain.com/myapp` what should I do to make it works well is putting these two lines into `config/application.rb` inside `class Application :`

 config.action_controller.relative_url_root = '/myapp' Rails.application.routes.default_url_options[:original_script_name] = '/myapp'



