At GearHost things are moving faster than ever. In an effort to keep more in touch with you and keeping GearHost transparent as a whole we've built a pretty simply yet snazzy blog platform. Instead of writing our first blog post about how we've released a blog (boring!) I figured our first blog would be about how we did it. After all we're talking to developers and designers here. So on that theme let's get started...

One of the main features of the new GearHost platform and GearHost as a whole is to make very complex and advanced systems extremely easy to use. Simple cloud hosting is what it's about. None of this complicated bull when all you want is rock solid hosting for your awesome app. With that in mind we started looking at blogging platforms. We looked at a few like Ghost, WordPress, DasBlog, BlogEngine.NET etc but didn't find anything we really fell in love with other than Ghost. It's simple and we like simple clean interfaces but the API is lacking so to the hell with it we said and we decided to build our own.

> BTW all those platforms I mentioned above can be one click installed on a CloudSite for free if you haven't tried already. Just signup for free and create a blog in seconds.

###Getting Started
We knew we wanted to use markdown for the blog posts so we needed to figure out a way to make collaboration easy between employees and even contributors from the community. We do this currently with our documentation leveraging GitHub and their API. Well that's easy, check.

The next problem is graphics. We need our design team to put together blog post images that represent the discussion nicely so we proposed the idea and voila we've got ourselves some images. Check.

Finally how do we put this whole thing together? We got with our developers and we set out a plan to put all the pieces together. This was fairly simple. As I mentioned we do this with our Documentation so it was more about fine tuning things. The only real big issue is GitHub's rate limit on requests. This post and more to come will get read by thousands of people so we need to make sure we don't run into a rate limit issue with GitHub. To solve that we implemented some caching. Pretty cool stuff.

So let's take a look at the [GitHub repo](https://github.com/GearHost/blog) for this stuff. Here's our directory structure and some important files that make this work.

###\master.json
This base file contains all the metadata for all articles that we display in JSON. It contains items such as the id, date, title, summary, author, etc.

###\posts
This contains all the blog posts formatted as markdown files with a .md extension. Posts are published to GearHost in the path https://www.gearhost.com/company/blog/{article-name-without-md}.

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