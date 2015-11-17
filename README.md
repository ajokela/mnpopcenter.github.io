# The MPC IT Blog
 
This is the MPC IT Blog repository.  This blog is managed using the Jekyll blogging platform and is published as a GitHub Pages site.  Github Pages is a web site creation tool that is integrated into Github.  It uses Jekyll under the hood to automatically rebuild a web site any time changes are committed to the master branch of a specially-named repository.  The site itself is available at <http://tech.popdata.org/>.

The advantage to this approach over other blogging platforms (e.g. Wordpress) is that the resulting site is purely static - there is no PHP or database to worry about.  And since it's hosted at Github, there is no web hosting service to worry about, either.  It also allows the publishing pathway to be based on git, which is already familiar to MPC IT staff.

The blog is managed like any other git/Github repository.  Blog editors manage the master repository.  Blog authors create new content on forks and then submit pull requests to the editors.  The author and editor will collaborate on the pull request, and once the post is ready the editor will merge it into the master branch of the main repo, triggering a site rebuild to publish the new content.

## Instructions for Blog Authors

### TL;DR

1. Fork this repo.
2. Clone your fork locally.
3. Create a file in the _posts directory called YYYY-MM-DD-this-is-my-title.markdown
4. Start the file with this block of yaml delimited by "---", called "Jekyll front matter":

    ```
    ---
    author: <your x500 here>
    title: This is the Title of My Post
    teaser: This is a short teaser about my post.  I'm going to talk about cool things!
    categories:
    - IPUMS
    - Data Processing
    tags:
    - Ruby
    - CSV
    ---
    ```
    Categories and tags are optional and can be omitted.
5. Add your markdown-formatted blog content below the front matter.
6. (First time only) Run `bundle install`
7. Run `jekyll serve --config _config.yml,_config_dev.yml`
8. Open http://localhost:4000/ (or whatever URL Jekyll reported that it used in the last step) and check your work.
9. Iterate.  The local server will reflect changes automatically by detecting when files change.
10. When done, do `git add . && git commit -a && git push`
11. Submit a pull request back to this repo.  If you want someone else to do an editing pass on it before merging, you need to add their github.com account as a read/write collaborator on _your fork_ of the repo.

You can reuse the same git clone to do future blog posts, too.  You might want to merge upstream first, but it's actually not necessary.

You really ought to read the rest of these instructions, though.

### <a name="content_reqs">Creating Blog Post Content</a>

Jekyll posts are mostly just text files.  You can write your content in the editor of your choice.

By incorporating special syntax into your posts, you have access to a good deal of Jekyll functionality.  Each blog post file should start with front matter (described in the next section).  That signals Jekyll to run this file gets run through the Jekyll engine, which means that at site generation time, the file gets processed by an input converter (e.g. Markdown -> HTML or Textile -> HTML) and also by the template engine, which is called Liquid.  Liquid tags allow you to do things like invoke the syntax highlighting code block feature or retrieve the site URL value.

For this site we have standardized on markdown as our input format.  Specifically, [Kramdown markdown](http://kramdown.gettalong.org/syntax.html).

#### Blog Post Metadata - Jekyll front-matter

All Jekyll blog posts need to start with a YAML section called the "front matter".  This is the metadata for your post.  Front matter is delineated by three dashes "---".  Here is an example block of front matter:

~~~
---
title: 'Our IT Hiring Process: How and Why'
teaser: 'An overview of our IT hiring process and why we designed it this way.' 
author: fran
categories:
- Team
tags:
- hiring
- staff
---
~~~

Most front matter variables have site-wide defaults, so you don't need to worry about setting them in each post.  The minimum set of front matter fields you should include in each post are title, author and categories.  A teaser is strongly encouraged, and tags are optional but also encouraged.  There are other variables which can be used to change the layout of a post or otherwise alter behavior, but the site editors would generally add these if needed.  

For the `categories` variable, the set of allowable values is XXX coming soon XXX.

#### Blog Post Content - Kramdown Markdown

After the end of the front matter, the rest of the file contains your blog post content.  Use [Kramdown markdown](http://kramdown.gettalong.org/syntax.html) syntax to write your post.  Most, but not all, of the Kramdown functionality will work on Github Pages (this is mostly relevant for code blocks, read below on how to do this correctly).  All the usual stuff works, such as lists, headers, links, inline images, tables, and so on.

#### Including Code in Your Posts

Jekyll on GitHub Pages uses Pygments to provide syntax-highlighted code blocks in posts.  To use Pygments, use the tags `{% highlight _lang_ %}` and `{% endhighlight %}` to surround your code.  For example:

~~~
{% highlight ruby %}
flr_file.each_record do |record|
  customer = Customer.new
  customer.name = record.name
  customer.street = record.street
  customer.zip = record.zip
  customer.save
end
{% endhighlight %}
~~~

(I can't show you what this would produce on the blog because Github doesn't apply Jekyll to the page you're currently reading, so it doesn't recognize Liquid tags).

Pygments offers hundreds of languages.  Some useful ones for us are ruby, python, python3, perl, sql, bash, java, css, yaml, c, c++, js, json, html and text.

_Warning: Do not try to use other flavors of markdown fenced code blocks (e.g. "~~~" or "```").  They will not work in the Jekyll/GitHub Pages ecosystem._

To create inline code snippets, use markdown code spans with the single backticks.  These _do_ work on GitHub Pages sites.  For example:

~~~
At the shell, type `gem install hflr` to install the hierarchical fixed length records gem.
~~~

produces

At the shell, type `gem install hflr` to install the hierarchical fixed length records gem.

#### Including Images in Your Posts

If you want to include images in your posts, add them to the images directory in the repository.  Then, to reference them in your post, use this combination of markdown syntax and Liquid tag:

`![Image Alt Text]({{ site.url }}/images/filename.png)`

### Authoring Workflow 

There are two basic workflow approaches for authoring blog posts, each with its own advantages.

####  Method 1: Locally by Cloning the Repo and Running Jekyll Locally

* Pros: Edit on local machine with your preferred tools.  Ability to preview locally (with Jekyll properly installed locally).
* Cons: Additional one-time setup required. Still have to go to Github to create pull request.
* Best for: Authoring more complicated posts. Collaborative editing with others.  

_Note: If you don't have Jekyll installed, please see [Instructions for Installing and Running Jekyll Locally](#install) first._

1. Fork this repository on github.com.
2. Clone it locally.
3. (Optional) Create a branch.  Probably only necessary if you want to work on multiple posts at once.
1. Navigate into the _posts directory.
1. Create a new file in the _posts directory. Use the naming scheme "YYYY-MM-DD-title-of-my-post.markdown" for your new file.
1. Create your post. Follow the directions for [Creating Blog Post Content](#content_reqs) below.
1. Start your Jekyll server if you haven't already, and preview your work.
1. When done, add your new file, commit your changes, and push to your forked repo to Github.
1. Go to the Github web interface for the forked repository, and [create a pull request](https://help.github.com/articles/creating-a-pull-request/) back to the master branch of the main repo. 

#### Method 2: Online via the Github Web Interface

* Pros: Everything can be done from the web.
* Cons: No way to preview the post.
* Best for: Very simple posts. Posts written externally that you just want to cut and paste. Fixes to existing posts. 

1. If you don't already have a fork of the repository:
  1. Open a browser to the repository: <http://github.com/mnpopcenter/mnpopcenter.github.io>.
  1. Fork the repository.
1. If you already have a fork of the repository (and want to keep it instead of just tossing it out and doing the step above instead):
  1. Navigate to your fork.
  1. Update your fork from the upstream source (see [here](http://stackoverflow.com/questions/22318145/update-github-fork-from-web-only-in-other-words-without-git) for a way to do this right from the web or [here](http://stackoverflow.com/questions/7244321/how-to-update-github-forked-repository))
1. Navigate into the _posts directory of your fork.
1. Click the + icon to create a new file in the _posts directory. Use the naming scheme "YYYY-MM-DD-title-of-my-post.markdown" for your new file.
1. Create your post. Follow the directions for [Creating Blog Post Content](#content_reqs).
1. When done, submit a pull request back to the main repo.

## <a name="install">Instructions for Installing and Running Jekyll Locally</a>

1. Clone your forked repository to your local machine.
1. (Optional) Create a new gemset in your Ruby environment.
1. From the root of the repository, run `bundle install`.

Now you have Jekyll and its dependencies installed.  To start the Jekyll server:

1. Run the command `jekyll serve --config _config.yml,_config_dev.yml`
1. The site should be viewable at http://localhost:4000

Note that Jekyll will report that it started on http://0.0.0.0:4000/, but at least on my system the server is not reachable at this address, you have to use http://localhost:4000/.  

You can keep the server running, and it will automatically detect changes to files in the repository and rebuild the site accordingly.  At this point you can iterate over your blog post authoring using Method 2 above, and preview the site locally as you go.
