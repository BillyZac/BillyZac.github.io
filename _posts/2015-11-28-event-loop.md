---
layout: post
title:  "The Event Loop"
date:   2015-11-28 00:00:00 -0700
categories:
published: false 
---
a single threaded
v8
call stack
heap
different from other langs

the runtime = v8 in chrome
heap = mem allocation
call stack = frames

web apis -- extra things the browser provides
- dom
- ajax

single threaded means
one thread == one call stack == one thing at a time

stack pushes and pops
return pops
a function call pushes

see stack trace in dev tools

main() is the overall function that holds your program. shows up as an anonymous function in the dev tools, at the top level of the call trace.

## blocking
basically means code that is slow
doesn't refer to just one specific thing
(ruby is threaded)
it's a problem because we're running in the browser where we want a smooth user experience.
browser is blocked until the stack is cleared
not ideal, not good ui
that's why we have async callbacks
browser and node both do this
almost no functions in the browser api are synchronous

how does setTimeout() work?
## the event loop and concurrency

the js runtime can only do one thing at a time, but the browser api can run multiple threads. node too.

setTimeout() is an api provided by the browser.
the web api handles the timer.
when done, pushes onto the task queue
event loop looks at the stack. if it's empty, it looks at the task queue and runs the first thing it sees. pushes it back onto the stack. which is javascript land.
(~14:00)

## The setTimeout() Zero Hack
Makes the event loop wait until the stack is clear until running your code.
A way of deferring.

setTimeout() does not give you a guarenteed time it will run, it gives a minimum time until it runs.

## callbacks
two ways this term is used: 
- a function called by another function
- asynchronous functions

60 fps is the ideal repaint rate. This is what the browser tries to do.
But when it is contrained by the call stack it can't achieve that rate and the screen stutters.
The render is given a higher priority than your callbacks.
Our synchronous code blocks the render calls and the user can't clickon anything. The browser freezes.
With async, we give the render a chance to jump in and do the render between each call.
When people say "don't block the image loop", they mean don't put shitty code on the stack because it blocks the UI
Ex: scrollhandlers:
queues up at every frame
not blocking the stack, but flooding the queue
need to debounce it (What does this mean?)

## Philip's workflow:
(Mentioned at end of talk)
screma(?) -- js parser
slow motion the code
webworker -- visualize what's happening at runtime.

===
[Philip Roberts: What the heck is the event loop anyway? | JSConf EU 2014](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

@philip_roberts


