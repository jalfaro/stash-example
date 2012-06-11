Stash Example
=============

Simple example for using a DRY approach with Stash.

Since listing to the [EE Podcast](http://ee-podcast.com/episodes/dry-techniques-with-ee) with John Rogerson discussing a DRY approach to developing EE templates with Stash I have spent some time trying to figure out how to work using this method. It has taken some time to finally get my head round the main topics but has been really worth the learning. I write this with one main caveat, I confess I am still not an expert on the following areas:

* The parse order
* Using Stash
* The DRY approach

but with the help of a great tutorial on [EE Insider](http://eeinsider.com/articles/template-partials-using-stash/) and through help form the EE community on Twitter I have certainly gained a lot more knowledge than I had before I started.

This will not be a comprehensive tutorial on using Stash but I will try to explain what worked for me from a beginners point of view, and how I have worked through the different resources available. Before even beginning to use Stash it is well worth reacquainting yourself with the [parse order](http://loweblog.com/downloads/ee-parse-order.pdf), as this is absolutely key to not spending hours pulling your hair out, Low has recently written [another great article](http://gotolow.com/blog/parse-order-and-low-variables) which helps with figuring this out.

To start with I created two fairly standard templates which would be used in most every day site builds - a news articles template that list all the latest news articles and a single article template. These would both use a wrapper template which Adrienne Travis talks about in her article [Template Partials using Stash](http://eeinsider.com/articles/template-partials-using-stash). The wrapper template is used to hold the standard structure for the site- document head, header, and footer.