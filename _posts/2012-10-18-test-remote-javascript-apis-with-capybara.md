---
layout: post
title: Test remote javascript APIs with Capybara and puffing-billy
---
Testing remote APIs is easy in Ruby.  Libraries like [webmock][1], [vcr][2] and
[artifice][3] give you all the tools you need to ensure that you're sending the
right requests to the right remote endpoints.

However, it becomes a lot more difficult when it's your **Javascript** code
that's making the requests.  Using requests specs with Capybara gives you loads
of tools to control the browser, but it doesn't let you stub or mock responses
to requests that originate from within the browser.

This is where [puffing-billy][4] saves the day.  It's a request stubbing
library *for browsers*.  It spawns a HTTP(S) proxy server that it uses to
intercept requests from your browser.  Using simple webmock-alike syntax, you
can configure the proxy to send fake responses to requests for specific URLs.

For example, the following is a simple piece of javascript code that fetches
the temperature for Bath, UK from the [openweathermap.org][5] service.

{% highlight html %}
<!-- /weather/ -->
<p>Current temperature for Bath, UK: <span id='temp'></span></p>
<script>
  $(function () {
    $.ajax({
      url: 'http://openweathermap.org/data/weather/2656173',
      dataType: 'jsonp',
      success: function (data) { $('#temp').text(data.temp + '°C') },
      error: function () { $('#temp').text('unavailable') }
    });
  });
</script>
{% endhighlight %}

And this is a request spec for that snippet.  Note how it easily fakes JSONP
data and error responses.

{% highlight ruby %}
# spec/requests/weather_spec.rb
describe 'Weather for Bath, UK', :js => true do

  it 'should fetch the temperature from openweathermap.org' do
    # fake some JSONP data
    proxy.stub('http://openweathermap.org/data/weather/2656173')
      .and_return(:jsonp => {:temp => 12.7})

    visit '/weather/'
    page.should have_content('Current temperature for Bath, UK: 12.7°C')
  end

  it "should fail gracefully when openweathermap.org isn't available" do
    # fake a failure
    proxy.stub('http://openweathermap.org/data/weather/2656173')
      .and_return(:code => 500)

    visit '/weather/'
    page.should have_content('Current temperature for Bath, UK: unavailable')
  end

end
{% endhighlight %}

[puffing-billy][4] supports HTTP and HTTPS requests and all common HTTP verbs.
Go check it out on GitHub now!

[1]: https://github.com/bblimke/webmock
[2]: https://github.com/myronmarston/vcr
[3]: https://github.com/wycats/artifice
[4]: https://github.com/oesmith/puffing-billy
[5]: http://openweathermap.org/wiki/API

