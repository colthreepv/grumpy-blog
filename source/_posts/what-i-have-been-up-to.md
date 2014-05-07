title: 'what i have been up to'
date: 2014-05-07 12:29:12
tags:
---
After months of javascript mastering in both frontend side (with angular) and backend side (with node) i had to take a summary of this experience.

In my opinion node.js is a great success: is quite uncommon that a new platform receives such a warm welcome from the community, and there are several reasons behind.

Node has a very good performance, has a low entry point for whoever knows how to structure javascript code, and the node API are really barebone, making it possible to learn in a week.

The custom types in node are quite easy to use and very powerful (Buffers and Streams), i think anything the joyent developers (and contributors) developed is pure Gold.

## The downside
After some time that you code gruntjs scripts, dive into async libraries, structure your imports, code review other people code, write tests... you might realize that in the end javascript in the backend is *viable*, but just not sufficient for the vast amount of things you can do on a server. 

Don't get me wrong, you can do basically anything with node modules (which many have also a performant C binding),  the problem here is the language doesn't help you! 
Ecmascript5 is a refinement of a simple language designed for event-driven browsers,  after all!

Node usually eats as much ram as python/ruby, without having the same amount of libraries, but the main problem is you need good developers (to structure the application), and even with them you need to strictly follow a common guideline or your code will look like a gigantic Picasso of code.

So I came to the conclusion to experiment with some other server-side language... 

### What to try?
After days of googling, i chosen without doubts Scala as the next thing to learn.  
There is quite some hype on the web, and the user community seems to be enthusiast about it!
After all many cloud services (Apache FSF projects for instance) are based on JVM, so learning a JVM-interoperable language would be interesting for my resumee and to develop really scalable solutions.

I was even more ambitious, with Scala I could even code Android applications, so one language to rule them all... that was the plan.

I even choose what to read in order to have a good introduction to the language: [Programming in Scala 2nd Edition](http://www.amazon.com/Programming-Scala-Comprehensive-Step-Step/dp/0981531644)

### Scala is the future, then?

Well... err... maybe!  
From what i learned 75% of Scala userbase are disappointed Java developers, tired of boilerplate, slow release cycle and general displease about language choices. 
The book in my opinion is really polished and well written although is FULL of future references (`you will learn this in Chapter 14.1`, then on Chap 14 `You can find advanced usage on Chapter 36`), and is generally quite long... as the language is complex.
This might scare a LOT of people out there, exactly the same amount that loves javascript for its simplicity. I ended up learning a diametrically opposed language...  
900 pages of language manual might seem much but the book is co-written by a language author and really makes you understand many concepts left out by other publications; i suggest to read it just to be introduced to functional programming, that's a really intriguing and stimulating part of Scala!

After reading 200 pages, i decided it was time to go on the practice... i was blown out by the complexity of editors! Yeah IDE!
I'm a sublime text enthusiast, i love simple editors, but with Java/Scala where [a Map can have 50 methods](http://www.scala-lang.org/api/2.10.4/index.html#scala.collection.immutable.Map), an IDE is actually useful, too bad that it requires a LOT of configuration and trial/error before getting it right... 

In the end i went using IntelliJ IDEA, i would suggest this to everyone, you can find a good setup guide for [intellij + scala on stackoverflow](http://stackoverflow.com/search?q=scala+intellij+setup)

Then i finally coded a simple hello world that picks the words "hello" and "word", from Wikipedia,  uses [jsoup](http://jsoup.org/) as DOM parser and prints the concatenated output in console.

I downloaded the source on my cheap VPS hosting with 256MB of ram, and... that hello world didn't even compile!
YEAH, actually `sbt` runs out of memory just pulling some libraries and putting them togheter... 
That made me realize that *maybe* this technology isn't the right one for prototyping... what i really do in my spare time is _prototype_ something. 

After this short moment of self-enlightment i kept on reading about Scala for awhile, and also found some downsides, for example [memory/cpu consumption](http://stackoverflow.com/a/5901529/1071793), that must be the price you pay for code elegance!  

I'm really curious if some hobbyists have used that language/technology successfully, and to achieve what.  
To the next grumpy-post!
