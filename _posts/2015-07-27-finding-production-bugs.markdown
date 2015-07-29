---
layout: post
title:  "Finding production bugs"
summary: "How to configure your system to allow you to effectively find and fix bugs in a production environment."
date:   2015-07-27 15:08:12
categories: jekyll update
---
I have worked on a number of different projects and one thing that I feel seems to be common across them all is the
fact that finding bugs is always difficult. By bugs here I don't mean the type where a designer has their magic ruler
out and is wanting a button to be 1 pixel to the left or a shade darker than Kelly Osbourne's hair. Im talking about
the real meaty bugs for example, every time a user updates their account balance they lose a pound etc...

I think there is a very logical way that we can approach the finding, fixing and prevention of such bugs so long as the
system that they exist in is set up to allow us to.

The main thing we need to ensure is *- obviously I hope -* that logging is in place. The problem is however that most of
the time our logs are polluted with crap. Logging should be something that is there to help not something there because
Dave the manager thinks it should be so we just log stuff here and there. One of the main questions we need to find an
answer to when trying to find a bug is where did it originate from? This could be broken down into:
Was it the customer service? Or was it the banking service? Not only which service did it start at more specifically
which endpoint?

We should be logging at a transaction level to help find bugs. We need to decide what
information for a given transaction is useful of course but fundamentally it should be who, what, when, where eg.

    01/01/2015 17:05 POST /customer/xyz received {id: 'xyz', name: 'John', dob: 01-01-1980}

then we can clearly see that on the 1st of jan at 5pm, a post occurred with some data, about some customer. If this is
the customer who is reporting a bug we have a ball park to look in by searching his name/id etc. in our logs and finding
this log message.

The next step is finding the actual problem in the code. So we now know that if we go to wherever /customer is defined,
lets say our customer service, we will be able to manually trace through the code and find out what happens. What if
we have time as a pressure? This could be code you have never seen before, it could be c# for gods sake. So how can we 
find out where that code could be going
wrong? The logs again of course :p Does he want me to log everything I hear you say? Nope as that will give us the noise
problem. We should be logging anything useful! For example does the customer get put in a database, if so log that

    01/01/2015 17:05 customer {...} with id [xyz] saved successfully

more importantly if the DB explodes at that time log the error too!
    
    01/01/2015 17:05 Blahhhh happened whilst saving customer {...} with id [xyz]
    
Another example here is does the customer get sent to some other place eg. SalesForce *- your usually pretty f@c#ed if so -*
then log this too

    01/01/2015 17:06 customer {...} with id [xyz] sent to SalesForce

Depending on your system etc. and the bug your trying to find, you could hopefully find the error in the log at that
point without looking at the code.

A nice idea I have used a few times is to add transaction id's to your logs. This would allow you to track a single action
very easily for example,

    [123-abc] 01/01/2015 17:05 POST /customer {...}
    [123-abc] 01/01/2015 17:06 GET /account/123

where [123-abc] is the transaction id. We can see from this that the same transaction fired off 2 requests, this is
useful as we can now grep the logs for this transaction id and find everything that happened along the way.

If that is not enough to locate the error, try running your tests *- you may want to start with this -*, assuming you have some. 
If a test fails great fix it and check if the error still occurs. If you don't have tests then shame on you, go create
some. If none of your tests fail then you need to ask yourself why that is? Do you need a test for this?

Hopefully by this point you have been able to locate the blocks of code responsible for the bug even if you don't know
the exact line, if not then code reading it is for you. So how do we fix it? I think the best option is to start with a
test, try to replicate the scenario that you are experiencing problems with. Whether this is a unit/integration/functional
etc. test is dependant on the bug. Once you have a test in place you can make it go green and at the same time add some
logging into the code to ensure next time you can find errors like this.

In order to easily find log messages for your application it is recommended that you aggregate your log files. There are
lots of tools out there to help you with this such as [logentries](http://logentries.com). Another tip is to use log levels, debug, info etc. to allow
you to filter out any noise in your logs.

In summary, log useful things and test your code :D