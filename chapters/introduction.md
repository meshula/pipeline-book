# Introduction

The motivation for looking at the content pipeline came up a few years ago. I used to build rendering engines for LucasArts, and we were getting to a point where our renderers and engines were getting pretty powerful, and you could do a heck of a lot of stuff, but we started to run into the problem where it wasn't possible to get the things you wanted into the games in a timely way. We had engines that could throw around millions of polygons, but the tools weren't able to do the same thing. It was difficult and time consuming to get things down the pipeline, the tools bogged down. We had one example where we got a Star Wars Episode II Geonossian insect creature from ILM to use in Republic Commando. We could render it in real time, but in our tools it would redraw, even in wireframe, at several seconds per frame. You can't iterate quickly when tools behave like that.

We are really pushing, our aim is to achieve filmic quality in games in our next generation of titles. We had to find some kind of a solution, so we asked ILM how they are dealing with this really heavy data. We got the model from them, it was bogging our tools, but somehow they were getting thousands of them into a scene. Ultimately, we had to pick apart the entire pipeline from top to bottom to understand how to get into the content driven and heavily data driven world. Now, several years later, results and techniques we've shared back and forth with ILM are in practice on our new game, Star Wars: The Force Unleashed.


## Goals of a Modern Pipeline


- We want to have close interoperability between tools and preview.

- We want the preview to be as close as possible to final on our target platforms, whether that's a game engine or a film render. The faster you can get the result of a tweak, the better off you are.

- We're interested in sharing assets, from top to bottom, across the board.

- We've moved away from having a lot of funny little proprietary formats that suit the needs of the moment, or the pipeline being used for the particular project, to a pipeline where the tools, and the interfaces to the tools are consistent, our data representations are consistent, and we share fundamental representations of geometry, materials, and shader libraries, across the companies.

## Blinn's Law

One of Blinn's Laws mentioned frequently recently is the saw that “all frames take 45 minutes.” A translation of that is “no matter how much hardware or software optimization I give you, you're going to add more stuff in until it takes 45 minutes again.” We see the same things in games as we get closer and closer to ship. The frame-rate stays right on the edge, and the artists throw more and more things in, so you're constantly going back to optimize to get things back to 30 (or whatever the target frame-rate happens to be).

The 45 minute frame time is going away. As we move into an era of interactive feedback and interactive tools, we are going to get used to a 30 Hz frame-rate in tools; the artists like it, and they won't let it go. Caching and coherency are the keys for letting that happen.

## Caching and Coherence

I like to make the analogy that a good content pipeline is like what goes on in a good game loader. In order to get the game loaded up quickly, you're going to have to analyze the content of your scene. The order of the load has to be sequenced correctly, and for particular subsystems; some subsystems might require their RAM to be loaded before others and so on. Perhaps some portions of the data need to be converted on the fly. You try to order these things so that as many things as possible are going in parallel so that the user doesn't have to wait too long before things happen.

Thinking about that in terms of a content pipeline, where artists are trying to iterate, consider the individual steps that comprise the pipeline. An example is mesh partitioning — it might be necessary to touch the data several times in several tools in the process if you're not careful. You might be using XML as the model format, and you might find yourself needing to convert the mesh from XML to an internal format again and again. This is where caching comes into play. You want to store off the intermediate results so you're not recomputing them constantly.

## Cached data leads to a pipeline

The idea that all this leads to is that you've got this process where art starts at one end, goes through a series of steps, and the cached data are the results that you're saving out to disk as the process goes on. This leads to the existence and need for a pipeline.

A really bad example, yet a really common one is the situation where you have a lot of little tools that you need to get your mesh from Maya into your game. The first approximation of this pipeline might be that you write a little exporter script that writes mesh data out to some kind of easy to parse intermediate file, then you have some tool that maybe the graphics programmer gave you that reads that object file in, converts it to an optimized format, then writes it out to something else. Then, another tool picks that up and packs it into a big large file for the game to load. Typically when you're starting out, you often find people doing these steps manually, one after the other, and hoping to remember the individual steps, like “did I remember to run the normal map extractor?” If you do the same series of steps a hundred times, you have a pipeline.