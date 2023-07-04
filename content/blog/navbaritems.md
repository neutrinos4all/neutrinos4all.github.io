---
title: "Pelican navbar menu items"
date: 2016-04-02
slug: 'navbaritems'
tag: 'tutorials'
---

Static site generators can greatly reduce the slope of the website creation learning curve. Take advantage of these tools if you are interested in creating your own simple website but want to sidestep manually generating the HTML and CSS on your own.
<!--more-->

--

__Update as of July 2023:__ I have obviously migrated this site from Pelican to Hugo, which means this blog post doesn't really correspond well to what you now see. However, I'm leaving it up in case other people find this useful in the future. This is one of my most popular posts, and the one that people actually email to talk with me about. Thanks to those who have and may do so in the future!

--

This is the first in a series of tutorials I'll be writing on the Pelican static site generator. I'm writing these because I was completely new to the notion of static site gen before I started creating my own website. I had a hard time finding documentation for some aspects of its layout. That was frustrating, particularly because I thought some of the functionality I was looking for should be fairly easy to execute.

I was mostly correct, but it just took some digging. Hopefully someone stumbling on this site will find these resources helpful. You should also note that I'm using `pelican-bootstrap3` as a theme.

### Navbar links ###
Once you create your site template and determine what you want to have (Will it be a blog only, or do you want to include some information in a dedicated page about yourself? What about linking to a document, like your resume? See the navbar at the top of my site for an example), you should figure out how you want to present it. I thought I would use a dedicated "Blog" section to post musings (and later tutorials), as well as include some information about myself and a way to contact me. These would hopefully appear as menu items in my navbar. That led me to include the following sections:

* About Me
* My Blog
* My CV

I whipped up a few entries in Markdown, and I was a little worried because the .md format requires you to include some metadata (of which only the `Title` and `Date` are actually required, but the rest provides useful hooks for additional theme functionality):

    Title: Sample blog post
    Date: 1901-01-01
    Category: blog
    Slug: sample
    Tags: tag1, tag2, tag3

When I finished my first blog post and built the site on localhost (`fab build`, then `fab serve`), the theme automatically uses the "Category" metadata entry as the link under which it'll appear as a menu item in the navbar. Great! It worked basically out of the box.

However, when I wrote my bio page and used "About" as the Category, the page was put under its own menu item but it included the `Date` metadata and formatted the page header like a blog post. That's not what I wanted for the bio page, which would be static and therefore shouldn't really have a posting date.

The solution is the distinction between *articles* and *pages*, which you can find in the documentation [here](http://docs.getpelican.com/en/3.6.3/content.html). I ended up creating two folders inside my content directory: `blogroll` and `pages`, and I'll save the corresponding files in their appropriate directory. When writing a page, you'll notice that you only need a `Title` entry at the top of the file, and it won't require any date information. It also slightly changes that page's header format. Rebuilding your site will also automatically put your pages in as menu items, using the title as the item name.

Finally, I wanted to add a link at the top of the page that would provide a link to my email address. My reasons here were two-fold: I didn't want my email plastered in plaintext on a page to make it harder for spambot crawlers to find my email address, and I wanted it somewhere immediately visible from the site index. Since I wanted my index to arrive at my blogroll (the default Pelican behavior), I decided to use reCAPTCHA's [MailHide](https://www.google.com/recaptcha/admin#mailhide) on the other side of a link. However, at this point I was only adding links to the menu through either page titles or article categories. That behavior wouldn't work here, because I didn't want to make a document with a link to my email's reCAPTCHA inside; I wanted to link it directly to the reCAPTCHA, so when you clicked on the "Email" item in the menu it took you right there.

My solution was to add an entry in my `pelicanconf.py` file called `MENUITEMS`:

    MENUITEMS = (
    ('Email', 'http://www.google.com/recaptcha/mailhide/d?...')
    )

This put an item in the menu with my desired functionality. The solution is deceptively easy, but as I said before I was completely new to this entire site generation process and these utilities. It took some time and searching to realize how to make it work.

### Navbar organization ###
The last thing you'll notice is that the menu items in the navbar are inserted alphabetically according to type: manually-placed menu items first, pages second, then articles third. It also doesn't capitalize words beyond the first if you have multiple-word page titles or article categories. At this point, my navbar wasn't really adhering to the layout I desired, and I wanted to change that.

Some searching revealed that Pelican includes some [settings](http://docs.getpelican.com/en/3.6.3/settings.html) that allow you to change this default organizational behavior, and you should spend some time looking through that list and seeing what you can change by adding those lines to `pelicanconf.py`. I decided to arrange my menu item order manually, rather than allowing Pelican to do it for me. The following changes to my config file enabled it:

    DISPLAY_CATEGORIES_ON_MENU = False
    DISPLAY_PAGES_ON_MENU = False

    MENUITEMS = (
        ('About', '/pages/about.html'),
        ('Blog', '/category/blog.html'),
        ('Email', 'http://www.google.com/recaptcha/mailhide/d?...'),
        ('Vita', '/pdfs/HouserCV.pdf')
        )

The menu items will now be generated in this order and draw their content from directory specified as the second value in the tuple.

It's important to note how all of this works in a general sense. Content I've linked manually to the menu items is being drawn from the files in the `output` directory, which is generated from the .md files I write in `content` (which is the whole point of the `fab` commands). `blog.html` is a file with dynamic links to the other files I've written with "blog" as the category, and my CV is stored in a `pdfs` directory I've created to keep track of any .pdf files I'll use throughout the site. If you maintain your site files in a well-organized hierarchy, keeping track of everything is trivial.

### A note on debugging ###
If you move through these steps and a page's format isn't changing between builds or `fab` can't find your files as you navigate around your site on localhost, be sure to try the following:

* Clear your browser's cache, or better yet add the `LOAD_CONTENT_CACHE = False` flag to your config file. A little more detail on that can be found [here](http://docs.getpelican.com/en/3.6.3/faq.html#changes-to-the-setting-file-take-no-effect). Caching can sometimes interfere with those smaller changes in either format or metadata assignments.
* Remember to watch the terminal output as you move around the localhost site (much easier with two monitors). Lines with output `WARNING:root:Unable to find "/samplefile.html" or variations` can be helpful, particularly because the expected filepath is given.

If you've found any errors or have any comments or questions, please feel free to email me.
