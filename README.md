##Using Stash For The First Time

Since listing to the [EE Podcast](http://ee-podcast.com/episodes/dry-techniques-with-ee) with John Rogerson discussing a DRY approach to developing EE templates with Stash I have spent some time trying to figure out how to work using this method. It has taken some time to finally get my head round the main topics but has been really worth the learning. I write this with one main caveat, I confess I am still not a complete expert in following areas:

* The EE parse order
* Using Stash to its full potential

but with the help of a great tutorial on [EE Insider](http://eeinsider.com/articles/template-partials-using-stash/) and through help form the EE community on Twitter I have certainly gained a lot more knowledge than I had before I started.

This will not be a comprehensive tutorial on using Stash but I will try to explain what worked for me from a beginners point of view, and how I have worked through the different resources available. Before even beginning to use Stash it is well worth reacquainting yourself with the [parse order](http://loweblog.com/downloads/ee-parse-order.pdf), as this is absolutely key to not spending hours pulling your hair out, Low has recently written [another great article](http://gotolow.com/blog/parse-order-and-low-variables) which helps with figuring this out.

###Getting Started
To start with I created two fairly standard templates which would be used in most every day site builds - a news articles template that list all the latest news articles and a single article template. These would both use a wrapper template which Adrienne Travis talks about in her article [Template Partials using Stash](http://eeinsider.com/articles/template-partials-using-stash). The wrapper template is used to hold the standard structure for the site which generally doesn't change from page to page: document head, header, and footer. The file I created can be found [here](https://github.com/expressionengine/stash-example/blob/master/_wrappers.group/_main.html). This can be seen as the skelton of the page, it has all the main elements of the document and where possible I have created snippets to keep the template clean and easy to maintain:

* [document head](https://github.com/expressionengine/stash-example/blob/master/snippets/sn_document_head.html)  (stored in a snippet).
* [header](https://github.com/expressionengine/stash-example/blob/master/snippets/sn_header.html) (stored in a snippet).
* [aside](https://github.com/expressionengine/stash-example/blob/master/snippets/sn_aside.html) (stored in a snippet).
* [footer](https://github.com/expressionengine/stash-example/blob/master/snippets/sn_footer.html) (stored in a snippet).

###Template Partials
Template partials are the main content which will change from page to page, the way I see it is: if the wrapper is the bread of the sandwich then the partials are the filling. First of all I will explain the [single article template](https://github.com/expressionengine/stash-example/blob/master/news.group/article.html) as this is the easiest place to start.
 
####[Single Article Template](https://github.com/expressionengine/stash-example/blob/master/news.group/article.html)
You will notice there are three Stash variables within this template, these are the variables which pull in the content which is specific to that page. They add in the id for the body tag, the main content, and the pagination for the news articles (I'll explain this later on).  Lets run through the [single article template](https://github.com/expressionengine/stash-example/blob/master/news.group/article.html) from top to bottom.

The first part of the file (line 1) embeds the wrapper template using a standard embed, this is the main structure and markup for the page and could actually be placed anywhere in the template, I think it makes sense to store this at the top of the file:

	{embed="_wrappers/_main"}

Next (line 2) comes the first Stash variable which sets the body id to whatever segment_1 in the url is (in this case it would be news as it is my template group name):

	{exp:stash:set name='bodyId'}{segment_1}{/exp:stash:set}
	
The main content for the page is fairly standard; it uses a channel entries tag to output a single entry from the news articles channel. The clever part is that this channel entries tag is then wrapped in a Stash variable (line 4):

	{exp:stash:set name="mainContent"}

the name I have given to this variable is "mainContent" but it could be anything you like - "ham", "cheese" whatever you fancy!
That's it for the single article page, its as easy as that.

####[News Index Page](https://github.com/expressionengine/stash-example/blob/master/news.group/index.html)
The second page we need to run through is the [news index page](https://github.com/expressionengine/stash-example/blob/master/news.group/index.html) which will show 5 articles then use pagination to display the rest.

The first two lines do exactly the same as the single article template and we also use a channel entries tag to display 5 entries followed by outputting the pagination. You will notice that the articles are output using an ordered list and because your pagination variables need to be inside the channel entries tag this means that I have an ordered list with a div at the end containing the pagination. This has always bugged me as it is not valid markup, you can not embed a div in a ul or ol unless its within a set of li tags and strictly speaking the pagination is not part of the list. Do not fear though Stash comes to our rescue.

To move the pagination outside of the ol but keep it within the channel entries tag we have to do something a little different with the Stash tags. You will notice that at line 4 and 5 the stash tags are written:

	{exp:stash:set parse_tags="yes"}
	{stash:mainContent}

These are Stash tag pairs and the first one wraps the content but doesn't have a name, the reason for this is to enable multiple stash tag pairs to be used inside it (one for the content at line 5 and one for the pagination tags at line 25), it can be a little confusing at first but all you are really doing is creating a big stash wrapper and then naming the elements you want to stash inside this wrapper. Once you have created this template you are all set.

##So How Does It Work?
The news index page is visited, the EE parser runs through the template code and because it is wrapped in a{exp:stash:set} tag pair it "stashes" the code for use later, it then embeds the _wrappers/_main template and parses all the code in that template. The wrapper template calls the Stash data using the {exp:stash:get} variables:

 * one to get the mainContent - {exp:stash:get name="mainContent"}
 * one to get the pagination - {exp:stash:get name="pagination"}

and spits out the html to the page, simple right? Well that's my understanding of how things work, I am sure there are far better technical descriptions of what actually happens but hopefully that all makes sense. The single article page works much in the same way except as it is calling the same _main wrapper template it doesn't show the pagination because it is not required and that is why I wrapped it in a conditional.
  
##Going Forward
There are so many more clever things you can do with stash and once you have the basics down it's easy to start copying out the method above for all your other site pages. I hope this has given a short insight into how I found working with Stash for the first time and please feel free to make comment/suggestions either on [github](https://github.com/expressionengine/stash-example) or in the comments on [my site](). 

