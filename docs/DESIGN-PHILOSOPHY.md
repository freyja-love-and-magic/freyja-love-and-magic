# Planet Nine's Design Philosophy

## **Planet Nine is intentionally built to iritate professional developers. Below is why.**

## Overview

Modern software development is largely this:

![draw the rest of the owl image](docs/drawing-an-owl-scaled.jpeg)
this is supposed to be the "draw the rest of the owl meme" if it's not something's broken.

Unfortunately, this has made programming a ridiculous discipline that requires a four year degree or some nonsense to shit out a website.

I learned how to make a mini pool table in woodshop class back in seventh grade. 
I didn't think much about it at the time, but to perform this task I didn't need to know the intricacies of the sustainable lumber industry in the Midwest, nor the milling process for the wood involved.
Likeise I didn't need to know how to machine screws, nor how to turn 120 volts of AC current into the RPMs necesary for my circular saw to make the imprecise cuts I was making. 

I don't even know all this now, but I could probably cobble together a mediocre mini pool table if needed. 

The discipline of programming is headed towards this type of commodotization, but since everyone is convinced that they're going to be the next Facebook, the mini pool tables they're building are being built out of Damascus Steel, exotic marble, and felt made from the matted fur of rare beasts incubated in some sub-basement of google's ten percent time. 

So the design philosophy of Planet Nine is to give you wood, screws, and maybe some felt, and see what you can build.
The rest of the industry will make sure the wood and screws actually work.

## First the hard parts

> There are only two hard things in Computer Science: cache invalidation and naming things.
> -- Phil Karlton 

This is great, but also belies the tendencies of computer scientists to make things more complicated than they need to be.
Why do caches need to be invalidated, and why do things need a name?
But that's a digression.

Instead let's take these as hard, and add two more: authentication, and scaling.
Since these are hard, and we are all new to programming together, we will solve them the way humans have been dealing with tough problems for time immemorial.
We shall pretend they don't exist. 

### Cache invalidation

State does not exist within Planet Nine, and thus caches don't either.
Things may be persisted at locations reachable via some reference, but those things will not persist in-memory (i.e. in a cache). 

### Naming things

Planet Nine has a lot of named "things," the protocols of The Stack, the mini-services of allyabase, the apps of the-nullary, and The Advancement. 
What it does not do is define what the "things" that go into or are exchanged between these things are, nor does it name them.
What this means in practice is that Planet Nine neither contains nor defines "types."

Consider the following:

```json
{
  "privateKey": "95d2a94214c0cfbff7a09b7bd38ec5492e378901d0193da659c372d21c310e7a",
  "pubKey": "0253d014f89b4a6da3a445f8877cd897af281373f181be23c511ea412f46e78401"
}
```

The above is the JavaScript Object Notation (JSON) representation of a private/public keypair of the kind used in the Sessionless protocol.

Different languages may want, or need, to represent this tuple in different ways.
Could be different collection types: dictionary, map, hashmap, array, set, etc.
Could be different data types: something other than hexadecimal strings.

The protocol specifies that pubKeys be shared as hexadecimal strings, but it does not specify an internal representation.
Thus a specific language _implementation_ may use whatever internal representation of these two keys that it wants.

Now you, as someone developing something for or with Planet Nine, can define a thing if you would like, it's just that Planet Nine does not.
This is a large and intentional deviation from many distributed systems like [ActivityPub][activitypub] that have well-defined types with prescribed structure.

### Authentication

Auth (authentication or authn, and authorization or authz) is at the heart of Planet Nine.
The web-based systems that people are accustomed to pretty universally exchange some kind of user credentials (email and password, or sign in with Apple, or something) for a token representing the user within the system, which we'll call a userId.
The userId is then used to attach various stateful things to over time as the user uses the system.
There are various reasons for this, and not all of them are wrong, but Planet Nine asks the question of what we can do if we can skip this step?

Along with the userId, systems grant to the user a token that reflects their logged in state called a session (those familiar with auth may also know JWTs, which for this document we consider the same). 
The session combines with the userId to provide authorization in the system, i.e. whether a resource can be accessed or manipulated by the user. 
The issue with sessions that they're a shared secret that is sent over networks, and thus are vulnerable.

Asymmetric cryptography (in-use since the 70s) allows for signed message authorization.
This keeps the secret part on a user's device in the form of a private key while offering the same authz capability. 
While sessions can't be shared, signatures can be.

That's the crux of Planet Nine.
What can you do with shared authorization?

### Scaling

In general, production-level software run by companies only have this one problem, as the other three are all solved by design decisions easily made by professional developers.
This also isn't a problem for many systems these days as hardware is quite capable of handling millions of requests without complex setups. 
This is often turned into a problem by tons of unnecessary network calls and database queries made from poor design decisions easily made by professional developers.

But again, we're going to deal with this problem by not doing it. 
We can call it the Starbucks model of scaling a distributed system.

allyabase instances are called "bases." 
If a base gets too many users, say ten thousand or so, it'll reach a capacity, and then we'll have to "open a Starbucks across the street," i.e. create a new base for new users.
Since we don't rely on centralized authentication, bases can act independently, and we can "scale" by just loosely tying together bases under the Planet Nine name. 

You may then ask how, if things aren't named, instances don't share identity or data, and no central authority is keeping everything together, does anything "work?" 

It's a valid question.

## How does Planet Nine work?

I'd like to tell you a story about phone numbers. 
Let's say you wanted to build a system that allows all eight billion people on the planet to input every phone number they've ever had in any way that they would like, and then you display those phone numbers consistently in some format across an arbitrary number of client applications.

In an abstract sense, we can represent this flow as three boxes:

<put ETL diagram here>

Some set of software "extracts" this data from the good people of the Earth.
Your system "transforms" it into some format the client applications can consume.
Your system then "loads" this transformed datatype into what the client consumes and displays. 

But we've already established that all eight billion people can input whatever they want.
So lets say our transform layer turns everything into a thirteen character numerical string consisting of country code + area code + phone number.
Now what happens when someone inputs, "fuck you dickheads, you're not getting my number!"
I'd imagine your system might struggle to convey that in thirteen numbers.

The question then becomes where does your system break?
Does this bad input get rejected so that this user gets excluded for not wanting to share their phone number?
Does your server break because it performs some operation on this string that doesn't work?
Do all your clients break arbitrarily because they're trying to display something they don't expect?

Now here's the thing I want everyone who designs systems to remember: we are not the arbiters of what human beings consider a phone number to be.

The only safe way to build this system is to provide as few limitations to what a phone number is, and then to have clients that rely on it continue to operate it if it doesn't fulfill the requirements of whatever it is that consumes it.

In other words Planet Nine doesn't do ETL. 
Instead it does the following:

* No transformations
* Things either work or they do not
* When things don't work they fail immediately, and predictably

Planet Nine can do this because its approach allows for type segmentation within the system.
Phone numbers, and for that matter any other type of thing, doesn't need to represent the input for everyone on Earth, just for the segment within whatever sub-unit of Planet Nine you care about that your applications are running in.

## The Code

Us professionals have devised a few decades worth of "best" practices, and ways of doing things designed, as these things often are, to keep ourselves employed at the expense of people who just want to get started. 
People talk about Clean Code and Architecture, SOLID, Domain-Driven Design, etc.
They're all worthy concepts, all of which are purposefully thrown out the window in Planet Nine.

Many of these concepts exist to make software more manageable when worked on in an environment where people are coming and going, and some consistency to a large codebase needs to be maintained. 
That is not what Planet Nine is.

Planet Nine is meant to be the starter set. 
It's meant to be the mini pool table that you use to start to build bigger and better things.
That it's software means you can use it as a core component, but it is not meant to be expanded upon the way that most software systems are. 
Thus the need for design principles in an ever-expanding codebase is obviated since, if we've built things the right way, we should never have to touch this code ever again.

Since it's trying to be the starter set, here are some of the principles I have purposefully ignored:

* Don't Repeat Yourself (DRY): This is probably the hardest to keep my brain from doing, but the motivation is this. I can't tell you how many open source repos I've jumped into just to get lost in some kind of subdirectory rat's nest looking for where some common logic handler was actually executed. In an IDE this is trivial of course, but in github it's not, and it makes things for someone who's not ready to pull the code a problem. If you think of someone new to code, they're not going to have the experience to even go searching, so I wanted as much of the logic to be present where I figured someone would go to look, and that meant repeating things a lot.

* Separation of Concerns: Having a system which does not clearly define what handles what, and what things handlers expect in the first place, necessarily lends itself to confusion at the boundaries. In other systems, great care is given to make sure that certain parts of the system can't touch other parts, but rather than introduce code to impose such restrictions, I've opted to simply go for the most expedient route towards things working. I suppose you could call it the pragmatic instead of the dogmatic approach. 

* JavaScript: There was this one time, maybe fifteen years ago or so, where I was talking to a guy who was working with a non-profit looking to teach coding to the youts. I offered to volunteer, and he asked me what language I would teach. I said JavaScript, and he scoffed at the notion, as though being taught the most common programming language on the planet for free was a disservice because it didn't have a strong type system. 

Since Planet Nine can expand to different languages on different platforms, it needs some common way to send messages between applications. 
This should be human-readable because Unix says so, and because all the reasons to not be human readable are only for gigantic systems that need to scale in that way. 
The most ubiquitous option for this, and one that's easy to work with, is JavaScript Object Notation, or JSON.

If we're using JSON it means you're learning some JavaScript. 
You know what's harder than learning one programming language when you don't know any?
Learning two.

Something like 90% of programmers know some JavaScript. 
So it's as close to a lingua franca that we've got, it's a good first language, it is the language that runs in all web browsers, and you've got to learn it to use Planet Nine regardless of what everything else is built in. 

But don't worry. To prove to the 1337 coders out there that I'm cool, I ported some of the stuff to Rust too.

* Frameworks: In general, very popular frameworks are created and maintained by gigantocorps like React by Meta, and Angular by Google. These gigantocorps are the problem, and thus Planet Nine will not use the software that they make (I'm sure they've snuck some in in the nest of dependencies that npm throws into everything, but we'll be systematically addressing that over time). For UI we leverage the web stack, and use vanilla html, css, javascript, and wherever possible SVGs. This latter might be surprising. I encourage you to review the [SaVaGe][savage] repository for how that works.    

* Databases: Databases are great for when you find one thing amongst eleventy billion things. To do this they add quite a bit of complexity to systems. The goal is to never get to the scale where the complexity is worth it, so we simply use the file system. If you think that's not gonna work, it's easy to swap out the datastore because you can save everything everywhere as JSON.

* SVGs: One of the hardest things about modern web development is that in order to handle every. single. dang. use. case. under. the. sun, html, javascript, and css--already a confusing triad--have so many options that are unnecessary for like 99.999% of UIs. The docs for the triad are a disjoint tangle of probably three to five thousand pages. SVG on the other hand covers like 99.99% of UIs, and has a spec that's a tenth of the size at only a few hundred pages. There are a TON of resources for making SVGs, and AI is surprisingly great at it since it's a simple spec. 

* Sessionless and Express: There are two dependencies where we did not forego the industry standards. Sessionless is a very light wrapper around the same elliptical curve cryptography used in Bitcoin and Ethereum. Though this author [thinks blockchain is not very promising][web3], the asymmetric cryptography it uses has been in use in systems for over half a century. Sessionless uses the secp256k1 curve like Bitcoin and Ethereum so that if anyone ever breaks it, they'll steal everyone's money before coming after whatever useless stuff is in your bases.

Express is used on the server side because it's actually more widely used than the bare http package in node. Whether it is used forever or not is tbd. 

 



[activitypub]: https://en.wikipedia.org/wiki/ActivityPub#Example_data
[savage]: https://github.com/opensource-force/SaVaGe
