---
layout: single
title:  "Rails & Mongo without generators"
date:   2023-10-15 15:47:42 -0400
categories: software development
---
1. `gem install rails`
2. `rails new journey-app --skip-active-record`
3. create a file called mongoid.yml
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

4. created a filed named app/models/journey:
{% highlight ruby %}
class Path
  include Mongoid::Document
  include Mongoid::Timestamps
  field :title, type: String

  has_many :locations, dependent: :destroy
  has_many :comments, dependent: :destroy
end
{% endhighlight %}

5. created a filed named app/models/location:
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

6. created a filed named app/models/comment:
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

7. create a controller file at app/controllers/paths_controller.rb
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
