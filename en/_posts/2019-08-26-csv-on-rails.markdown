---
title: CSV on Ruby on Rails
---

I'm trying to accept CSV formatted batches on a RoR controller like a normal
params, without having to parse it, without extra controller code. I wanted
that magic feeling one feels with JSON posts but using CSV.

I found that it was not as hard, the main issue was the lack of documentation
about the methods that need to be used, because Rails team loves `:nodoc:`. So
I will share some code I used to make it work.

## Parameter parser

The first thing we need is a custom [parameter parser][parameter_parsers], that is an under-documented
feature on Rails, to use it we just need to make an initializer:

``` ruby
# config/initializers/csv_param_parser.rb

require 'csv'

ActionDispatch::Request.parameter_parsers.merge!(
  csv: -> (raw_post) {
    parsed_batch = CSV.new(raw_post, headers: true, header_converters: :symbol)
    { batch: parsed_batch.map(&:to_h) }
  }
)
```

You might need to fine-tune the options, but headers are required to have a
hash instead of an array. Also your use case might require enabling a converter
(to make it possible to have numbers or booleans).

Anyway, with that in place, you can write your controller to receive a CSV
batch without any mention to CSV.

``` ruby
def import
  batch = params.require(:batch).permit(:name, :value)
  results = Result.create batch
  redirect_to :index, notice: "Good, #{ results.size } imported"
end
```

And it should work.

Also, if json is to be accepted, you can use a JSON with the same values and it
will just work

``` json
{ "batch": [{"name": "foo", "value": 12}] }
```

But have in mind that for this to work, the `content-type` header must be set
correctly.

## Testing CSV requests

For testing those endpoints, we will need to set the request body correctly and
also the content-type on the request. Luckily Rails also have an undocumented
method for this, that need to be run on the test or spec file:

``` ruby
RSpec.describe "BatchEndpoints", type: :request do
  register_encoder :csv, param_encoder: -> params { params.to_s }

  it 'create results with a csv body' do
    post import_path, as: :csv, params: sample_csv
    expect(response).to be_successful
  end
end
```

The [register_encoder][] method will make the `as: :csv` do some magic, like
setting the content type. It will also try to encode the given params hash as a
CSV, but it will fail because the `to_csv` method is not defined, so we will
need to define the `param_encoder` block to handle this. Here I do nothing
because I already have a CSV I want to test. It is under-documented at the end
of a big block of documentation for [integration tests][2].

 [parameter_parsers]: https://devdocs.io/rails~5.2/actiondispatch/http/parameters/classmethods#method-i-parameter_parsers-3D

 [register_encoder]: https://github.com/rails/rails/blob/7fe3c69331175a64f01ef64e7afab6d9236fbdbc/actionpack/lib/action_dispatch/testing/request_encoder.rb#L49

 [2]: https://devdocs.io/rails~5.2/actiondispatch/integrationtest
