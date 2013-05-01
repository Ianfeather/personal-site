---
layout: post
title: "Should the nav element be sectioning content"
date: 2013-05-01 08:52
comments: true
categories: 
---

Recently we've been nurturing the document outlines at Lonely Planet to create a simpler snapshot of the page for both accessibility and SEO reasons. One thing I was surprised about is how the <pre><nav></pre> element creates a new section in the document outline.
  
New sections require a heading or they appear in the document outline as "untitled". 

<div class="blog-grid">
  <div class="column">
    <img src="images/boston-globe-outline.jpg" />
  </div>
  <div class="column">
    <img src="images/smashing-mag-outline.jpg" />
  </div>
</div>

How often do you code up a navigational section with a heading? More often than not the elements and their position on the page is self-documenting. And do you even want your navigation to be part of the document outline at all?

It is worth noting at this point that VoiceOver (I haven't tested other screen readers) will not read out 'Untitled nav' and will just honour the links within. This gives is roughly the same importance as a <pre><div></pre>. 

Also, not all html5 elements create a new sectioning context. <pre><footer></pre> does not, for example.

This leaves you with a few options for marking up your navigation:

<ul>
  <li>A non html5 element. Using a <pre><div></pre> or a <pre><ul></pre> will not affect the document outline.</li>
  <li>A non html5 element with an aria navigation role. Maybe the best of both worlds but at the expense of not following the latest specs.</li>
  <li><pre><nav></pre> with a heading. If you want to use the more semantic <pre><nav></pre> you could use a heading and define your navigation in the outline. If it doesn't fit your design then you can always visually hide it. If you're doing this though, should you need to be jumping through these hoops?</li>
  <li><pre><nav></pre> - Just live with having untitled navigation</li>
</ul>

Which do you think is currently the most practical? More importantly, is the spec serving the majority of use cases with the <pre><nav></pre> being sectioning content?