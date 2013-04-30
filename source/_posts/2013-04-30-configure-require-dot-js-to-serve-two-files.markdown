---
layout: post
title: "Configure require.js to serve two files"
written: "on the Victoria line"
date: 2013-04-30 09:35
comments: true
categories: 
---


## The reason

Why would you want to serve two javascript files rather than concatenating them? It's actually a common practice to leverage better caching of assets which are rarely updated.

At [Lonely Planet](http://www.lonelyplanet.com/england/london/hotels) our files are broken up into:

<ul>
  <li>core.js - Common libs and abstractions(eg. jquery), header and footer code, analytics</li>
  <li>application.js - The code responsible for everything between the header and footer</li>
</ul>

Core.js is likely to never change so we want any repeat users to enjoy the benefit of strong browser caching. We serve this script appended with an MD5 hash so that we can invalidate the cache when it does finally get updated.


## The background

We use the requirejs-rails port which includes the r.js optimiser. It does an awesome job of consolidating all your modules into one file which you load asynchronously using the requirejs script tag and a data-main attribute. The main focus seems to be web apps rather than traditional web sites and as a consequence there were very few resources available around the topic of serving more than one file.

We wanted to use core.js to initialise the rest of the application once it had satisfied its own dependencies. The reason behind this was that core.js would then load jQuery before it was subsequently requested by application.js.

After much playing around with the requirejs config it was clear this was the wrong approach. We were getting multiple files requested dynamically and it was becoming a headache. I knew there would be a simple solution and really it was just a matter of looking at it from the application side: core.js was really a dependency of application.js

## The half-solution

### require.js config:
{% codeblock %}
modules:
  - name: 'appplication'
    exclude: ['core']
{% endcodeblock %}

### application.js:
{% codeblock %}
  require ['core', 'lib/application'], (Core, Application) ->
    Application = new Application()
{% endcodeblock %}

Why this didn't work? Application.js had dependencies which themselves depended on jQuery. Require loaded core and lib/application at the same time. This was the output:

{% codeblock %}
<script type="text/javascript" src="core.js">
<script type="text/javascript" src="application.js">
<script type="text/javascript" src="jquery.js">
{% endcodeblock %}

Not bad, but we have an extra request to load jQuery.

## The actual solution

### require.js config:
{% codeblock %}
modules:
  - name: 'appplication'
    exclude: ['core']

findNestedDependencies: true
{% endcodeblock %}

### application.js:
{% codeblock %}
require ['core'], () ->
  require ['jquery', 'lib/application'], ($, Application)->
    $ ->
      application = new Application()
{% endcodeblock %}

### Output:
{% codeblock %}
  // Dynamically created
  <script type="text/javascript" src="core.js">
  <script type="text/javascript" src="application.js">

  // On the page
  <script type="text/javascript" src="require.js" data-main="application.js">
{% endcodeblock %}

Running r.js will optimise core.js separately from application.js. Once it executes, the process will happen like this:

<ul>
<li>Try to find core.js</li>
<li>We don&apos;t have it bundled so load the optimised core.js dynamically</li>
<li>Once core.js is loaded we move into the require function</li>
<li>Try to find jQuery. Requirejs knows it is already loaded.</li>
<li>Try to find lib/application.js. This is bundled into application.js from the r.js optimisation step.</li>
<li>Move into the nested require function</li>
<li>Initialise application()</li>
</ul>

That&apos;s all there is to it. We can now specify any rarely used modules as a dependency of core.js, leverage the browser cache, and make faster websites.