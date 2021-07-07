---
layout: page
title: Cheatsheets
permalink: /cheatsheets/
date: 2021-07-07
last_modified_at: 2021-07-07
---

## JWT

Generate JWT access token

```ruby
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

## Shoulda Matchers

```ruby
RSpec.describe UserProfile, type: :model do
  it { should validate_presence_of(:name) }
end
```

or

```ruby
describe UserProfile, type: :model do
  it { should validate_presence_of(:name) }
end
```

### Rails Model

```ruby
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
