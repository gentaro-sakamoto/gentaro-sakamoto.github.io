---
published: false
title: How to upload photo by Paperclip On AWS
layout: post
---
# Configuration On AWS Side

1. Create a bucket.
2. Give it the permission upload/delete


# Configuration On Rails Side

## In Gemfile

```
gem 'paperclip'
gem 'aws-sdk', '<2.0'
```

## In config/aws.yml

```
common: &default_settings
  access_key_id: ACCESS_KEY_ID
  secret_access_key: ACCESS_KEY

development:
  <<: *default_settings

staging:
  <<: *default_settings

production:
  <<: *default_settings
  
```

## In development.rb:

```
config.paperclip_defaults = {
  :storage => :s3,
  :s3_host_name => 'REMOVE_THIS_LINE_IF_UNNECESSARY',
  :bucket => 'S3_BUCKET_NAME'
}
```

You need to configure the another environments such staging and production as well.


# Ref
- [Upcoming Stable Release of AWS SDK for Ruby - Version 2](http://ruby.awsblog.com/post/TxFKSK2QJE6RPZ/Upcoming-Stable-Release-of-AWS-SDK-for-Ruby-Version-2)
- [Does Paperclip 4.3.1 support aws-sdk-v2?](https://github.com/thoughtbot/paperclip/issues/2021#issuecomment-151563433)
- [Paperclip with Amazon S3](https://github.com/thoughtbot/paperclip/wiki/Paperclip-with-Amazon-S3)