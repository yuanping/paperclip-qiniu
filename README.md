# Paperclip::Qiniu

storage [paperclip](https://github.com/thoughtbot/paperclip/) attachments to http://qiniutek.com

example project: https://github.com/lidaobing/paperclip-qiniu-example

example site: http://stark-cloud-4321.herokuapp.com/

## Usage

* confirm you are working on a rails app

* add following line to `Gemfile`

```ruby
gem 'paperclip'
gem 'paperclip-qiniu'
```

* edit your `config/application.rb`

```ruby
module PaperclipQiniuExample
  class Application < Rails::Application
    # ....
    config.paperclip_defaults = {:storage => :qiniu,
      :qiniu_credentials => {
        :access_key => ENV['QINIU_ACCESS_KEY'] || raise("set env QINIU_ACCESS_KEY"),
        :secret_key => ENV['QINIU_SECRET_KEY'] || raise("set env QINIU_SECRET_KEY")
      },
      :bucket => "paperclip-qiniu-example",
      :use_timestamp => false,         # required, and must be set to false
      :qiniu_host => "cdn.example.com" # optional
    }
  end
end
```

for more information on `qiniu_host`, read http://docs.qiniutek.com/v2/sdk/ruby/#publish

* add a model like this

```ruby
class Image < ActiveRecord::Base
  attr_accessible :file
  has_attached_file :file, :path => ":class/:attachment/:id/:basename.:extension"
  validates :file, :attachment_presence => true
end
```

* show image in your view

```erb
<%= qiniu_image_tag @image.file.url, :thumbnail => '300x300>', :quality => 80 %>
or
<%= image_tag qiniu_image_path(@image.file.url, :thumbnail => '300x300>', :quality => 80) %>
```

support options: `thumbnail`, `gravity`, `crop`, `quality`, `rotate`, `format`, `auto_orient`. for more information on these options, check the document: http://docs.qiniutek.com/v3/api/foimg/#fo-imageMogr

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## CHANGELOG

### 0.1.0 (2012-09-06)

* add view helper: qiniu_image_tag, qiniu_image_path
* update demo code, style is no longer needed.

### 0.0.3 (2012-09-06)

* support qiniu api v3.

### 0.0.2 (2012-07-17)

* support public host.

### 0.0.1

* it works.

