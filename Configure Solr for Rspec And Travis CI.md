## Configure Solr for Rspec And Travis CI

Configuring solr for your local Rspec suite and in travis CI can sometimes be tedious task.In most of the projects i have come across methods using solr was tested by stubing  the solr result.However i wanted to test my methods based on the actual solr session and i did not want my solr session to run always.So here is how i configured solr for Rspec and travis

## RSpec

I was surfing through the internet to find a way to start my solr and run it only when tests are running.I came across this particular gem

```ruby
gem 'sunspot-rails-tester'
```

The only configuration that you have to do in your spec_helper is
```ruby
$original_sunspot_session = Sunspot.session

RSpec.configure do |config|
  config.before do
    Sunspot.session = Sunspot::Rails::StubSessionProxy.new($original_sunspot_session)
  end

  config.before :each, :solr => true do
    Sunspot::Rails::Tester.start_original_sunspot_session
    Sunspot.session = $original_sunspot_session
    Sunspot.remove_all!
  end
end
```

To test your method with solr follow the example below
```ruby
require 'spec_helper'

describe 'search page' do
  it 'highlights the active tab in the navigation' do
    # uses the stub session
  end

  it 'finds and displays a person', :solr => true do
    # uses actual solr - indexing will happen
  end
end
```

## Travis

The complexity comes when you have to configure solr for travis.By default solr is not installed in travis boxes.So solr has to be downloaded and installed.This [link](https://github.com/moliware/travis-solr) will be of great help to install solr in your travis boxes.One thing to notice here is your configuration. Test is the default collection name created by Sunspot solr so the same name has to be used in your travis boxes,Otherwise your tests may file in CI environment.You are also required to specify the path of solrconfig.xml folder which should include schema.xml file as well.

```ruby 
curl -sSL https://raw.githubusercontent.com/moliware/travis-solr/master/travis-solr.sh | SOLR_VERSION=5.3.1 SOLR_COLLECTION=test SOLR_COLLECTION_CONF=path/to/spec/support/solr bash
```

Once you have configured the above steps will be able to run your specs successfully in both your local environment and Travis CI.Happy Coding !!!


Thanks to [moliware](https://github.com/moliware) for shell script.
