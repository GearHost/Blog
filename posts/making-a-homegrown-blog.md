#Making a Homegrown Blog

At GearHost things are moving faster than ever. To help keep in touch with you and be more transparent we've built this pretty simply yet snazzy blog to help. Building a blog is easy or so we thought. Here's a little journey of how we're leveraging some .NET code and [GitHub](https://github.com/) to create the official GearHost blog. Here's how.

Yeah everyone has a blog but we wanted something more simple so we looked initially to [Ghost](https://ghost.org/) which BTW is a great blogging platform. In fact you can [one click install it on a CloudSite for free](https://my.gearhost.com/account/signup) if you haven't tried it already. However as great as Ghost is we wanted something more simple. Plus we've got to keep our developers employed so we set them off to make a little magic.

We've decided to use a [public GitHub repo](https://github.com/GearHost/blog) for multiple reasons but mainly why not, it's cool. It stores the markdown and graphics and the folder structure is pretty simple.

###\master.json
This base file contains all the metadata for all articles that we display in JSON. It contains items such as the id, date, title, summary, author, etc.

###\posts
This contains all the blog posts formatted as markdown files with a .md extension. Posts are published to GearHost in the path https://www.gearhost.com/company/blog/{article-name-without-md}.

* **Post filenames:** Are formatted by the post name, such as *lets-do-this*. We use all lowercase letters and dashes (-) to separate the words.

* **Media subfolders:** The \posts folder contains the \media folder, inside which are subfolders with the images for each article including the header graphic. The post image folders are name identically to the article file, minus the *.md* file extension.

###\project
This will contain all the source code we used to build the GearHost blog.

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

That's basically it, really simple stuff but effective. Enjoy and we'll upload the project to the public repo shortly.

> Something to keep in mind is GitHub does rate limit requests however there are several ways to mitigate this per GitHub. Even these can't help blogs with a lot of requests so we've implemented caching techniques to keep the data in memory.
