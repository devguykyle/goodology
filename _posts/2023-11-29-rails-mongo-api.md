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




Image Upload - Text directions and code
Add the following gems to your gemfile:

gem 'carrierwave'

gem 'mini_magick'

gem 'fog'

Then run bundle install --without production

Generate the Image resource:

rails generate scaffold Image name:string picture:string user:references

Run the migration to create the images table:

rake db:migrate

Add styling to all image views:

rails g bootstrap:themed Images

Enter Y through all the questions to override the existing image views

Open your user.rb model file under app/models and enter in the following line:

has_many :images

Ensure your image.rb model file has the line belongs_to :user

To generate an uploader:

rails generate uploader Picture

Add the following line to your image.rb model file to associate images with picture:

mount_uploader :picture, PictureUploader

Pull up the _form.html.erb partial under app/views/images folder and update it to make it look like below:

<%= form_for @image, :html => { multipart: true, :class => "form-horizontal image" } do |f| %>

<% if @image.errors.any? %>

<div id="error_expl" class="panel panel-danger">

<div class="panel-heading">

<h3 class="panel-title"><%= pluralize(@image.errors.count, "error") %> prohibited this image from being saved:</h3>

</div>

<div class="panel-body">

<ul>

<% @image.errors.full_messages.each do |msg| %>

<li><%= msg %></li>

<% end %>

</ul>

</div>

</div>

<% end %>

<div class="control-group">

<%= f.label :name, :class => 'control-label' %>

<div class="controls">

<%= f.text_field :name, :class => 'form-control' %>

</div>

<%= error_span(@image[:name]) %>

</div>

<div class="control-group">

<%= f.label :picture, :class => 'control-label' %>

<div class="controls">

<%= f.file_field :picture, accept: 'image/jpeg,image/gif,image/png' %>

</div>

<%= error_span(@image[:picture]) %>

</div>

<%= f.submit nil, :class => 'btn btn-primary' %>

<%= link_to t('.cancel', :default => t("helpers.links.cancel")),

images_path, :class => 'btn btn-default' %>

<% end %>

Update your create action in the images_controller.rb file under app/controllers folder by adding the line below under @image = Image.new...:

@image.user = current_user

To display the image in the show page, open the show.html.erb file under app/views/images folder and update it as follows:

<%- model_class = Image -%>

<div class="page-header">

<h1><%=t '.title', :default => model_class.model_name.human.titleize %></h1>

</div>

<dl class="dl-horizontal">

<dt><strong><%= model_class.human_attribute_name(:name) %>:</strong></dt>

<dd><%= @image.name %></dd>

<dt></dt>

<dd><%= image_tag(@image.picture.url, size: "300x300") if @image.picture? %></dd>

</dl>

<%= link_to t('.back', :default => t("helpers.links.back")),

images_path, :class => 'btn btn-default' %>

<%= link_to t('.edit', :default => t("helpers.links.edit")),

edit_image_path(@image), :class => 'btn btn-default' %>

<%= link_to t('.destroy', :default => t("helpers.links.destroy")),

image_path(@image),

:method => 'delete',

:data => { :confirm => t('.confirm', :default => t("helpers.links.confirm", :default => 'Are you sure?')) },

:class => 'btn btn-danger' %>




____________________________________________________________

Stripe for Payment Introduction - Text directions, references and code
Guide for basic rails implementation of Stripe checkout:

https://stripe.com/docs/checkout/guides/rails

Note down your test secrety key and your test publishable key

In your gemfile add the stripe gem:

gem 'stripe'

Then bundle install --without production

Create a file named stripe.rb within your config/initializers folder and fill it in:

Rails.configuration.stripe = {

:publishable_key => ENV['STRIPE_TEST_PUBLISHABLE_KEY'],

:secret_key => ENV['STRIPE_TEST_SECRET_KEY']

}

Stripe.api_key = Rails.configuration.stripe[:secret_key]

Open your .zshrc file and fill in

export STRIPE_TEST_SECRET_KEY=yoursecrettestkeyfromstripe

export STRIPE_TEST_PUBLISHABLE_KEY=yourpublishabletestkeyfromstripe

Open a new terminal window for these to take effect

Now set these for heroku from your terminal window type in:

heroku config:set STRIPE_TEST_SECRET_KEY=yoursecrettestkey

heroku config:set STRIPE_TEST_PUBLISHABLE_KEY=yourpublishabletestkey

__________________________________________________________________________

Payment Model - Text directions and code
Create a payment model:

rails generate model Payment email:string token:string user_id:integer

Run the migration file to create the payments table:

rake db:migrate

In your user.rb model file under app/models folder enter in the following code:

has_one :payment

accepts_nested_attributes_for :payment

In your payment.rb model file under app/models folder enter in the following code for attr_accessors and methods:

class Payment < ActiveRecord::Base

attr_accessor :card_number, :card_cvv, :card_expires_month, :card_expires_year

belongs_to :user

def self.month_options

Date::MONTHNAMES.compact.each_with_index.map { |name, i| ["#{i+1} - #{name}", i+1]}

end

def self.year_options

(Date.today.year..(Date.today.year+10)).to_a

end

def process_payment

customer = Stripe::Customer.create email: email, card: token

Stripe::Charge.create customer: customer.id,

amount: 1000,

description: 'Premium',

currency: 'usd'

end

end


___________________________________________________________________________

Image Size Validations - Text directions and code
In app/uploaders/picture_uploader.rb file uncomment the following lines:

def extension_white_list

%w(jpg jpeg gif png)

end

Add the following to app/models/image.rb file:

validate :picture_size

private

def picture_size

if picture.size > 5.megabytes

errors.add(:picture, "should be less than 5MB")

end

end

In the app/views/images/_form.html.erb partial add the following validation at the bottom:

<script type="text/javascript">

$('#image_picture').bind('change', function() {

var size_in_megabytes = this.files[0].size/1024/1024;

if (size_in_megabytes > 5) {

alert('Maximum file size is 5MB.');

}

});

</script>

To install latest imagemagick:

sudo apt-get install imagemagick --fix-missing

Open your picture_uploader.rb file within the app/uploaders folder and add the following two lines right below the class definition:

include CarrierWave::MiniMagick

process resize_to_limit: [300, 300]

Update the app/views/images/index.html.erb file and make it look like below:

<%- model_class = Image -%>

<div class="page-header">

<h1><%=t '.title', :default => model_class.model_name.human.pluralize.titleize %></h1>

</div>

<table class="table table-striped">

<thead>

<tr>

<th><%= model_class.human_attribute_name(:name) %></th>

<th><%= model_class.human_attribute_name(:picture) %></th>

<th><%=t '.actions', :default => t("helpers.actions") %></th>

</tr>

</thead>

<tbody>

<% @images.each do |image| %>

<tr>

<td><%= link_to image.name, image_path(image) %></td>

<td><%= image_tag image.picture.url, size: "100x100" %></td>

<td>

<%= link_to t('.edit', :default => t("helpers.links.edit")),

edit_image_path(image), :class => 'btn btn-default btn-xs' %>

<%= link_to t('.destroy', :default => t("helpers.links.destroy")),

image_path(image),

:method => :delete,

:data => { :confirm => t('.confirm', :default => t("helpers.links.confirm", :default => 'Are you sure?')) },

:class => 'btn btn-xs btn-danger' %>

</td>

</tr>

<% end %>

</tbody>

</table>

<%= link_to t('.new', :default => t("helpers.links.new")),

new_image_path,

:class => 'btn btn-primary' %>

___________________________________________________________

Sending Email in Production - Text directions and code
First add in your credit card details to your heroku account

Then enter in:

heroku addons:create sendgrid:starter

Set the sendgrid apikey credentials you created for heroku:

heroku config:set SENDGRID_USERNAME=apikey

heroku config:set SENDGRID_PASSWORD=enterintheapikey

To display your settings you can type in:

heroku config:get SENDGRID_USERNAME

Open your .profile file and enter in the following as well, if using a different environment, check discussions/google/docs or your environment's settings help/doc for more info about where you can set this:

export SENDGRID_USERNAME=apikey

export SENDGRID_PASSWORD=entireapikey

Then open a new terminal window for these to take effect

Under config/environment.rb file add in the following code at the bottom:

ActionMailer::Base.smtp_settings = {

:address => 'smtp.sendgrid.net',

:port => '587',

:authentication => :plain,

:user_name => ENV['SENDGRID_USERNAME'],

:password => ENV['SENDGRID_PASSWORD'],

:domain => 'heroku.com',

:enable_starttls_auto => true

}

Now update the development.rb file under config/environments folder and add the following two lines:

config.action_mailer.delivery_method = :test

config.action_mailer.default_url_options = { :host => 'http://previewurlforyourapp'}

My preview url looks like this: http://ruby-on-rails-123170.nitrousapp.com:3000

Now update the production.rb file under config/environments folder and add the following two lines:

config.action_mailer.delivery_method = :smtp

config.action_mailer.default_url_options = { :host => 'yourherokuappname.herokuapp.com', :protocol => 'https'}

Test it out in development by signing up a user and then grabbing the confirmation link from the web output in your terminal and copying/pasting the link in your browser

________________________________________________________________________

Setup Authentication System - Text directions and code
First add the following gems to the gemfile:

gem 'devise'

gem 'twitter-bootstrap-rails'

gem 'devise-bootstrap-views'

If using Rails 5, also add gem 'jquery-rails'

Then run bundle install --without production

Then install devise:

rails generate devise:install

rails generate devise User

Pull up the migration file that just got created and uncomment the 4 lines under confirmable:

t.string :confirmation_token

t.datetime :confirmed_at

t.datetime :confirmation_sent_at

t.string :unconfirmed_email

Pull up the user.rb model file under app/models and in the line for devise, add in a:

:confirmable,

after :registerable, entry

Run your migration now to create the users table:

rake db:migrate (rails db:migrate if using Rails 5)

In your application_controller.rb file under app/controllers add in:

before_action :authenticate_user!

In your welcome_controller.rb file under app/controllers add in:

skip_before_action :authenticate_user!, only: [:index]

Run the following generators to install bootstrap themed styling:

rails generate bootstrap:install static

rails generate bootstrap:layout application # select Y to force override after hitting enter

rails generate devise:views:locale en

rails generate devise:views:bootstrap_templates

In the application.css file under app/assets/stylesheets folder, right above the line that says *= require_tree add in the following line:

*= require devise_bootstrap_views

If using Rails 5, you need to perform the following two steps:

- Remove the favicon link tags from application.html.erb layout file (there should be 5)

- In the application.js file under app/assets/javascripts folder, add the line //= require jquery to make it look like below:

//= require rails-ujs
//= require jquery
//= require twitter/bootstrap
//= require turbolinks
//= require_tree .

______________________________________________________________________________________

Update Form for Credit Card Payments - Text directions and code
In the app/views/devise/registrations folder, open the new.html.erb file and add this code below to the section right under the closing </div> below the password confirmation, if using a new account from stripe, below where it says data: { stripe: 'cvv' }, switch the cvv with cvc to make it look like data: { stripe: 'cvc' }:

<%= fields_for( :payment ) do |p| %>

<div class="row col-md-12">

<div class="form-group col-md-4 no-left-padding">

<%= p.label :card_number, "Card Number", data: { stripe: 'label'} %>

<%= p.text_field :card_number, class: "form-control", required: true, data: { stripe: 'number'} %>

</div>

<div class="form-group col-md-2">

<%= p.label :card_cvv, "Card CVV", data: { stripe: 'label'} %>

<%= p.text_field :card_cvv, class: "form-control", required: true, data: { stripe: 'cvv'} %>

</div>

<div class="form-group col-md-6">

<div class="col-md-12">

<%= p.label :card_expires, "Card Expires", data: { stripe: 'label'} %>

</div>

<div class="col-md-3">

<%= p.select :card_expires_month, options_for_select(Payment.month_options),

{ include_blank: 'Month' },

"data-stripe" => "exp-month",

class: "form-control", required: true %>

</div>

<div class="col-md-3">

<%= p.select :card_expires_year, options_for_select(Payment.year_options.push),

{ include_blank: 'Year' },

class: "form-control",

data: { stripe: "exp-year" }, required: true %>

</div>

</div>

</div>

<% end %>

_________________________________________________________________________

Javascript Events - Text directions and code
Go to your application.html.erb file under app/views/layouts folder and right above the line for <%= javascript_include_tag "application" %> enter in the following:

<%= javascript_include_tag "https://js.stripe.com/v2/" %>

Go to new.html.erb file under app/views/devise/registrations folder and on top of the file add in the following code:

<script language="Javascript">

Stripe.setPublishableKey("<%= ENV['STRIPE_TEST_PUBLISHABLE_KEY'] %>");

</script>

In the <%= form_for line add a class of 'cc_form' and make it look like below:

<%= form_for(resource, :as => resource_name, :url => registration_path(resource_name), html: { role: "form", class: 'cc_form' }) do |f| %>

Under app/assets/javascripts folder, create a file called credit_card_form.js and fill it in with the following code:

$(document).on('ready turbolinks:load', function() {

var show_error, stripeResponseHandler, submitHandler;

submitHandler = function (event) {

var $form = $(event.target);

$form.find("input[type=submit]").prop("disabled", true);

//If Stripe was initialized correctly this will create a token using the credit card info

if(Stripe){

Stripe.card.createToken($form, stripeResponseHandler);

} else {

show_error("Failed to load credit card processing functionality. Please reload this page in your browser.")

}

return false;

};

$(".cc_form").on('submit', submitHandler);

stripeResponseHandler = function (status, response) {

var token, $form;

$form = $('.cc_form');

if (response.error) {

console.log(response.error.message);

show_error(response.error.message);

$form.find("input[type=submit]").prop("disabled", false);

} else {

token = response.id;

$form.append($("<input type=\"hidden\" name=\"payment[token]\" />").val(token));

$("[data-stripe=number]").remove();

$("[data-stripe=cvv]").remove();

$("[data-stripe=exp-year]").remove();

$("[data-stripe=exp-month]").remove();

$("[data-stripe=label]").remove();

$form.get(0).submit();

}

return false;

};

show_error = function (message) {

if($("#flash-messages").size() < 1){

$('div.container.main div:first').prepend("<div id='flash-messages'></div>")

}

$("#flash-messages").html('<div class="alert alert-warning"><a class="close" data-dismiss="alert">×</a><div id="flash_alert">' + message + '</div></div>');

$('.alert').delay(5000).fadeOut(3000);

return false;

};

});

Note above where we wrote $("[data-stripe=cvv]").remove();, if using a new account in stripe, change the cvv here with cvc, so data-stripe=cvc

__________________________________________________________________________

Extend Devise Registrations Controller - Text directions, references and code
List of test credit cards for stripe link:

https://stripe.com/docs/testing

Create a registrations_controller.rb file under app/controllers folder and fill it in:

class RegistrationsController < Devise::RegistrationsController

def create

build_resource(sign_up_params)

resource.class.transaction do

resource.save

yield resource if block_given?

if resource.persisted?

@payment = Payment.new({ email: params["user"]["email"],

token: params[:payment]["token"], user_id: resource.id })

flash[:error] = "Please check registration errors" unless @payment.valid?

begin

@payment.process_payment

@payment.save

rescue Exception => e

flash[:error] = e.message

resource.destroy

puts 'Payment failed'

render :new and return

end

if resource.active_for_authentication?

set_flash_message :notice, :signed_up if is_flashing_format?

sign_up(resource_name, resource)

respond_with resource, location: after_sign_up_path_for(resource)

else

set_flash_message :notice, :"signed_up_but_#{resource.inactive_message}" if is_flashing_format?

expire_data_after_sign_in!

respond_with resource, location: after_inactive_sign_up_path_for(resource)

end

else

clean_up_passwords resource

set_minimum_password_length

respond_with resource

end

end

end

protected

def configure_permitted_parameters

devise_parameter_sanitizer.for(:sign_up).push(:payment)

end

end

Update the registrations route for devise users in the config/routes.rb file:

devise_for :users, :controllers => { :registrations => 'registrations' }

In the next video the payment bug will be resolved where the first attempt at sign-up fails but the second attempt succeeds. This issue can be resolved by removing turbolinks. To remove turbolinks, simply remove the line that references turbolinks starting with //= in the application.js file under the app/assets/javascripts folder. In addition you can also remove the turbolinks gem from the gemfile and do a bundle install --without production.

Now it's time to deploy to heroku and test out the app! Don't forget to post a link to your heroku app in the discussions area! Congratulations on having completed customizing payments with Stripe!

____________________________________________________
Modules group functionality and concerns
services, concerns, constants and any other code that, by having the same responsibility should stay together

_______________________________________________________________________________
https://guides.rubyonrails.org/getting_started.html
File/Folder	Purpose
app/	Contains the controllers, models, views, helpers, mailers, channels, jobs, and assets for your application. You'll focus on this folder for the remainder of this guide.
bin/	Contains the rails script that starts your app and can contain other scripts you use to set up, update, deploy, or run your application.
config/	Contains configuration for your application's routes, database, and more. This is covered in more detail in Configuring Rails Applications.
config.ru	Rack configuration for Rack-based servers used to start the application. For more information about Rack, see the Rack website.
db/	Contains your current database schema, as well as the database migrations.
Gemfile
Gemfile.lock	These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. For more information about Bundler, see the Bundler website.
lib/	Extended modules for your application.
log/	Application log files.
public/	Contains static files and compiled assets. When your app is running, this directory will be exposed as-is.
Rakefile	This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing Rakefile, you should add your own tasks by adding files to the lib/tasks directory of your application.
README.md	This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on.
storage/	Active Storage files for Disk Service. This is covered in Active Storage Overview.
test/	Unit tests, fixtures, and other test apparatus. These are covered in Testing Rails Applications.
tmp/	Temporary files (like cache and pid files).
vendor/	A place for all third-party code. In a typical Rails application this includes vendored gems.
.gitattributes	This file defines metadata for specific paths in a git repository. This metadata can be used by git and other tools to enhance their behavior. See the gitattributes documentation for more information.
.gitignore	This file tells git which files (or patterns) it should ignore. See GitHub - Ignoring files for more information about ignoring files.
.ruby-version	This file contains the default Ruby version.