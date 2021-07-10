---
layout: page
title: Ruby on Rails Cheatsheetss
permalink: /cheatsheets/rails
date: 2021-07-07
last_modified_at: 2021-07-07
---

### Rails

Generate a new Rails app w/ Postgres support

```sh
rails new myapp --database=postgresql
```

Without minitest

```sh
rails new myapp -T
```

### Rails Model

```rb
describe 'associations' do
  it { should belong_to(:parent).optional }
  it { should belong_to(:category).class_name('MenuCategory') }
  it { should have_many(:childs) }
end

describe 'indexes' do
  it { should have_db_index(:parent_id) }
end

describe 'validations' do
  it { should validate_presence_of(:name) }
  it { should validate_uniqueness_of(:name).scoped_to(:category_id) }
  it { should allow_value('https://foo.com').for(:website_url) }

  # class UserProfile
  #   include ActiveModel::Model
  #   attr_accessor :birthday_as_string
  #
  #   validates_format_of :birthday_as_string,
  #     with: /^(\d+)-(\d+)-(\d+)$/,
  #     on: :create
  # end

  it { should     allow_value('2013-01-01').for(:birthday_as_string).on(:create) }
  it { should_not allow_value('2013/01/01').for(:birthday_as_string).on(:create) }
end

describe 'delegations' do
  it { should delegate_method(:name).to(:brand).with_prefix.allow_nil }
  it { should delegate_method(:name).to(:category).with_prefix.allow_nil }
end

```

### Rails Enum

```rb
enum role: {
 user: 0,
 admin: 1,
 other: 99
}

```

### Rails Validations

```rb
validates :name, presence: true
validates :terms_of_service, acceptance: true
validates :terms_of_service, acceptance: { message: 'must be abided' }
validates :size, inclusion: { in: %w(small medium large),
  message: "%{value} is not a valid size" }
validates :name, length: { minimum: 2 }
validates :bio, length: { maximum: 500 }
validates :password, length: { in: 6..20 }
validates :registration_number, length: { is: 6 }
validates :points, numericality: true
validates :games_played, numericality: { only_integer: true }
validates :email, uniqueness: true
```

### Custom Validations

Testing custom validation with error message.

```ruby

   class UserProfile
     include ActiveModel::Model
     attr_accessor :sports_team

     validate :sports_team_must_be_valid

     private

     def sports_team_must_be_valid
       if sports_team !~ /^(Broncos|Titans)$/i
         self.errors.add :chosen_sports_team,
           'Must be either a Broncos fan or a Titans fan'
       end
     end
   end

   RSpec.describe UserProfile, type: :model do
     it do
       should allow_value('Broncos', 'Titans').
         for(:sports_team).
         with_message('Must be either a Broncos or Titans fan',
           against: :chosen_sports_team
         )
     end
   end
```

### Routes

Create a route that maps a URL to the controller action

```rb
# config/routes.rb
get 'welcome' => 'pages#home'
```

Shorthand for connecting a route to a controller/action
```rb
# config/routes.rb
get 'photos/show'

# The above is the same as:
get 'photos/show', :to 'photos#show'
get 'photos/show' => 'photos#show'
```

Automagically create all the routes for a RESTful resource

```rb
# config/routes.rb
resources :photos
```

HTTP Verb | Path | Controller#Action | Used for
--------- | ---- | ----------------- | -------
GET | /photos | photos#index | display a list of all photos
GET | /photos_new | photos#new | return an HTML form for creating a new photo
POST | /photos | photos#create | create a new photo
GET | /photos/:id | photos#show | display a specific photo
GET | /photos/:id/edit | photos#edit | return an HTML form for editing a photo
PATCH/PUT | /photos/:id | photos#update | update a specific photo
DELETE | /photos/:id | photos#destroy | delete a specific photo

Create resources for only certain actions

```rb
# config/routes.rb
resources :photos, :only => [:index]

# On the flip side, you can create a resource with exceptions
resources :photos, :except => [:new, :create, :edit, :update, :show, :destroy]
```

Create a route to a static view, without an action in the controller

```rb
# config/routes.rb
# If there's a file called 'about.html.erb' in 'app/views/photos', this file will be
#   automatically rendered when you call localhost:3000/photos/about
get 'photos/about', to: 'photos#about'
```

Reference: http://guides.rubyonrails.org/routing.html

### Controllers

Generate a new controller

**Note:** Name controllers in Pascal case and pluralize

```
$ rails g controller Photos
```

Generate a new controller with default actions, routes and views

```
$ rails g controller Photos index show
```

Reference: http://guides.rubyonrails.org/action_controller_overview.html

### Models

Generate a model and create a migration for the table

**Note:** Name models in Pascal case and singular

```
$ rails g model Photo
```

Generate a model and create a migration with table columns

```
$ rails g model Photo path:string caption:text
```

The migration automatically created for the above command:

```rb
class CreatePhotos < ActiveRecord::Migration
  def change
    create_table :photos do |t|
      t.string :path
      t.text :caption

      t.timestamps null: false
    end
  end
end
```

Reference: http://guides.rubyonrails.org/active_model_basics.html

### Migrations

Migration Data Types

* `:boolean`
* `:date`
* `:datetime`
* `:decimal`
* `:float`
* `:integer`
* `:primary_key`
* `:references`
* `:string`
* `:text`
* `:time`
* `:timestamp`

When the name of the migration follows the format `AddXXXToYYY` followed by a list of columns, it will add those columns to the existing table

```
$ rails g migration AddDateTakenToPhotos date_taken:datetime
```

The above creates the following migration:

```rb
class AddDateTakenToPhotos < ActiveRecord::Migration[5.0]
  def change
    add_column :photos, :date_taken, :datetime
  end
end
```

You can also add a new column to a table with an index

```
$ rails g migration AddDateTakenToPhotos date_taken:datetime:index
```

The above command generates the following migration:

```rb
class AddDateTakenToPhotos < ActiveRecord::Migration[5.0]
  def change
    add_column :photos, :date_taken, :datetime
    add_index :photos, :date_taken
  end
end
```

The opposite goes for migration names following the format: `RemoveXXXFromYYY`

```
$ rails g migration RemoveDateTakenFromPhotos date_taken:datetime
```

The above generates the following migration:

```rb
class RemoveDateTakenFromPhotos < ActiveRecord::Migration[5.0]
  def change
    remove_column :photos, :date_taken, :datetime
  end
end
```

### Scaffolding

Scaffolding is great for prototypes but don't rely too heavily on it: http://stackoverflow.com/a/25140503

```
$ rails g scaffold Photo path:string caption:text
$ rake db:migrate
```

### Rake

View all the routes in an application

```
$ rake routes
```

Seed the database with sample data from `db/seeds.rb`

```
$ rake db:seed
```

Run any pending migrations

```
$ rake db:migrate
```

Rollback the last migration performed

**NOTE:** Be VERY careful with this command in production, it's destructive and you could potentially lose data. Make sure you absolutely understand what will happen when you run it

```
$ rake db:rollback
```

### Path Helpers

Creating a path helper for a route

```rb
# Creating a path helper for a route
get '/photos/:id', to: 'photos#show', as: 'photo'
```

```rb
# app/controllers/photos_controller.rb
@photo = Photo.find(17)
```

```rb
# View for the action
<%= link_to 'Photo Record', photo_path(@photo) %>
```

Path helpers are automatically created when specifying a resource in `config/routes.rb`

```rb
# config/routes.rb
resources :photos
```

HTTP Verb | Path | Controller#Action | Named Helper
--------- | ---- | ----------------- | ------------
GET | /photos | photos#index | photos_path
GET | /photos/new | photos#new | new_photo_path
POST | /photos | photos#create | photos_path
GET | /photos/:id | photos#show | photo_path(:id)
GET | /photos/:id/edit | photos#edit | edit_photo_path(:id)
PATCH/PUT | /photos/:id |	photos#update | photo_path(:id)
DELETE | /photos/:id | photos#destroy | photo_path(:id)

### Asset Pipeline

Access images in the `app/assets/images` directory like this:

```html
<%= image_tag "rails.png" %>
```

Within views, link to JavaScript and CSS assets

```html
<%= stylesheet_link_tag "application" %>
<%= javascript_include_tag "application" %>
```
```html
<!-- Filenames are fingerprinted for cache busting -->
<link href="/assets/application-4dd5b109ee3439da54f5bdfd78a80473.css" media="screen"
rel="stylesheet" />
<script src="/assets/application-908e25f4bf641868d8683022a5b62f54.js"></script>
```

Reference: [http://guides.rubyonrails.org/asset_pipeline.html](http://guides.rubyonrails.org/asset_pipeline.html)

### Form Helpers

Bind a form to a model for creating/updating a resource

**Use this method if you're using strong params to protect against mass assignment**

```rb
# app/controllers/photos_controller.rb
def new
  @photo = Photo.new
end
```
```rb
# ERB view
<%= form_for @photo, url: {action: "create"}, html: {class: "nifty_form"} do |f| %>
  <%= f.text_field :path %>
  <%= f.text_area :caption, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```
```html
<!-- HTML output -->
<form accept-charset="UTF-8" action="/photos/create" method="post" class="nifty_form">
  <input id="photos_path" name="photo[path]" type="text" />
  <textarea id="photos_caption" name="photo[caption]" cols="60" rows="12"></textarea>
  <input name="commit" type="submit" value="Create" />
</form>
```

Create a form with a custom action and method

```rb
<%= form_tag("/search", method: "get") do %>
  <%= label_tag(:q, "Search for:") %>
  <%= text_field_tag(:q) %>
  <%= submit_tag("Search") %>
<% end %>
```
```html
<form accept-charset="UTF-8" action="/search" method="get">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <label for="q">Search for:</label>
  <input id="q" name="q" type="text" />
  <input name="commit" type="submit" value="Search" />
</form>
```

Reference: [http://guides.rubyonrails.org/form_helpers.html](http://guides.rubyonrails.org/form_helpers.html)

Upload file ActiveStorage (via console or rake tasks)

```rb
pathname = Pathname.new(path_to_image)

  image.attach(
    io: File.open(pathname),
    filename: pathname.basename.to_s,
    content_type: Rack::Mime.mime_type(pathname.extname)
  )
```

Generate dummy SKU
```rb
Faker::Barcode.upc_e_with_composite_symbology.gsub('|','-')
```

## JWT

Encode JWT / generate access token

```rb
def access_token
  secret = Rails.application.credentials.jwt[:secret_key]
  method = Rails.application.credentials.jwt[:algorithm]
  payload = {
    data: {email: email, full_name: full_name},
    iat: Time.now.to_i,
    sub: Rails.application.credentials.jwt[:subject]
  }

  JWT.encode(payload, secret, method)
end
```

Decode JWT
```rb
JWT.decode(token, secret, true, { algorithm: method })[0]
```

## Shoulda Matchers

```rb
RSpec.describe UserProfile, type: :model do
  it { should validate_presence_of(:name) }
end
```

or

```rb
describe UserProfile, type: :model do
  it { should validate_presence_of(:name) }
end
```
