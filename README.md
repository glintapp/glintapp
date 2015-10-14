<table>
<tr>
<td>
<img src="glintapp-logo.png" alt="GlintApp Logo">
</td>
<td>

<h1>GlintApp</h1>
<pre>is a principle </pre>
<pre>for writing modular web site and web apps </pre>
<pre>with <a href="https://nodejs.org">node.js</a></pre>

</td>
</tr>
</table>


GlintApp does nothing new, it just follows the unix principle (which node.js follows in general).

The main point is the focus for doing this at the application level:

*Small is beautiful, Make each program do one thing well.*


<table>
<tr>
<td>

# what is is
 - a way of writing modular for the server and the browser (universal javascript aka. isomorphic).
 - a guide of writing reusable applications.
 - working now, with the existing servers and browsers
 - only for the application level. see: [module levels][]
 - fun, fun , fun

</td>
<td>

# what it is NOT
 - a framework
 - does everything for you
 - fancy

</td>
</tr>
</table>


# background information

[node.js](https://nodejs.org), [npm](https://www.npmjs.com) as well as [github](https://github.com/) are great to work with.
The whole environment encourages you to use and write many small modules, and not monoliths.
It just takes a couple of minutes, (or even less than a minute with tools at hand) to write a __hello world__ module, test it and publish it.
Comming from the Java world, this surely is an improvement.


Small modules help solving (or avoiding) complexity. Which is great. But it does not stop with small modules. Modules are composed again.

## module levels

<table>
<tr>
<td>
<img src="lego-modules.png" alt="Composable Modules - LEGO">
found on www.gifbay.com
</td>
<td>

<code>
We can divide the modules into three levels:

1. function level / like `is-browser`, to `debug`
2. library/framework level / like `express` or `jquery`
3. application level / `your application that solves your or your customers problems`
</code>

</td>
</tr>
</table>



Structuring modules in the first two levels is basically solved.
How to structure the application (third level) is another thing.
It most probably is influenced by the libraries/frameworks you use.
If you are not careful, your application ends up beeing a monolith again, that is composed of many modules, but you can't reuse most of it.

> `GlintApp` focuses on the application level (third level).

Lets first look at node.js and the browser and then come back to how to compose the application.

## TL;DR

if you want to take a shortcut to the `how` and skip the `why`, jump to : [Steps to a GlintApp Galaxy]


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
- nginx support for example is not yet there (in process) update: it's supported now
- browser support: [caniuse http/2](http://caniuse.com/#feat=http2)

#### Web Components
Web Components can help re-using html, css, javascript, they are just a bit late, and support is still not where it should be.

**further reading**

- documentation: [mozilla web components](https://developer.mozilla.org/en-US/docs/Web/Web_Components)
- browser support: [caniuse web components](http://caniuse.com/#search=web%20components)
- Google Polymer is based on web components: [polymer](https://developer.mozilla.org/en-US/docs/Web/Web_Components)


## structuring your application

The question often arises in node.js projects:


#### How to structure the application?

As mentioned before, often the libraries/frameworks you use influence your structure quite a bit.

> Lance Pollard initiated a listing of different structures: [example structures](https://gist.github.com/lancejpollard/1398757).
> You can also find many blog posts about how to structure a node.js application (us included: [our own structure struggles](http://intesso.com/articles/three-essentials-for-open-source-application-design) )


We might also ask the question:

#### Do we need to structure the application at all?

> No we don't need to.
>
> modularize instead of structure
>
> enter GlintApp &#9094;



# Steps to a GlintApp Galaxy


     [1. develop small modules, that contain server as well as browser code][]

     [2. every module must declare it's dependencies][]

     [3. make the modules configurable with a default configuration and an options argument][]

     [4. use widely supported javascript (concerning the browser)[]

     [5. if you use transpiler/compiler, make sure you provide the compiled code also][]

     [6. assets go into the public folder of your module][]

     [7. define the server technology for your GlintApp Galaxy][]

     [8. define commonly used modules for common concepts][]

     [9. require no special environment settings][]


> thats it.
>
> what follows is just an explanation of these steps...


# [1. develop small modules, that contain server as well as browser code]

> - in your module's package.json declare the entry point for the server (as always)
> - and the entry point for the browser (looked at by browserify)
>
> ```
  ...
  "main": "server.js",
  "browser": "browser.js",
  ...

> ```
> if your module requires special transformations, declare them in the browserify field:

  ...
  "browserify": {
    "transform": [
      [ "brfs" ],
      [ "envify", {"_": "purge"} ]
    ]
  },
  ...

> ```
>

but does that work at the application level ?

> Yes it does, browserify handles it.

Here's an example of how you can scructue your modules.
- the browser only get's the `upper arrows`,
- and the server the `lower arrows`

as you can see, there is browser specific files, server specific ones and commonly (shared) files as well, no matter where in the module hierarchy they are used.

```
                                 +-----------------------+
                                 | blog                  |
                                 +-----------------------+          +-----------------------+
                                 |                       |          | is-browser            |
                           +------> browser.js           |          +-----------------------+
                           |     |                       |          |                       |
                           |     |   // dom stuff        |       +---> browser.js           |
                           |     |   // etc.             |       |  |                       |
                           |     |                       |    +--+  |   return true         |
                           |  +---> server.js            |    |  |  |                       |
                           |  |  |                       |    |  |  |                       |
                           |  |  |   // data access      |    |  +---> server.js            |
                           |  |  |   // etc.             |    |     |                       |
                           |  |  |                       |    |     |   return false        |
+-----------------------+  |  |  +-----------------------+    |     |                       |
| app                   |  |  |                               |     |                       |
+-----------------------+  |  |                               |     +-----------------------+
|                       |  |  |   +-----------------------+   |
|  browser.js           |  |  |   | news                  |   |
|                       |  |  |   +-----------------------+   +-------------------------------------+
|   require('blog')() +----+  |   |                       |                                         |
|   require('news')() +------------> browser.js           |                                         |
|                       |     |   |                       |     +-------------------------------+   |
|                       |     |   |   require('ctrl')() +----+  | ctrl                          |   |
|  server.js            |     |   |   // etc.             |  |  +-------------------------------+   |
|                       |     |   |                       |  |  |                               |   |
|   require('blog')() +-------+   |                       |  +---> index.js                     |   |
|   require('news')() +------------> server.js            |  |  |                               |   |
|                       |         |                       |  |  |   // shared                   |   |
|   require('auth')() +-----+     |   require('ctrl')() +----+  |                               |   |
|                       |   |     |   // etc.             |     |   if(require('is-browser')){ +----+
+-----------------------+   |     |                       |     |                               |
                            |     |                       |     |     // browser stuff          |
                            |     +-----------------------+     |                               |
                            |                                   |   } else {                    |
                            |                                   |                               |
                            |   +-----------------------+       |     // server stuff           |
                            |   | auth                  |       |                               |
                            |   +-----------------------+       |   }                           |
                            |   |                       |       |                               |
                            +----> server.js            |       |                               |
                                |                       |       +-------------------------------+
                                |   // data access      |
                                |   // etc.             |
                                |                       |
                                |                       |
                                |                       |
                                +-----------------------+


```




# [2. every module must declare it's dependencies]

> declare them in `dependencies` and `devDependencies`

# [3. make the modules configurable with a default configuration and an options argument]

> write the module configuration in it's own file e.g. with a configuration file like `config.js`

> when requiring the module, allow overriding of the default configurations:

TODO e.g.

# [4. use widely supported javascript (concerning the browser)

> be supporting, being a bit conservative might not be wrong here (depending on your Universe)

# [5. if you use transpiler/compiler, make sure you provide the compiled code also]

> do not throw internal complexities (which should not exist anyway :-) on to the consumer

# [6. assets go into the public folder of your module]

> this lets the GlintApp Universe bundle the assets. e.g. with [assets-bundler](https://github.com/intesso/assets-bundler)

# [7. define the server technology for your GlintApp Universe]

> as much as we like to support everything, architecture also means to make decisions, also to limit our selves to solve the actual problems.
> e.g. [Express](http://expressjs.com/) or [hapi](http://hapijs.com/) etc.


# [8. define commonly used modules for common concepts][]

> when you design a system, it consists of *structure*s and *concept*s. See [arc42](http://www.arc42.org).
> typically you want to follow consistent concepts in the different parts of the system (modules). e.g. consistent Logging mechanism.
> Therefore, make known the conceptual modules, the individual modules shall require.


# [9. require no special environment settings]

> be a good neighbour.


# local modules

> When you create many small modules for your application, maybe you don't want to publish them all to npm or have seperate git repositories or share them at all.
>
> See [LOCAL_MODULES.md] for suggestions on how to do this.


# author
Andi Neck | [@andineck](https://twitter.com/andineck) | andi.neck@intesso.com | intesso

# get involved
Any Feedback is highly appreciated.
Please create an [Issue](https://github.com/glintapp/glintapp/issues/new) or [PR](https://github.com/glintapp/glintapp/pulls).

# known implementations
 - [glintcms](http://glintcms.com)
 - ... please send a PR which adds your implementation ...

# recommended reading
 - [unix philosophy and node]()http://blog.izs.me/post/48281998870/unix-philosophy-and-node-js)
 - [linux and the unix philosophy] ISBN-10: 1555582737
 - [browserify handbook](https://github.com/substack/browserify-handbook)
 - [arc42](http://www.arc42.org)