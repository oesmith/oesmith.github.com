---
layout: post
title: Using connect-assetmanager with sass and coffee-script
alias: /post/4981762820/using-connect-assetmanager-with-sass-and-coffee-script
---
I'm a big fan of using as few curly-brackets as possible in my code, which
means I love writing my stylesheets using [Sass][1] and my javascripts using
[CoffeeScript][2].

Because I love fast websites too, I also mash all my assets together using
[connect-assetmanager][3].  However, out of the box, connect-assetmanager
doesn't automatically compile Sass or CoffeeScript.

It's not difficult to get it working though.  All you need to do is write a
couple of short `preManiupulate` handlers, like so:

{% highlight coffeescript %}
# handlers.coffee
sass = require 'sass'
coffee = require 'coffee'

exports.sassRenderer = (file, path, index, isLast, callback) ->
  if /\.sass/.test path
    console.log "Compiling #{path}"
    callback sass.render(file)
  else
    callback file

exports.coffeeRenderer = (file, path, index, isLast, callback) ->
  if /\.coffee/.test path
    console.log "Compiling #{path}"
    callback coffee.compile(file)
  else
    callback file
{% endhighlight %}

Now all you need to do is insert these into your expressjs config and you're
good to go!

{% highlight coffeescript %}
# server.coffee
handlers = require 'handlers'

# ...

app.use assetManager
  js:
    route: /\/assets\/js\/[0-9]+\/.*\.js/
    path: './assets/javascripts/'
    dataType: 'javascript'
    files: ['sprintf.js','main.coffee']
    preManipulate:
      '^': [handlers.coffeeRenderer]
  css:
    route: /\/assets\/css\/[0-9]+\/.*\.css/
    path: './assets/stylesheets/'
    dataType: 'css'
    files: ['style.sass']
    preManipulate:
      '^': [handlers.sassRenderer]
{% endhighlight %}

Easy!

[1]: http://sass-lang.com
[2]: http://jashkenas.github.com/coffee-script/
[3]: https://github.com/mape/connect-assetmanager
