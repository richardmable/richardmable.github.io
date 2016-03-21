---
layout: post
title: Passing Ruby Data into JS with Rails
---

##This post is in draft form. Read if you dare!

struggled creating a hidden field with custom action
Sometimes you have to know when to turn around:

Ugly non-working solution:

Currently I have a fake form with a hidden field that sends the var’s value and use the submit button to call a custom action that increments the var and then ideally, re-renders the page, using responds_with to send the incremented var back to the page. But, my custom action seems to think my var is nil. I can see it as zero when my params are sent. That’s as far as I gotten. Is this even possible?


played around with that, and while reading discovered gon gem

talk about gon gem

parsing data back to page:

{% highlight javascript %}
console.log(gon.cardDisplay[0].question);

	$(".question").append(gon.cardDisplay[0].question);
	$(".answer").append(gon.cardDisplay[0].answer);
{% endhighlight %}

tricky with gon gem is to put <%=  Gon::Base.render_data %>
in each view you want to use it in. Otherwise it will lose the var.
Many attempts were made
First checked if worked with linking to another page. That worked and could render data
It would display to page
Next, played around with placing <%=  Gon::Base.render_data %> in different views
Tried in card_load.js.erb
but that is not a view!
So then I put <%=  Gon::Base.render_data %> in the flip_card.html.erb view
along with the body of the application controller
while it may seem repetitive, it ensures that Gon is available in all views when using uJs.
After seeing that I could display the objects in the console and alerts, I knew it was possible
It was just a matter of adjusting all my vars and tweaking initial displays.
Now when a user loads a deck of cards, the new data is uJs'd into the page, and the user can start going through any cards
On the user's end, it looks automatic and nothing appears to reload.
