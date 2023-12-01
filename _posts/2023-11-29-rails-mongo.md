---
layout: single
title:  "Rails & Mongo without generators"
date:   2023-10-15 15:47:42 -0400
categories: software development
---
1. `gem install rails`
2. `rails new journey-app --skip-active-record` if you're going to use another test suite such as RSpec add `--skip-test --skip-system-test`
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
7. create a file named app/models/journey:
```
  class Path
    include Mongoid::Document
    include Mongoid::Timestamps
    field :title, type: String

    has_many :locations, dependent: :destroy
    has_many :comments, dependent: :destroy
  end
```

8. create a file named app/models/location:
{% highlight ruby %}
class Location
  include Mongoid::Document
  include Mongoid::Timestamps
  field :name, type: String
  field :point, type: Point

  belongs_to :path
  has_many :comments, dependent: :destroy
end
{% endhighlight %}
9. create a file named app/models/comment:
{% highlight ruby %}
class Comment
  include Mongoid::Document
  include Mongoid::Timestamps
  field :message, type: String
  field :name, type: String

  belongs_to :path
  belongs_to: :location
end
{% endhighlight %}
10. create a controller file at app/controllers/paths_controller.rb
{% highlight ruby %}
class PathsController < ApplicationController

  def index
    @paths = Path.all
  end

  def new
    @path = Path.new
  end

  def show
  end

  def create
    @path = Path.new(path_params)
  end

  def destroy
    @path.destroy
  end
end
{% endhighlight %}
