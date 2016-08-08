At GearHost things are moving faster than ever. In an effort to keep more in touch with you and keeping GearHost transparent we've built a pretty simply yet snazzy blog platform. Instead of writing our first blog post about it being just that, our first blog post (boring!), I figured it could be about how we build this blog platform (that's cool!). After all we're talking to developers and designers here. So on that theme let's get started.

One of the main features of GearHost as a whole is to make very complex and advanced systems extremely easy to use. Simple cloud hosting is what it's about. None of this complicated bull when all you want is rock solid hosting for your awesome app. With that in mind we started looking at blogging platforms. We looked at a few like Ghost, WordPress, DasBlog, BlogEngine.NET etc but didn't find anything that was simple and integrated well with our main site. While we did fall in love with Ghost along with it's simple and clean interface the API wasn't up to par. So to the hell with it we said, we'll build our own.

> BTW all those platforms I mentioned above can be one click installed on a CloudSite for free if you haven't tried already. [Just signup for free and create a blog in seconds.](https://my.gearhost.com/account/signup)

###Getting Started
We knew we wanted to use markdown for the blog posts so the objective was to find an easy way to collaborate the markdown files between employees and even contributors from the community and the blog platform. We actually already do this currently with our [Documentation](https://www.gearhost.com/documentation) by leveraging GitHub and their API. Okay, that was easy. Check.

The next problem is graphics. Not some cheesy graphics but something that tells the blog story by a picture, something catchy. I went to our designers and had them put together some concept ideas. They came back with what I thought was some impressive creative, and quick at that. Check.

Finally was putting all these small moving parts into a blog. I got with the development team and we set out a plan on how to make this all work. It was fairly simple which was purpose built into the architecture. As I mentioned we already do some of this with our Documentation so it was more about fine tuning things. The only real big issue is GitHub's rate limit on requests. This post and more to come will get read by thousands of people so we needed a way to make sure we don't run into a rate limit issue with GitHub preventing content from serving. To solve that we implemented some caching using [Memcached](http://memcached.org/) which we use in almost all our code. Pretty cool stuff.

So let's take a look deeper look at the [GitHub repo](https://github.com/GearHost/blog). Here's our directory structure and some important files that make this work.

###\master.json
This base file contains all the metadata for all articles that we display in JSON. It contains items such as the id, date, title, summary, author, etc.

###\posts
This contains all the blog posts formatted as markdown files with a .md extension. Posts are published to GearHost in the path `https://www.gearhost.com/company/blog/{article-name-without-md}`.

* **Post filenames:** Are formatted by the post name, such as *lets-do-this*. We use all lowercase letters and dashes (-) to separate the words.

* **Media subfolders:** The \posts folder contains the \media folder, inside which are subfolders with the images for each article including the header graphic. The post image folders are name identically to the article file, minus the *.md* file extension.

###\project
This will contain all the source code we used to build the GearHost blog so you can make your own version as well.

###Parsing GitHub markdown to HTML
So the next step is to get the markdown file from GitHub then parse it. Using the [GitHub API](https://developer.github.com/v3/) we can download the markdown directly. We built a GitHubHelper class that facilitates this and other functions. Anyway here are some simple bits on how we did it:

```C#
	public static string ParseMarkDownToHtml(string markDown)
	{
		string url = "https://api.github.com/markdown";
		var data = new Dictionary<string, string>();
		data.Add("text", markDown);
		data.Add("mode", "markdown");
		data.Add("context", "publicrepo/path");
		var stringData = new JavaScriptSerializer().Serialize(data);

		using (var webClient = new WebClient())
		{
			webClient.Headers.Add("User-Agent", "username");
			webClient.Headers.Add("Authorization", "Basic " + Convert.ToBase64String(new ASCIIEncoding().GetBytes("username:password")));
			return webClient.UploadString(url, "POST", stringData);
		}
	}
```

That's basically it, really simple stuff but effective. Enjoy and we'll upload the project to the public repo in the next few days.

Oh and BTW this is our very first blog post. Ahhhh yeah.

By: [Ryan Kekos](https://twitter.com/ryankekos)
