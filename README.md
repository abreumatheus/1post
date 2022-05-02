# 1POST

A tiny, fast and fun static site generator for quick blogging. 1POST is written entirely in NodeJS and has no dependencies. You can install as a global package or run directly from NPX. This is the Lighthouse results for 1POST generated pages:


<img width="566" style="margin: 0 auto; display: block" alt="Screen Shot 2022-05-02 at 03 00 52" src="https://user-images.githubusercontent.com/31618881/166191359-67e7e736-af0c-478d-a310-7434b90243b2.png">

## Installation

To install 1post, just run on a terminal:

```
npm install -g 1post
```

Now if you type `1post` on your terminal, you must see a "help" output.  
If you prefer not to install it, you can execute all the commands directly on NPX.

## Usage

Create a new folder for your blog, and from this folder root run:

```bash
1post start
```

You just created a new blog on the current folder, we will talk about its files soon.  
Now just run:

```
1post my-first-post
```

A new post has been created on the folder `/posts`. You can edit the post by editing the file `/posts/my-first-post/post.html`, this file is your post and will be automatically processed on every build. Now run:

```
1post build
```

Now your static blog has been assembled and is ready for deploy.  
If you want to test the results, just type:

```
1post serve
```

This command will serve the current blog locally on port 8080 for tests purposes only.

## Commands

1POST has 5 commands only, as mentioned on its help:

```
1POST is a CLI to create and manage a very simple static generated blogs via NPX
For more information: https://github.com/felippe-regazio/1post

Usage:
  npx 1post {command}|{postslug}

Commands:
  help: show this help
  start: start a new blog on the current folder
  build: updates the blog index page feed with newer posts
  serve: serves the blog locally under the localhost:8080 address

Blogging:
  To create a new post, just run "npx 1post {postslug}"
```

## Configuring the blog

To start a new blog, create a folder then run

```
1post start
```

This command must be executed only one time by folder. If you run it on a ready-existent blog, your custom files will be overwrited. After run the command, you will see 5 new files and a `posts` folder.

```
blog/
  posts/
  blog-config.json
  card.png
  index.html
  template-index.html
  template-post.html
```

### blog-config.json

This file is responsible for you blog global configurations, its entries are available on every template file at 1post (even post files) by interpolating the key name like this {{blog_title}}. You must fill all the fields on this file as explained:

|key|description|
|---|---|
|blog_locale|A ISO locale to use as your blog locale, ex: en, fr, pt-BR|
|blog_title|Your blog title|
|blog_description|Your blog description|
|blog_theme|You can choose a blog theme or create one by yourself, see the `Theming` section of this doc to know more|
|blog_author|Probably your name|
|blog_author_url|An URL to know more about you, ex: LinkedIn, Facebook, etc|
|blog_no_posts_hint|Phrase to show when the blog list is empty|
|blog_posted_by_hint|Phrase to but before credits, ex: "Posted by"|
|blog_url|Your blog deployment URL, ex: "http://myblog.com"|

You can create new entries on this file, but you must not delete or ovewrite the existing ones. Lets imagine you created an entrie called `dog_name` like this:

```
{
  ...
  dog_name: "Ruffus"
}
```

Now you can use {{dog_name}} on every template file, if you want to show your dog name, just time on any template html (template-* file or post.html file):

```html
<h3>{{dog_name}}</h3>
```

That will be compiled to

```html
<h3>Ruffus</h3>
```

### card.jpg

This is the SEO card image. When using metatags to show a preview about your content, this image will be used as cover, you can changed it as you want. Recommended size: 1200px x 627px. 

### index.html

This is your blog index. This file is automatically generated, to edit your index you must edit the template-index.html file.

### template-index.html

This is your blog Home template. You can edit it as you want, add new assets, images, dependencies, whatever. You can also interpolate any date from `blog-config.json` as mentioned above. An important thing to keep is the special interpolation `{{posts_feed}}`, is where your post list will rendered. Everytime you run `1post build`, this template is used to generate a new `index.html` page.

### template-post.html

This is your post template. Every post you write will respect this "layout". You also can edit it as you want, change anything you want. Here you can also interpolate any key from the `blog-config.json`, and also from your `post configuration header` which we will talk about later. The special interpolation key `{{post}}` is used to bind your post content to this layout, dont remove it.

## Writing a Post

To write a post, you must type `1post {postslug}`, where post slug will be the address to your post and also its folder name on `/posts` folder. Lets create a post. After start your blog (read the "Configuring the blog" section of this doc), now run:

```bash
1post my-first-post
```

A new folder `/posts/my-first-post` was created with the following files

```
posts/
  my-first-post/
    index.html
    post.html
```

The `index.html` file is an automatically generated file, it binds your `post.html` file to your `template-post.html` file and parsed to build the final version of your post. Now, to write your post, just edit the file `post.html`.

When opening your `post.html` file, you will see a strange notation on top. You must not remove or change this notation structure, but you can add new items to it. This is your post-metadata, you must fill the post Title and Description there, it will be used to SEO and to render your post H1 heading.

```
<!--:::{
  "post_title": "Post title",
  "post_description": "Post description",
  "post_created_at": "Mon May 02 2022 01:35:03 GMT-0300 (Brasilia Standard Time)"
}:::-->
```

You can also add new entries to the post-metada if you want, for example: `"favorite_food": "garlic"`, and then retrive its value on the post by interpolating `{{favorite_food}}`. You can also bring any value from the `blog-config.json` file by interpolation. After fill the post-metadata, just write your post as a simple and normal HTML file.

## Building

After write or edit a post, you must rebuild your blog, to do it just run

```
1post build
```

This will reconstruct your index page with the new posts feed, and will also parse the new and modified posts. Remember to run this command before every deploy.

## Serving

If you want to serve your blog for test purposes, just run:

```
1post serve
```

## Theming

You can change your blog theme on the file `blog-config.json` by editing the key `theme`. It supports the following values:

```
black-and-white
dark-blue-ocean
default-dark
default
fluorescent-green
skinny-theme
solar-theme
solid-pink
sweet-carnival
wooden-theme
```

You can preview any of this themes by visiting https://felippe-regazio.github.io/plume-css/ and clicking on the button in left bottom corner of the screen. You can also create your own theme, if you desire to create your theme, please read the Plume-CSS documentation.

1Post uses Plume-CSS for styling. Plume is a very simple, powerful and lightweight CSS-Only Microframework created by same creator of 1Post. So, anything Plume's can do, 1Post can also do. You can check Plume's documentation here: https://felippe-regazio.github.io/plume-css/.

## Static Assets

Everything in 1POST is just simple HTML combination and interpolation, there is no complex building process, just add your assets as you would do on a normal HTML file. A tip: Try to keep header assets and script tags on the `template-*` files.

## Cache

1POST has a cache strategy to process only new and/or modified posts on a new build. If want to clean the cache and rebuild all the posts from scratch, just delete the file `/posts/cache.json`.

## Deploying

Your blog is a simple collection of static files, just drop its folder on the server and go get a coffee ;)

## Contributing

PR's are welcome. 1POST has a ridiculously simple code architecture, and its built under a dumb imperative paradigm. The index.js dispatch the commands, and all the commands logics are on the `cli` folder.

## 1POST Philosofy

1POST is very small and really, i mean really simple. Is indicated if you want to write quick, pretty, fast and powerful HTML+CSS only posts, specially technical posts. All the posts will be on the same level in a unique list on the Home, and identified by Title and Date. 

1POST has no search bar, no tags, no footer, no header, no markdown, no JS frameworks, no JS at all, no complex categorization features, almost nothing; only the good and old HTML+CSS. When i said simple i mean: VERY simple (but powerful) blog. It has a fast CLI to manage Posts and Templates and build the blog, it also automatically configures: Style, Themes, Acessibility and SEO.

1POST is indicated if you want a Content-First fast and quick blog with "as simple as possible" philosofy in mind. For example: we have a single post list on the Home beacause even if you have a 1.000 itens list there, the payload is smaller and faster than a entire JS framework and hundreds of JS code lines to create pagination and search which, for a single purpose of Content-First, its just too much.

If you need more features, complex customizations, deep designed pages and a deeper kind of control and content categorization, 1POST it's not for yout, there is a plenty of other options out there that can manage your needings as a breeze and infinitelly better then 1POST. But if just want to quick blog your posts really quick with a cool design and a great performance, 1POST will make you happy.
