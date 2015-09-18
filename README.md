# GlintApp

![GlintApp Logo](GlintApp.png)

> GlintApp is a principle for writing web site and web apps with [node.js](https://nodejs.org).


# principles

it follows the unix principle.

 - Small is beautiful.
 - Make each program do one thing well.
 - tl;dr
 - read: http://blog.izs.me/post/48281998870/unix-philosophy-and-node-js
 - long version: ISBN-10: 1555582737

node.js in general follows the unix principle.

GlintApp follows this philosophy at the application level.

it promotes writing application modules that run on the server as well as in the browser.

# what is is
 - a way of writing modular for the server and the browser (universal javascript aka. isomorphic).
 - a guide of writing reusable applications.
 - working now, with the existing servers and browsers
 - fun, fun , fun

# what it is not
 - a framework
 - does everything for you
 - fancy

# implementations
 - [glintcms](http://glintcms.com)

# give me the details

[node.js](https://nodejs.org), [npm](https://www.npmjs.com) as well as [github](https://github.com/) are great to work with.
The whole environment encourages you to use and write many small modules, and not monoliths.
It just takes a couple of minutes, (or even less than a minute with tools at hand) to write a __hello world__ module, test it and publish it.
Comming from the Java world, this surely is an improvement.


Small modules help solving (or avoiding) complexity. Which is great. But it does not stop with small modules. Modules are composed again

## module levels

[animation](https://pbs.twimg.com/tweet_video/CJR5osMUAAAW0XO.mp4)

<video controls="" autoplay="" name="media"><source src="https://pbs.twimg.com/tweet_video/CJR5osMUAAAW0XO.mp4" type="video/mp4"></video>

We can divide the modules into three levels:

1. function level / like `is-browser`, to `debug`
2. library/framework level / like `express` or `jquery`
3. application level / `your application that solves your or your customers problems`

Structuring modules in the first two levels is basically solved.
How to structure the application (third level) is another thing.
It most probably is influenced by the libraries/frameworks you use.
If you are not careful, your application ends up beeing a monolith again, that is composed of many modules, but you can't reuse most of it.

> `GlintApp` focuses on the application level (third level).

Lets first look at node.js and the browser and then come back to how to compose the application.


## node.js and the browser

this section gives first a bit of a background, if you know this already, you can skip to the next chapter.

**Then there is modules for node.js and modules for the browser**

Now we have JavaScript on the Server with node.js and in the browsers:

1. But how is it different?
2. Can't we just use the same thing on the server as we use in the browser?

The environment is different. (here's just a few differences)
- Node.js does not have a DOM built in (there is the JSDOM Module, but that's a different thing).
- Browsers do not have the node.js API like `http` or `fs` etc.
- Browsers have different JavaScript Engines and do not always support the same things (worse with old browsers)
- and Browsers do not have a built in `require` mechanism, like node.js has

Because of the differences, it's the same language, but you can't share lots between the server and the browser out of the box.

You need some additional things...

### [component](https://github.com/componentjs/component)
TJ Holowaychuck back in 2012 started [component](https://github.com/componentjs/component) for modules to use in the browser.
There is also several similar things with similar names [disambiguation](https://github.com/componentjs/component/blob/master/disambiguation.md) and [comparison](https://github.com/componentjs/guide/blob/master/component/vs.md).
It uses [github](https://github.com/) as the repository, instead of [npm](https://www.npmjs.com), and has got it's own tooling for things like installing building etc.
It has got it's own commonjs and nodejs like `require` implementation. It's semantic is similar, but not the same. It add's the username into the naming scheme: `require('username/modulename')`.

What I really like about this project is the focus on small reusable modules, that don't require heavy frameworks/toolsets, and the involvement of many people (at least initially).
Many of them are very slick and have a nice and simple api. Now many of these modules can be found in [node.js](https://nodejs.org) as well.
Before you build a browser module yourself check the existing ones on component: [registry](http://component.github.io/) and [old registry]([node.js](https://nodejs.org)).

But do we really need another module registry?


### [browserify](http://browserify.org)
Why not writing code for the browser as we do for the server and use a tool to make the browser happy?
James Halliday known as substack created such a tool: [browserify](http://browserify.org)
Browserify works really great and also solves the problem of writing reusable code for node.js and the browser.
It even gives you big parts of the node.js API to use in the browser.
And it does not create another 'universum', where the modules written in/for it, can only be used within this universum.
Which is great.

With all the problems it solves, it has some things it does not solve by itself, and also adds some new problems:
- the bundled javascript file can become quite big
- it does not handle other assets like images, css files etc.

Over all, it is a great thing, I am really thankful for.
One thing I didn't mention so far is, the documentation as well as the eco system are exceptional too:

- [documentation](https://github.com/substack/node-browserify#browserify)
- [handbook](https://github.com/substack/browserify-handbook)
- [node modules](https://github.com/substack/node-browserify#compatibility)
- [tools](https://github.com/substack/node-browserify/wiki/browserify-tools)
- [plugins](https://www.npmjs.com/browse/keyword/browserify-plugin)



### there is many package managers for the browser

Sure there is great things you can use for the browser.
For example [duo](http://duojs.org), [webpack](https://webpack.github.io), [bower](http://bower.io/) etc.

Most of them are focussed on the browser only. And don't have reusing node.js code in mind.

There is a recommendable read on the story of normalize.io, which had the nobal goal to normalize the package management [normalize.io](http://www.jongleberry.com/the-story-of-normalize.html).


### what about webcomponents, HTTP/2 etc.

#### HTTP/2
HTTP/2 has got an influence on how to pack, or not to pack assets for the browser with the Server Push mechanism.
Previous best practices (like bundling javascript files) become anti patterns with HTTP/2

**support**

- HTTP/2 is currently not supported everywhere yet
- there is currently no node.js core support for http2 (but in userland ther is)
- nginx support for example is not yet there (in process)
- browser support: [caniuse http/2](http://caniuse.com/#feat=http2)

#### Web Components
Web Components can help re-using html, css, javascript, they are just a bit late, and support is still not where it should be.

**further reading**

- documentation: [mozilla web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
- browser support: [caniuse web components](http://caniuse.com/#search=web%20components)
- Google Polymer is based on web components: [polymer](https://developer.mozilla.org/en-US/docs/Web/Web_Components)


## structuring the application
The question often arises in node.js projects, how to structure the application.
As mentioned before, often the libraries/frameworks you use influence your structure quite a bit.

Lance Pollard initiated a listing of different structures: [example structures](https://gist.github.com/lancejpollard/1398757).
You can also find many blog posts about how to structure a node.js application (us included: [our own structure struggles](http://intesso.com/articles/three-essentials-for-open-source-application-design) )

But there is more...

... come back soon.



# initiated by
Andi Neck | andi.neck@intesso.com | intesso

# get involved
Any Feedback is highly appreciated.
Please create an [Issue](https://github.com/glintapp/glintapp/issues/new) or [PR](https://github.com/glintapp/glintapp/pulls).
I'm happy to add you as a comitter too.
