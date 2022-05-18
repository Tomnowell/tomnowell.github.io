# How to make a portfolio

## Show off

I really dislike writing about myself. Every time I dust off my old CV and rewrite it I cringe
a little. Did I really write that about myself? Am I that boastful, that cocky. What a jerk I am.
I'm a fairly confident person. I'm happy to stand on a stage and sing to few thousand people. I'll
even try telling them a bad joke or two.  But, when it comes to writing a document on paper, or online,
for that matter. I freeze. But as I have matured like cheese, and not like Daniel Craig, I have found it
necessary. My desire for a decent income outweighed my humility and I finally updated my CV. But that's not
enough. As I embark on my career change from teacher to full time nerd. I need to show off online, too.
I need a website.

## Develop

That makes sense, if I want to be a developer I should develop. I have a few projects I think the world could use
or at least that I am not so utterly ashamed of that I don't mind making the repositories public. Check out my [sourdough bread calculator](https://github.com/Tomnowell/IlPane). But it's been years since I did anything serious with HTML or CSS and I am not
(yet) a very good designer.



## Templates

Fear not, there are many css templates available for free or for very little money. I like to support people who give you a choice. Just a couple of examples [W3 schools](https://www.w3schools.com/w3css/w3css_templates.asp), [HTML5UP](https://html5up.net/). HTML5 UP have a sister site [pixelarity](https://pixelarity.com/) Where for under $20 you can get all the designs and use them without attribution. I thought this may be useful if I need to do a design for a client in a hurry.

## Go solo


If you want to go a bit more custom, but don't want to write all your own CSS I recommend the [bootstrap](https://getbootstrap.com/) library. I also used [fontawesome](fontawesome.com) for nice icons. They make any site look a thousand times more professional.

Whatever you decide, it is really easy to get a static page up and running in a short time.

## Add graphics

Of course the template is just one part of designing a website. You'll probably want to customize it a little.  So, get your template and poke around in the code and make it your own! Definitely attribute the authors if you're using it for free. Use your own graphics or use stock images from sites like [pexels](https://www.pexels.com/) or [unsplash](https://unsplash.com/).

## Add Content

Add a few of your projects and write about your skills. I added an email me form from [formspree](https://formspree.io/) there are alternatives, in fact I am currently implementing a [typeform](https://try.typeform.com/home/?gclsrc=aw.ds&&tf_campaign=brand_9724248146_v2&tf_source=google&tf_medium=paid&tf_content=101060309498_427947667564&tf_term=ytypeform&tf_dv=c&tf_matchtype=e&tf_adposition=&gclid=CjwKCAiA6Y2QBhAtEiwAGHybPaS-yJkyhvrj88r7Vyryp7RkN8lz0EKjSeNZBbEk8_KiAAgq7zIlARoCVxYQAvD_BwE&gclsrc=aw.ds) form as a feedback / comments box for this blog. Literally, watch this space. Formspree are more generous with their free tier. You get 50 free form submissions a month.  Typeform give you just 10, so please think about your comments carefully before posting them. Just create your forms back end on the provider and they will provide an endpoint for your form. Send your form data to the endpoint as a http POST request and Bob's your uncle, well, he's my uncle. Hi Rob!

## Hosting

If you want free hosting, I recommend Github Pages. But there are other free or paid options of course. You'll have to think outside the box if you want anything dynamic on Github Pages as they don't allow any server side scripting, *PHP or Python*, for example.  But they suggest integration with [Jekyll](https://jekyllrb.com/). I chose Github Pages to host my site as I already use Github for my repositories and it's as simple as:

0. If you don't have git installed, install it.
1. Make a repository with the name
    > YOUR-GITHUB-USERNAME.github.io
2. Clone that repository on your local device:
    1. Copy the link from Github:
    2. In your shell run
        git clone (PASTE THE LINK HERE)
3. Put your website in that repository on your local device.
4. In your shell run:
        git add .
        git commit -m "Initial commit"
        git push
5. drink coffee

My website was up and running after about 3 hours. That included the time to fiddle around with some of the formatting and CSS. But it wasn't perfect. I recommend using a simple local http server to check your website. If you have python installed on your system, you can use http.server. For example:

0. In your shell of choice navigate to the folder containing the index.html file of your website.

1. Run the command:
        python -m http.server 8888 --bind 127.0.0.1

2. In your browser of choice go to 127.0.0.1:8888

When you're ready follow the instructions to push to Github.

## You're done! Well done, you

Your shiny new portfolio is born and isn't it handsome? / beautiful? / cool? ...delete as appropriate

Your website should appear in all it's glory. You might be able to get away without specifying port "8888" and IP addresses "127.0.0.1". For example:
        python -m http.server
This may work for you. It works on linux for me, but not on Windows. I suggest running a local server to check your changes because Github actually have a soft limit of 10 pushes per hour to a Pages site. While I exceeded this (sorry Github folks) and didn't receive any warnings, it's quicker and less demanding on the server to do most of your testing locally.

Next time, I'll go through how I linked [my Github page](https://tomnowell.github.io/index.html) with my domain: [www.tomnowell.com](http://www.tomnowell.com).

My plan for the third edition in this mini series will be to explain how I integrated this blog functionality. While people have been doing this kind of thing for years. I'm still proud that *I* could figure out how to do it. Until then,  

Peace & love & learn :)

### Thanks

I'd like to thank [Stephen Anderson](https://github.com/stephenandersondev) for his post about [how to make a portfolio for free](https://levelup.gitconnected.com/how-to-build-a-web-developer-portfolio-for-free-d456699ecef7). I got some great hints and inspiration.
