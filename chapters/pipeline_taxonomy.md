# Pipeline Taxonomy

I'm characterizing the three kinds of pipelines you can come up with as Ad hoc, Structured, and Modular.

## Type 1: Ad hoc

The ad hoc structure is what you typically get in a start up situation, or when you are innovating your technology a lot. You build it as you need it. You might start out with the really simple pipeline in the previous example, you might add in your new normal mapper, you add this and you add that. Little by little, the pipeline grows up and around the features as you invent them.

A central idea here is that the entire product is built pretty much all in one go. There is likely to be a single individual who's job it is to push the big red button that causes all the gears to start grinding, and make the product pop out at the other end. By product, I mean typically a single game level (or possibly a whole entire DVD, but I hope not!)

An ad hoc pipeline has a lot of dependencies and couplings between steps, so you can't really skip along — you're committed. If you've worked with this kind of pipeline, I'm sure you've seen pipeline steps that can take five or six hours to really grind through everything and get to the end.

Another characteristic is that the data formats are arbitrary, because they have been made up as the project goes along.

### Ad hoc Pros and Con

Ad hoc pipelines are very flexible, due to the lack of structure. It is possible to easily innovate, and have rapid response to new needs. By the end of the production though, the ad hoc process turns into a liability. In the worst case, there enough squirrelly, or weird parts to the process that it might turn out that there is only one person in the building who can successfully get a build. As time goes on, and the game takes two or three years to produce, you might find that there's all kinds of crazy hacks and workarounds, there's exceptions - like maybe you can run the normal map extraction on every character, but don't ever run it on a one particular character, because the entire level will break. As the production gets near the end, failing pipeline elements become common and the TD's end up running all around the building like crazy just trying to keep the whole thing spinning.

## Type 2: Structured

The typical next stage for a studio after having been ad hoc for a few years, and experiencing the joy of it all, is to get structured. That's a good next step. A structured pipeline is like a flowchart, you've probably seen these types of diagrams. You might have the director up at the top, he dispatches his minions to go out and do some layout, some character modeling, some vizdev, and whatnot. It all branches out, comes back together, there's little approval loops. Finally at the bottom, you get your end product.

A key characteristic of this kind of pipeline is that there are sign-offs and approvals at certain stages, and that's good. There's checks and choke-points which are places where you can stop and assess the state of something. A choke-point is a place where several things have to come together at once, like a character model and it's character rig.

This kind of pipeline lends itself to automation. It can be scripted, you can ensure that everything is happening properly which is really difficult in the ad hoc structure. A key difference with ad hoc is that the structured pipeline is oriented towards the unit of work. If a product in the pipeline is a texture, then there is some sub-part of your graph that is all about textures.

It's important that the data formats be well documented because your data is going all over the building. You might also have to be rigid about it because you might still have some of those old rigid and brittle ad hoc tools sitting around that can't tolerate having the data migrate to a new format.

Structured pipelines become common after you've built a few games.

### Structured Pros and Cons

The structured pipeline has some pretty strong pros. It can be managed by non-technical personnel — they can use the databases and spreadsheets that they're familiar with. They can make estimates of where things are at, and when things are going to come out the other end.

The pipeline can be instrumented, and efficiencies identified. You might find out that it takes a really long time for backgrounds to get finaled, and you can address that, either by improving the process, our adjusting other processes to accommodate.

The flaw with the structured pipeline is the bottleneck. If too many things have to go through one person or one process, or one approval, you can back up everything. You can starve the rest of your pipe and have people sitting around who could be doing something productive, but they're blocked because they don't have lighting or whatever.

The fatal flaw with the structured pipeline that I've seen many times, is that the pipeline breaks down if you have to rework something and it sends something back up into the top part of the flowchart somewhere, and it has to go through a bottleneck again. That's something we've learned from film. If you've finalized something, for example a shader for Yoda's skin, the one thing you never do is come up with a really really cool advancement to that shader after lots of shots have been finaled. You'd have to re-film-out all the previously finaled shots, and worse, you might invalidate some other work like the lighting set up. As an industry, we do this in games all the time, but we really need to consider the cost of reworking final elements.

### Ad Hoc vs. Structured

Key differences between these two architectures are that the ad hoc pipeline is focused on getting the data through whereas the structured pipeline is focused on efficient process. Ad hoc is common during prototyping or in startup shops, whereas structured pipelines come with the introduction of formal management processes like waterfall. You can support a lot of creativity in an ad hoc environment, but you're more likely to get done with a structured process.

## Type 3: Modular

What I'd like to do is to introduce the notion of a Modular pipeline which is a hybrid of ad hoc and structured. A modular pipeline is strictly feed forward. There are no loops in the process so nothing can ever back up or starve people down the pipe. An analogy is an automobile assembly line — you've got the conveyor belt going along, the chassis is there, the wheels come in, the seat goes on, the engine goes in, eventually a car comes out the other end. The key to it all is that you've got little spurs on the side which is where your structured process occurs. Iteration on an element might occur in a loop on a spur — a background for example might get iterated and approved in a structured way in its own little flow chart, and it is released on to the main conveyor belt when it's done.

It takes a bit of work to set things up so that you can actually work that way and you really have to think about your dependencies.

### Modular Pros and Cons

You iterate on your spurs, there are no back ups, and you release to the main line as you go. Portions of the pipeline can be swapped out, or they can be reordered, or tools can even be re-purposed with little difficulty.

A flaw with this system is that without uniform well designed data structures, it can get expensive. If there are funny little formats everywhere, and you swap two processes, you might get stuck. There might be translators that go from A to B, but there might not be a translator from B to A. That's where it gets expensive if these things need to be created and maintained.

Uniform data structures can compromise algorithms. Perhaps some step needs a wonderful point cloud, but if everything uses the same data structures, but there's no accommodation for point clouds in there, it might become necessary to implement around not having the data that's needed.

Nonetheless, unblocking everyone else, and having little pieces of the pipeline isolated to be ad hoc or structured as needed is a big win.
