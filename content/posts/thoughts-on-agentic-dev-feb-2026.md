+++
author = "Jonathan Markman"
title = "Thoughts on agentic development, February 2026"
date = "2026-02-14"
description = ""
tags = [
    "AI",
    "AgenticDevelopment"
]
+++

Earlier in the month I ended up purchasing a subscription to Github Copilot Pro, it's (currently) $10/month. I wanted to try and replicate what a coworker was doing with agentically developing a board game; introduce "vibe coding" into some of my more creatively-focused workflows.

I have a gaming blog where I chronicle stuff and I wanted a nice solution for pop-up modals that worked with Jekyll. Trying to generate an entire modal with the free tier was doable, but required a lot of guiding and inevitably didn't work exactly as I wanted it to: the anchor tag that would launch the modal would push all text following it to a new line no matter what, and the agent wasn't good at debugging this.

I tried again today with the Paid Option™️ and with a little coaxing I got something I wanted. The first go was a bit disastrous where the modal would just be a opaque black box superimposed over the blog sidebar and some of the content, but when I told the agent to revise its approach, it finally got something that was both functional and looked good. The agent had to parse through the various subdirectories, which it didn't do the first time, and it looks like that made all the difference.

I tried to have the agent extricate the inline CSS from the include, but this ended up breaking its work. I had it investigate why this was the case, and I was kind of surprised it was able to grep the answer. This *is* a Jekyll blog so this is a very repeatable "thing". When the agent moved the CSS it generated into a Sass file, there were two issues:

- Two media queries to forcibly keep the modal + popup on top of the blog content vs the sidebar were conflicting with CSS/Sass load order
- The extracted-to-Sass-file styles weren't being imported to the Sass files that were being transpiled into CSS; they were imported into index.css/sass, but required an import into another Sass file used exclusively for the blog posts

The agent fixed the latter point and I instructed the agent to remove the media queries in the former point.

Having it extract the JavaScript was much easier, but one thing I've noticed is that the agent will always take the easiest path to its detriment: I told it to "Extract the JavaScript from the video modal include" and it did a copypaste job where it made a new .js file, copypasted the JavaScript from the include into that file, updated the script tag to source it, and then "make sure the JS file [was] in the right location".

It didn't account for the fact that it was going to break the JavaScript because Jekyll passes information to the include via variables and once the JS leaves the include, all those variables are going to be missing. The agent realized this after I refreshed, saw the modal was broken, and prompted the agent to review the .js file again.

Last thing I did for this session was have it spit out some documentation on how to use the modal include; I want to use this a lot and I may want to use it more than once in a post. It did a pretty good job documenting usage and how I arbitrarily decided upon mp4 and webm file types as its "supported" files because I try to use webms as often as I can with online pages.

The "complete example" it gave was actually *really* bad because it forgot how to use its own modal: it created two links for the modal pop up but forgot that it needed to use multiple includes in order to differentiate the modals. This could probably work if I had the agent refactor the include and accompanying JS, but I just deleted this part of the documentation. I also deleted the entire "Tips" section where it said where to put videos, account for file size, etc., because I know about that already. The agent is just covering all of its bases and assuming this is documentation "for everyone".

It wasn't a perfect landing on Github Pages: the modal broke just like it did locally when I had the aforementioned CSS/Sass issues. I had the agent look up why Jekyll includes would run into CSS/Sass issues like this, and I got back a lot of results that were reasonable to parse but also kind of sparse. It was like reading google results and going "well I guess I better go look *that* up now". The number 1 reason, "Safe Mode Restrictions", is the likely culprit where there are probably restrictions in terms of what custom CSS/Sass I can have the agent add. I feel like I'm going to have to find a GH Pages + Sass SME just to find out why this works because my own google searches have not been fruitful.

I had the agent go back to using inline CSS and it worked on GH Pages.

Some closing thoughts:

- I also added an AGENTS.md file + a starting skill for creating future includes to my gaming blog and I'll update these as I tweak the blog more to my liking.
- I'm simultaneously impressed and unimpressed with agentic development. I like that it actually made something that worked, but it left me with more review work to do.
- My CSS isn't great but the entire point of having a Jekyll blog is that it's supposed to remove a lot of the heavy lifting of building & maintaining the HTML + JS + CSS and enable me to just focus on the important part: writing. I think if I do a web project in the near future I'd try to do the CSS by hand and study it myself. For something like this, I'm ok with the agent knowing more about it than I am.
- I think a "project" like this is a great use case for agentic development. Based on how I interact with the agent on something like this, I don't think the actual education and learning how to do [x] in HTML, CSS, JS, etc. can or should go away. **It's impossible to get the most out of an agent if you don't know enough about what you're looking at.**