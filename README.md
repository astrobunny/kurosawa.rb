# kurosawa.rb

![Kurosawa Ruby](https://github.com/astrobunny/kurosawa.rb/raw/master/docs/images/kurosawa-ruby.jpg)

A RESTful JSON-based database for eventually-consistent filesystem backends. Uses the REST path to determine object hierarchy.

# THIS DATABASE IS STILL IN ALPHA

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'kurosawa'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install kurosawa

## Usage

### On your local environment

Run the server

```
KUROSAWA_FILESYSTEM=file://fs/db bundle exec kurosawa
```

### With a docker container

```
docker run -ti -p4567:4567 astrobunny/kurosawa.rb
```

### Try it out!

Send it REST commands! (Here I am using resty)

```
$ GET /
null
$ PUT / '{"a": 7, "b": {"e":[100,200,300]} }'
{"a":"7","b":{"e":["300","200","100"]}}
$ GET /
{"a":"7","b":{"e":["300","200","100"]}}
$ GET /a/b
null
$ GET /a
"7"
$ GET /b
{"e":["300","200","100"]}
$ GET /b/e
["300","200","100"]
$ PATCH / '{"c": "Idols"}'
{"a":"7","b":{"e":["300","200","100"]},"c":"Idols"}
$ GET /c
"Idols"
$ DELETE /b/e
null
$ GET /
{"a":"7","b":{},"c":"Idols"}
$ PUT /c '{}'
{}
$ PUT /c '{"5": "a"}'
{"5":"a"}

```

### Connecting to a filesystem

kurosawa.rb supports these filesystems:
- local filesystem
- s3

Using the local filesystem: 

```
KUROSAWA_FILESYSTEM=file:///var/some/local/path
```

Using S3:

```
KUROSAWA_FILESYSTEM=s3://access_key:access_secret:us-west-1@bucket-name
```

Ensure that you URL encode your access_key and access_secret so it will not fail to parse the URI. Replace us-west-1 with whichever region your bucket is located in.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/astrobunny/kurosawa.rb.

