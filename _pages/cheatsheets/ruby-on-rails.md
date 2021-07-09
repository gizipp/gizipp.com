---
layout: page
title: Ruby on Rails Cheatsheets
permalink: /cheatsheets/ruby-on-rails
date: 2021-07-07
last_modified_at: 2021-07-07
---

Generate JWT token

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

Random number
```rb
[*1..100].sample
```
