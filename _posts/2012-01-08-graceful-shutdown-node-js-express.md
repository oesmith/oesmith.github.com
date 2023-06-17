---
layout: post
title: Graceful server shutdown with node.js / express
---
[howmanyleft.co.uk][1] runs its node.js workers behind supervisord.  To avoid
dropping requests with [502s][2] when restarting workers, I hook into the
SIGTERM signal and call [close()][3] on the HTTP server. This stops the server
from listening for new connections, and once all the open connections are
complete, the worker exits.

Here's a quick example:

{% highlight javascript %}
var http = require('http');

var app = http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  setTimeout(function () { res.end('OK\n'); }, 5000);
});

process.on('SIGTERM', function () {
  console.log("Closing");
  app.close();
});

app.listen(3000);
{% endhighlight %}

Since I'm using redis on howmanyleft, I need to close my redis connection
gracefully too.  The [close][4] event on the HTTP server fires when all
connections have closed, so close my redis connection there.  [node_redis][5]
flushes all active redis commands when you call quit, so I won't lose any
data.

{% highlight javascript %}
var http = require('http'),
    redis = require('redis').createClient();

var app = http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  setTimeout(function () { res.end('OK\n'); }, 5000);
});

process.on('SIGTERM', function () {
  console.log("Closing");
  app.close();
});

app.on('close', function () {
  console.log("Closed");
  redis.quit();
});

app.listen(3000);
{% endhighlight %}

[1]: http://www.howmanyleft.co.uk
[2]: http://httpcats.herokuapp.com/502
[3]: http://nodejs.org/docs/latest/api/http.html#server.close
[4]: http://nodejs.org/docs/latest/api/http.html#event_close_
[5]: https://github.com/mranney/node_redis

