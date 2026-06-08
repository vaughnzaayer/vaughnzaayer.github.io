---
title: (Old) Jekyll Website
---

My personal website is built using [Jekyll](https://jekyllrb.com), a Ruby framework for static sites and blogs. Jekyll adds a ton of new possibilities with the combination of page templates, Liquid, and Markdown. 

<!--more-->

{% raw %}

<figure style="text-align: center; margin: 0;">
  <img src="/assets/images/octojekyll.png" width="600">
  <figcaption style="font-style: italic; font-size: 0.9em; color: #666;">
    Octojekyll! (<a href="https://jekyllrb.com">Jekyll Homepage</a>)
  </figcaption>
</figure>

{% endraw %}

## So, why Jekyll?

When I first decided I wanted to build my own personal website, I was faced with overwhelming options for where to start and what toolkits to use. Services like WordPress and Squarespace offered comprehensive tools and templates for a monthly fee, but I wanted to get more hands-on with building the site structure. Popular choices such as [React.js](https://react.dev) were also promising, but there was still the need to find a way to host my site. Eventually I discovered Jekyll, which let me create a static site with the flexibility of Markdown and Liquid. 

All of my site could be neatly organized with Jekyll's structure, giving me plenty of flexibility to incorporate new features without increasingly complicated refactors. GitHub Pages also integrated very nicely, so I can deploy my site without having to worry about hosting limitations.

## Building my site

Right off the bat I knew I wanted to build an "extended resume" for showcasing my skillset, projects, and interests. That meant creating a basic home page, about page, and pages dedicated to a blog and a list of my projects. 

### Basic Jekyll structure

All Jekyll sites follow a basic structure. In the main directory you'll have the following directories:

- _data
- _includes
- _layouts


Very basically, the `_data` directory houses a collection of `.yml` files, each storing data objects. For example, my file `_data/contact_links.yml` looks like this:

{% highlight yml %}

- name: Email
  link: mailto:vaughnzaayer@gmail.com
  icon: assets/icons/mail.png
- name: GitHub
  link: https://github.com/vaughnzaayer
  icon: assets/icons/github-mark.png
- name: LinkedIn
  link: https://www.linkedin.com/in/vaughnzaayer/
  icon: assets/icons/InBug-Black.png

{% endhighlight %}

This is handy for later on when auto-populating a list of elements. 

The `_includes` directory is where we start learning one of Jekyll's strengths: templates and reusabilty. This is where you will write HTML for elements you plan on reusing on different pages. The "hero" contact card on my homepage is one of these elements since I use it on both my home and about pages. We'll get into what this file contains later.

Next up is `_layouts`. As expected, this is where you will write "templates" or "layouts" to reuse on different site pages. This is extremely useful for something like blog posts -- maybe your blog post layout is different from what you use on your home page. Instead of having to retype or copy/paste every post format, Jekyll handles it all for you!

### Writing the default layout
I wanted to have a "generic" layout that I could use as a baseline when making a new page, specifically something that could be suitable for my landing and about pages. Since Jekyll layouts also inherit from each other, we can just write all of our boilerplate HTML (metadata, linking stylesheets, etc.) once and not have to worry about it for future pages. [Here's what my `_layouts/default.html` looks like, in addition to some unrelated metadata and linking.](https://github.com/vaughnzaayer/vaughnzaayer.github.io/blob/main/_layouts/default.html)

{% raw %}

Assuming a bit of prior experience with HTML, the text in the brackets (such as `{{ page.title }}` in the head section) probably stand out first. This is [Liquid](https://jekyllrb.com/docs/liquid/), another core component of working with Jekyll. Liquid acts as a scripting language to help generalize templates, allowing users to write variables with `{{ variable }}` and logic with `{% if statement %}`. The line {% endraw %}

{% highlight html %}

<title>{% raw %}{{ page.title }}{% endraw %}</title>

{% endhighlight %}

will automatically set the title of a webpage using the `default.html` template. We'll get to what `page.title` represents when we talk about actually using this template down the line.

Next, the line `{% raw %}{% include navigation.html %}{% endraw %}` pulls `navigation.html` from our `_includes` directory and plops it right into our template. A similar process happens for including `contact_card.html`, except with a little of bit extra logic. A boolean in a page's metadata is checked with `{% raw %}{% if page.contact_card %}{% endraw %}` indicating whether a contact card should be included in a particular page using this template. The following HTML until the `{% raw %}{% endif %}{% endraw %}` tag is only included if the condition in the Liquid tag is met.

Hopefully it's starting to become clear how Jekyll's templates and template processing with Liquid is so helpful for spinning up and scaling a simple website. Not only can templates be reused for a quicker workflow, but integrating Liquid logic lets us use metadata to populate the page to our liking.

### Contact Card and includes
The contact card found on the home and about pages uses the include system. As mentioned before, this system is super useful for automatically add complex elements like navbars to your templates. Here's the code for the contact card:

{% highlight html %}
{% raw %}
<div class="contact_card">
	<img src= "assets/images/headshot_cropped.JPG" alt="A picture of me." id="profile_pic">
		<li class="contact_link_box">
		{% for item in site.data.contact_links %}
		<a href="{{ item.link  }}" target="_blank" class="contact_link">
		{{ item.name }}
		</a>
		{% endfor %}
		</li>
</div>
{% endraw %}
{% endhighlight %}

As you can see, the contact card is a pretty straightforward div with an image at the top and a list of clickable links below that. However, I take advantage of Liquid to dynamically create a list of links instead of going back and hardcoding a new entry each with every new item I want to add. The line `{% raw %}{% for item in site.data.contact_links %}{% endraw %}` acts as a `for` loop does in most other languages, iterating over each entry in `site.data.contact_links`. We've actually seen this metadata before -- `site.data.contact_links` is just `_data/contact_links.yml`. Going back to the contact link data, every entry has a name and a link, thus the use of the variables {% raw %}`{{ item.link }}` and `{{ item.name }}`{% endraw %} to populate the HTML elements. 


### (Finally) using the templates
Having set up both the default template and the some basic page elements set up as includes, I can finally write my home page, `index.html`. Here's all the code for that page:

{% highlight html %}
{% raw %}

---
layout: default
title: Home
contact_card: true
---


	{% capture home_content %}{% include home_page_content.md %}{% endcapture %}
	{{ home_content | markdownify }}
		
{% endraw %}
{% endhighlight %}

Yup, that's all of it. In fairness, this might be implemented a little differently than most Jekyll sites, but it also does a great job flexing the advantages of using Jekyll. 

Starting from the top -- the short preamble is the metadata that Liquid uses when building our site. First we specify `layout: default`, which tells Liquid to use the `_layouts/default.html` template. Recall that `default.html` uses two metadata variables `page.title` and `page.contact_card`. We also set the metadata variables in the preamble with `title: Home` and `contact_card: true`.

The first line of the actual page is a little hack-ey. The text on my home page is written in the file `home_page_content.md` since I wanted to write in Markdown instead of straight HTML. By using Liquid's {% raw %}`{% capture variable_name %}` command, I can import the Markdown file with `{% include home_page_content.md %}` and save it to the variable `home_content`. Then, I can tell Liquid to process the content as Markdown using `{% home_content | markdownify %}`. Think of this as using the "pipe" character in bash, passing the output of one command or variable straight to the input of another command. 

Going back once again to our `default.html`, you may notice the inclusion of a variable `{{ content }}`. Basically, Liquid takes everything written in `index.html` and "pastes" it right where `{{ content }}` is, the template acting as a sort of "wrapper" around the actual content that makes that page unique. 

We can avoid using the `markdownify` workaround entirely of course. For my about page, I wrote the content straight into the [file](https://github.com/vaughnzaayer/vaughnzaayer.github.io/blob/main/about.md) `about.md`. It's just a regular Markdown file but with the preamble for Liquid containing the same metadata needed for `default.html`. Liquid once again just takes the content from `about.md` and inserts it where the `{{ content }}` variable is in the template. We get all the same benefits of the home page without having to copy/paste everything over! {% endraw %}


## Is Jekyll right for you?

When I first made this website, I was overwhelmed with the number of options in front of me. Most routes were either too tedious or too hand-hold-y for what I wanted. For people who know a little about the basics of web development (i.e. the basics of HTML, CSS, and JS) like me, Jekyll was *the* solution. 