---
layout: single
title:  "Rails & Mongo API without generators"
date:   2023-12-04 15:47:42 -0400
categories: software development
---
1. `gem install rails`
2. `rails new journey-app --api --skip-active-record` if you're going to use another test suite such as RSpec add `--skip-test --skip-system-test`
3. add `gem 'mongoid'` to your gem file
4. run `bundle install` in your terminal
5. create a file called config/mongoid.yml:
  {% highlight yaml %}
  development:
    clients:
      default:
        database: journey-app
        hosts:
          - localhost:27017
        options:
          server_selection_timeout: 1
  {% endhighlight %}
6. you can now run your application server using `bin/rails s`
7. create a file named app/models/project:
```
  class Project
    include Mongoid::Document
    include Mongoid::Timestamps
    field :title, type: String
    field :description, type: String
  end
```
8. create a controller file at app/controllers/projects_controller.rb
{% highlight ruby %}
class Api::V1::ProjectsController < ApplicationController

  def index
    projects = {"name": "some project"}
    render json: projects, status: 200
  end

  def show
  end

  def create
  end
end
{% endhighlight %}

9. in config/routes.rb put
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :projects, only: [:index, :show, :create]
    end
  end
end

