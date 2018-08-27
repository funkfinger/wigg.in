---
title: Testing helpers in Sinatra
date: '2010-03-09'
tags:
- code
- helper-methods
- helpers
- ruby
- sinatra
- test
- testing
---

How do you test your helper methods in a Sinatra::Base app?
<pre lang="ruby" line="1">
module FundAStache
  module Helpers    
    def logged_in?
      false # you shall not pass...
    end
  end
end
</pre>
Include them in your tests...
<pre lang="ruby" line="1">
class FundastacheUserTest < Test::Unit::TestCase
  include Rack::Test::Methods
  include FundAStache::Helpers
  ...
</pre>
