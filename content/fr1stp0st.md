Title: Fr1st P0st
Date: 2018-03-20 15:18
Modified: 2018-03-20 22:05
Category: Site
Tags: site, backend
Slug: fr1st-p0st
Authors: Christopher Tichenor
Summary: First post, how this site is made.

Yes, this dates me as an olde time Slashdot user. Buck feta!

I’m writing this before I’ve even registered my domain because I want everything _about_ this site chronicled _in_ the site. I’ll get to my mission statement in the next post.

Apologies for the stream-of-consciousness nature of the prose here.

## Domain Registrar

First order of business is to pick a domain registrar... time to start Googling. Hm, a couple of hits, a fair amount of what looks like sponsored posts... GoDaddy is fairly low-ranked which is a good sign... [NameCheap](https://makeawebsitehub.com/go/namecheap) and [BlueHost](https://makeawebsitehub.com/go/bluehost) might work? Providers may be gaming the algorithm, DuckDuckGo to the rescue! 

Ech, a few articles from 2010 and 2012... 

Ooh, [this is a nice article](https://makeawebsitehub.com/reviews/domain-registrars/).

Looks like NameCheap it is. I'll follow up if they screw me over.

## Site Architecture

Next is how to actually implement the site. I don't really care for comments, my Wordpress site from 6-7 years ago had its comments system overrun by Russian hackers, which I blocked by disabling the comments section and removing them from the stylesheet, so I'd rather just not have them altogether. 

A static site generator looks to be the right tool here, and I could probably leverage GitHub for hosting. Jekyll is the obvious choice, except that it'd require that I learn Ruby for modifications and Ruby is... meh. Unless you're looking to hire me, in which case, I *love* Ruby!

Off to [Awesome Python](https://github.com/vinta/awesome-python#static-site-generator)... quite a few options here, looks like [Cactus](https://github.com/eudicots/Cactus) runs on Django, also meh... [Hyde](https://github.com/hyde/hyde) (I see what they did there) looks promising, but hasn't been updated in about two years. [Nikola](https://github.com/getnikola/nikola) seems like a strong contender, last update was a few hours ago, might be a bit heavyweight, though... [Pelican](https://github.com/getpelican/pelican) looks like a contender. [Tinkerer](https://github.com/vladris/tinkerer/) doesn't really look compatible with my knowledge base.

So... Nikola or Pelican. Both seem to have pretty [decent](https://fedoramagazine.org/make-github-pages-blog-with-pelican/) [tutorials](http://journalpanic.com/pyhi/posts/make-your-own-blog-with-github-pages-and-nikola/). Nikola looks to have a direct CLI for GitHub pages, which is nice, but also still looks fairly complicated. I think I'll go with Pelican for now, looks like it should be fairly easy to switch out if something doesn't pan out.

## Building the Site

Time to make a [GitHub Pages](https://pages.github.com), um, page? Go through the tutorial, la la la... okay done, go to the site... not working? Try again a minute later... now it works. "Hello World" indeed.

Okay now to set up Pelican. Hm, instructions also want me to make a `[username].github.io-src` repo, okay, done... now it wants me to install the Python Pelican package? I'm not on Fedora so, no. I should probably pip install in a virtualenv... all the tutorials say to have a dedicated Pelican virtualenv? That doesn't sound right, but whatever. 

Alright, looks like I should probably use this sequence:

```bash
python3 -m venv Pelican
cd Pelican
source bin/activate
```

Okay, now we're in our virtualenv. Now to install Pelican and a few dependencies:

```bash
pip3 install pelican
pip3 install Markdown typogrify
```

Okay, now we have our packages, time to set up the site. Doesn't seem right to do it here, maybe I can go back to the other directory?

```bash
cd ../catichenor.github.io-src
```

The next step, which I assume to be `git submodule add git@github.com:catichenor/catichenor.github.io.git output` from the tutorial, doesn't work. Eh, maybe just skip straight to:

```bash
pelican-quickstart
```

Okay done.

Now to write up a post based on the example from the docs:

---

Title: This is a test.
Date: 2018-03-20 15:18
Modified: 2018-03-20 15:20
Category: Site
Tags: pelican, publishing
Slug: this-is-a-test
Authors: Christopher Tichenor
Summary: This is a test page, hopefully I can delete this.

This is a test. This is only a test.

---

Make and test the site... wow that's ugly. Okay, at least let me change the post to this one I'm typing, I can update it later.

Okay, that looks better, kinda. Now to publish to GitHub. Eh, looks like I didn't do the submodule thing correctly. Ugh. Okay, `cp -r output/* ../catichenor.github.io/` to the rescue here. I'll need to automate that plus probably some other stuff.

Next on the todo list is to get the domain stuff set up and switch up the theme but at least we have a working site. Good night!