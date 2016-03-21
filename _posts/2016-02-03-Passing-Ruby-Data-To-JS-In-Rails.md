---
layout: post
title: Using the Gon Gem in Rails
---

---
While working on the [Flash Card App](https://protected-tor-56595.herokuapp.com), we needed to solve an issue of passing Ruby data stored as variables onto the page via JavaScript, without reloading the page. This was necessary because our design of the application was to be a single page user experience, using Rail's unobtrusive JavaScript. A bit of asking around on Slack and Googling led to the [Gon Gem](https://github.com/gazay/gon).

The Gon gem's primary aim is to make your Rails' variables available in your JavaScript. This way you can use them in views that are rendered with JavaScript on a page update (i.e. when a button is clicked, etc).

Here's how you set a Gon variable in a Rails controller: `gon.variable_name = variable_value`

And here's how you call it in your JS file: `gon.variable_name`

Our particular use was that we wanted a set of flash cards to load automatically when selected without refreshing the page, and have them display in the flash card div. 

The tricky part that is not really covered by the readme is that you must put `<%=  Gon::Base.render_data %>` in each view you want to use it in, otherwise it will lose the variable.

There was a lot of experimentation with this gem and what exactly what it was doing, but in the end we found that putting `<%=  Gon::Base.render_data %>` in the flip_card.html.erb view along with the body of the application controller worked.

While it may seem repetitive, it ensures that Gon is available in all views when using uJS. After confirming that I could display the objects in the console and alerts, I knew it was possible to load the variables onto the flash car without a refresh, it was just a matter of adjusting all my vars and tweaking initial displays.

Example parsing data back to page:

{% highlight javascript %}
console.log(gon.cardDisplay[0].question);

	$(".question").append(gon.cardDisplay[0].question);
	$(".answer").append(gon.cardDisplay[0].answer);
{% endhighlight %}

Now when a user loads a deck of cards, the new data is unobtrusively loaded onto the page, and the user can start going through any cards. On the user's end, it looks automatic and nothing appears to reload.

Unfortunately, the gem appears to make only one initial database call. So whatever card set the user picks the first time is the one they are stuck with. I am still experimenting as to exactly why this is, but it seems to have to do with the Gon gem not refreshing its variables when called in the controller. I will update this post if I find a solution.
