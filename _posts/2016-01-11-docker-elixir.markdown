---
layout:     post
title:      "Docker + Elixir = ?"
subtitle:   "Host-container communication with Docker and Elixir"
date:       2016-01-11 22:40:00
author:     "Krzysztof Marciniak"
---

<p>Working on a university project (implementing a 3PC algorithm) I've encountered quite an interesting problem that made me pull my hair out for several days in a row (yes, that shouldn't have been such an issue, but somehow I didn't think of the solution sooner). The problem was making two nodes, host OS and container, communicate over the bridged interface Docker provides (either the default one, <i>docker0</i>, or some new one; I decided on the latter).
</p>
<p>First, let's pull an image that contains the basic Elixir installation:</p>

{% highlight console %}
# docker pull trenpixster/elixir
{% endhighlight %}

<p>
<blockquote>
<b>Disclaimer</b>:
As of the moment I'm writing this, the latest version of Elixir and Erlang are, respectively, 1.2 and 18.1. It might be that when you're reading this the situation has changed and <i>trenpixster/elixir</i> is no longer the one with the latest version; then simply search for elixir images (<i>docker search elixir</i>) and use that one instead, the only requirement is for the image to have Elixir (to be specific, the Elixir console - <i>iex</i>).
</blockquote>
</p>

<p>
Now, let's create a network bridge to which we'll attach our container:
</p>

{% highlight console %}
# docker network create --driver bridge mybridge
{% endhighlight %}

<p>
Checking with <i>ip a</i> I've found out that my ip address is <i>172.18.0.1</i> - that's quite important, since that's the IP address we'll be listening on on the host. To find that address, use this bash/zsh one-liner:
</p>

{% highlight console %}
{% raw %}
# docker network ls | grep mybridge | cut -d' ' -f1 | xargs -I "{}" ip addr show "br-{}" | grep inet
    inet 172.18.0.1/16 scope global br-34542f875229
{% endraw %}
{% endhighlight %}

<p>We need to do this, because the name of the interface is not <i>mybridge</i>, but <i>br-id</i> where id is the output you get running <i>docker network create</i>.</p>

<p>Okay, now we're set! Let's run our awesome container, passing a nice one-liner as a command (I'll explain why in a second)
</p>

{% highlight console %}
{% raw %}
# docker run --net=mybridge -i -t trenpixster/elixir /bin/bash -c '(export IP_ADDR=`ip a | tail -4 | head -1 | tr -s " " | cut -d" " -f3 | cut -d/ -f1` && iex --name "mycontainer@$IP_ADDR" --cookie password)'
{% endraw %}
{% endhighlight %}

<p>This should start the elixir console.</p>

{% raw %}
<p>So what are we exactly doing here? Of course, we're using our shiny new bridge and the image we've pulled, but the command running in the container sets the <i>$IP_ADDR</i> variable to the main interface's IP address (container's IP on the bridged interface) and then runs Elixir console setting full node name (<i>mycontainer@someip</i>) and sets cookie (let's say that's a password/callout for the host node).</p>
{% endraw %}

<p>Why do we do this? Should we not set the correct node name/cookie, iex would refuse connecting to the node either on host (because it can't find the host) or on the container (because setting the cookie whitelists the host system on our container).</p>

<p>Now that we know what we're doing, let's test our solution; firstly, create a simple module in the container console:</p>

{% highlight elixir %}
defmodule Hello do
def world, do: IO.puts "hello world"
end
{% endhighlight %}

<p>Now run the console on the host system as <i>iex --name "host@172.18.0.1"</i> and watch the communication working:</p>

{% highlight iex %}
iex> Node.spawn_link :"mycontainer@172.18.0.2", fn -> Hello.world end
hello world
#PID<8241.76.0>
iex> 
{% endhighlight %}

<p><i>Voilà!</i> Works like a charm.
</p>
