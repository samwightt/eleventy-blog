---
title: Smaller components with Svelte
description: Svelte is a brand-new framework that brings exciting opportunities to the browser. Smaller file size is just the start.
layout: layouts/post.njk
---

I've been using Svelte as my Javascript framework of choice for the past few weeks, and I've gotta say: it's fucking good.

A lot of Javascript frameworks care about bundle size and make a big show of how small their framework is. React developers celebrate when the framework is 60kb gzipped, and Vue developers shit on React developers because Vue is supposedly *much* smaller and faster than React. There's this big competition between different framework authors to reduce the amount of code that the core framework requires in order to get running at any cost. If your bundle size is smaller, then things must be faster.

Svelte takes a different approach. It tries to reduce its bundle size as much as possible, but in a creative way: *it compiles your code before it's used in the browser*. In Svelte, you write your components in a superset of HTML, CSS, and Javascript (you might call it 'SvelteScript'). Svelte takes all your code, runs it through a compiler, and outputs simple, efficient Javascript files that perform updates to the DOM atomically, with near-zero overhead. There's no need for a virtual DOM because the hard work of reconciliation is moved to the *compilation step* and done using static analysis.

As a result, the core framework code (the code that manipulates the DOM) is **much smaller** than many other frameworks:

As a result, Svelte is faster than its competitors. It has no need for a virtual DOM, no need to do diffing client-side, and no need to do a lot of the 'framework' work on the client. That's all done before the code gets sent to the user's browser.

In his post, Rick offers a quote from Pete Hunt, one of the former React core developers. In his talk, Pete talks about React and its performance:

> React is not magic. Just like you can drop into assembler with C and beat the C compiler, you can drop into raw DOM operations and DOM API calls and beat React if you wanted to. However, using C or Java or JavaScript is an order of magnitude performance improvement because you don't have to worry...about the specifics of the platform. With React you can build applications without even thinking about performance and the default state is fast.\

In this analogy, React is not the C compiler, but Ruby or Javascript. It's like an interpreted language that must be parsed before it's executed. And like an interpreted language, there's a lot of overhead. React has to do diffing. It has to do lots of checks and execute all of your component code in the right way. It has to manage state. It now (thanks to server components) has to handle communication with the server. It has to manage event lifecycles and APIs that avoid forcing you to diff data every time. React has to do a lot for you, and it does it all *in the browser*.

Svelte takes the opposite approach: instead of interpreting the framework, it *compiles* it. All that work is done before the code even touches the browser, which means things are faster for your users, that the associated framework code is **much smaller**, and that there's less for you to think about with performance.

Svelte is performant by default. React is unperformant by default. Again, to quote Rich:

> The original promise of React was that you could re-render your entire app on every single state change without worrying about performance. In practice, I don't think that's turned out to be accurate.