+++
author = "Jonathan Markman"
title = "Thoughts on agentic development, January 2026"
date = "2026-01-02"
description = ""
tags = [
    "AI",
    "AgenticDevelopment"
]
+++

AI's taking the development world by storm, and one of my thoughts as 2025 ended was that I should start recording my experiences with AI: agentic development, tooling, experiences of other devs, and so on. This will be the first in some number of posts that I manage to carve out time for. All of these types of post will be stream-of-consciousness in the moment.

Used Cursor today to generate unit tests for the main wrapper class for [CurrenSee](https://github.com/jmarkman/CurrenSee); all I really wanted it to do was generate some constructor tests and nothing more, and it did just that. I used Cursor's `auto` mode to select a model so I'll never know what model it selected, but based on how verbose and thorough it was and its usage of checkmark emoticons, I'm assuming it's either Gemini or Sonnet. It consumed the AGENTS.md file properly and knew where to look for test creation guidelines ("Read src/test-creation.md"), and it took at look at everything relevant: the main wrapper class, its interface, the endpoints, the test project, etc.

Steve Kinney posted [this article on Cursor models](https://frontendmasters.com/blog/choosing-the-right-model-in-cursor/) in September 2025, really high level but summarizes what the different models are.

It pointed to the `LatestExchangeRateEndpointTests.cs` file saying that there were test guidelines in there, but nothing was actually implemented in that file. It was just the typical NUnit boilerplate that's generated with every new test class. This was a little weird and it's probably a hallucination? Didn't like how strange this was.

I was pleasantly surprised that it followed the instructions in the markdown file perfectly and followed some of my personal preferences, i.e. it created the Arrange-Act-Assert comments and treated them as sections, and if Act and Assert were the same operation, it wrote the comment as "Act & Assert".

I didn't like how it considered "proper coverage" splitting up an "invalid api key" test into a test for a null input, an empty string, a whitespace string, a string with a tab character, and a string with a newline character. It seemed to consider the volume of tests worth it, meanwhile NUnit supports [TestCase attributes](https://docs.nunit.org/articles/nunit/writing-tests/attributes/testcase.html?q=TestCase) right out of the box and this could have been one test method. Considering the agent knew via the AGENTS.md file that the project was using NUnit and it's all just pattern matching & linear algebra (go look at the NUnit docs and find something like "parameterized test method"), this feels somewhat like a dealbreaker: I shouldn't *have* to tell NUnit that common library functionality exists.