---
layout: post
title: "Sinatra cheat sheets"
date:   2017-01-08
---


It's important to remember that Sinatra is an amazingly flexible micro framework with a top level DSL and not a feature packed framework like Rails. It's purposefully built to have just enough functionality to give you what you need to get going. There are incredibly useful helpers and extensions available that can be found with a bit of Googling, not to mention the thousands of Ruby Gems available to use.

Sinatra literally gives you a blank slate and all the application design decisions are entirely up to you. I found it much easier to work with Sinatra once I completely let go on any preconceptions I had about how web applications should be structured.

<h1>Routes</h1>
In Sinatra, a route is an HTTP method paired with a URL-matching pattern. Each route is associated with a block
<pre>
  <code>
    get '/' do
      .. show something ..
    end

    post '/' do
      .. create something ..
    end

    put '/' do
      .. replace something ..
    end

    patch '/' do
      .. modify something ..
    end

    delete '/' do
      .. annihilate something ..
    end
 </code>
</pre>


Route patterns may include named parameters, accessible via the params hash:

<pre>
  <code>
    get '/hello/:name' do
      # matches "GET /hello/foo" and "GET /hello/bar"
      # params['name'] is 'foo' or 'bar'
      "Hello #{params['name']}!"
    end
  </code>
</pre>

<h1>Conditions</h1>
Routes may include a variety of matching conditions, such as the user agent:
You can easily define your own conditions:
<pre>
  <code>
    set(:probability) { |value| condition { rand <= value } }

    get '/win_a_car', :probability => 0.1 do
      "You won!"
    end

    get '/win_a_car' do
      "Sorry, you lost."
    end
  </code>
</pre>

For a condition that takes multiple values use a splat:
<pre>
  </code>
    set(:auth) do |*roles|   # <- notice the splat here
      condition do
        unless logged_in? && roles.any? {|role| current_user.in_role? role }
          redirect "/login/", 303
        end
      end
    end

    get "/my/account/", :auth => [:user, :admin] do
      "Your Account Details"
    end

    get "/only/admin/", :auth => :admin do
      "Only admins are allowed here!"
    end
  </code>
</pre>    


<h1>Views / Templates</h1>
Each template language is exposed via its own rendering method. These methods simply return a string:
<pre>
  <code>
    get '/' do
      erb :index
    end
  </code>
</pre>

<h3>This renders views/index.erb.<h3>

Instead of a template name, you can also just pass in the template content directly:
<pre>
  <code>
    get '/' do
      code = "<%= Time.now %>"
      erb code
    end
  </code>
</pre>
Templates take a second argument, the options hash:
<pre>
  <code>
    get '/' do
      erb :index, :layout => :post
    end
  </code>
</pre>
