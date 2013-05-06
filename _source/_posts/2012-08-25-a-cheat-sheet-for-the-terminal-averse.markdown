---
layout: post
title: "A Cheat Sheet for the Terminal-Averse"
written: "after CLI enlightenment"
date: 2012-02-14 15:16
comments: true
categories: 
---

<p class="blog-intro">Did you know there's a cheat sheet you can build into Terminal? It's one that allows you to run complex git commands and change directories just by hitting a couple of keys. The best part is they're keys that you define for yourself. If you're a designer/front end developer who is wary and resistant to the growing impetus of the command line then you're in luck.</p>

<blockquote class="pull-quote">By defining rules around my most common workflow routines I've been able to improve productivity whilst also taming an area that was previously foreign territory.</blockquote>

Cut to the source code: <a class="h6" href="https://github.com/Ianfeather/aliases/blob/master/aliases">https://github.com/Ianfeather/aliases/blob/master/aliases</a>

<h2>Here's how it works:</h2>
Being a terminal-averse user you're probably not a huge fan of Vim. If this is the case you can invoke textmate from the command line as standard. If you use coda then you will have to download this plugin first (<a href="http://wefoundland.com/project/command-line_coda/">http://wefoundland.com/project/command-line_coda/</a>) but it will make your life significantly easier. For this tutorial I'm going to use textmate but if you use coda simply replace 'mate' with 'coda' below, or the equivalent for your favourite text editor.
<ol>
	<li>Fire up terminal</li>
	<li>Open your local version of bash_profile

{% codeblock %}
mate ~/.bash_profile
{% endcodeblock %}

Add a link to your local aliases file by adding this line at the bottom of the file

{% codeblock %}
source ~/.bash/aliases
{% endcodeblock %}

Now, save and close</li>
	<li>Navigate to your local bash folder

{% codeblock %}
cd ~/.bash
{% endcodeblock %}

If the above command yields a 'directory does not exist' error, create this directory by running

{% codeblock %}
mkdir ~/.bash
{% endcodeblock %}

Now create an aliases file and open it

{% codeblock %}
touch aliases
mate aliases
{% endcodeblock %}

</li>
	<li>This file is where you can create and manage your shortcuts to your oft-used commands. It's remarkably easy. For example:

{% codeblock %}
alias gb='git branch'
{% endcodeblock %}

The above line would allow you to view a list of your current git branches simply by typing gb directly into the command line, and:

{% codeblock %}
alias ia='open -b jp.informationarchitects.WriterForMacOSX'
{% endcodeblock %}

Would allow you to open up a file in iA Writer by running: ia myfile.md</li>
</ol>
You can add, group and comment (#) as many rules as you like within this file. Here is a sample of my current alias setup: <a href="https://github.com/Ianfeather/aliases/blob/master/aliases">https://github.com/Ianfeather/aliases/blob/master/aliases</a>

It's worth noting that this only works for your local user. If you want to create the same rules for all users you execute the same steps in /etc/aliases. Each user would still be able to override these using the above rules though.

By defining rules around my most common workflow routines I've been able to improve productivity whilst also taming an area that was previously foreign territory.

Give it a whirl. Any questions, I'll do my best to help.