---
title: Domain Driven Design for Frontend Developers
info: |
  ## Ideas to guide your architecture decisions
layout: image
image: /assets/images/dddbg.webp
class: text-center
---

# Plan

## Why am I here
## What's DDD and who is it for
## Practical examples for frontend devs


<style>
h2 {
  color: rebeccapurple;
  margin-bottom: 1rem;
  font-family: system-ui;
}
</style>

<!--

-->


---
transition: slide-left
layout: image
image: /assets/images/why-doesnt-work.jpeg
backgroundSize: contain
class: flex flex-col justify-center
---

# A <br> better <br> way <br> of <br> working

<!--
How looking to solve impostor syndrome led me to realize I needed learn about architecture.

Why learning about new coding patterns wasn't enough.

At this point, good architecture is just code you're able to easily understand when you get back to it after some time.
That is maintainable code, because I can maintain it tomorrow without throwing the laptop out the window.
Simplicity, in short: how do I maintain my code simple as things get more complex?
-->


---
transition: slide-up
layout: image
image: /assets/images/the-light-of-ddd.jpg
---

# Find <br> DDD

<!--
I've never been happier to go to work that when I worked in a team that embraced this philosophy. 
I'm here preaching the DDD gospel to anyone who listens because I want to persuade devs to try out these ideas, and 
perhaps increase the chances that I find myself in an organization like that one again.

What made it so awesome?
A day in the life of a dev in DDD...
-->


---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center
---

# Domain Driven Design

"Put Domain at the centre of your focus"

::right::

<img
    v-click
    class="w-86"
    src="/assets/images/domain=solution.png"
    alt=""
/>

<!--
DDD is a very simple idea at its core. What is Domain though?

Your company provides a solution that solves some problem: products.
At first glance, Domain is the logic in that solution.

Beyond business logic though, Domain is whatever provides value to your company.
What makes your solution worth it, what is its unique selling point. 
-->


---
transition: fade
---

# Domain Driven Design (DDD)
The problem: increasing complexity over time. <br> The solution: DDD

DDD provides a set of techniques to facilitate the understanding (and modeling) of the Domain 

<p>

**Cultural**
- Develop and use "Ubiquitous Language"

**Organizational**
- Provide developers with access to __domain experts__
- Include engineers when the business solution is being discussed

**Technical**
- Domain driven code
- Test your solution

</p>

<!--
The tagline for Eric Evans book is "Tackling complexity in the heart of software". The idea of placing Domain at the 
center has implications not only for the developer, but for the entire organization. 
DDD provides a set of techniques to facilitate the implementation of these ideas.

Matter of fact, almost all of your common activities as a developer change in a team that takes this seriously: code
reviews are a big one, but also sprint planning, retros and even the way you do your daily standup changes. Agile (or
any other iterative development process) is actually core to DDD. 

This makes the difference between a team simply capable of writing software to meet a specified set of use cases, and a
team capable of consistently evolving the product to meet new business use cases.

"The better your understanding of the domain problem, the better the code you'll be able to write for the solution".
-->


---
layout: quote
class: text-center
---

#

"If all your developers are doing is writing code, <br>
then you’re wasting half of the money you’re paying them."
<br>
<span class="text-sm text-cyan">Eric Evans... maybe</span>

<!--
This is the elevator pitch when I'm trying to sell DDD in an organization.

It is the learning process, not the end goal, which is the greatest strength of DDD.

Any team can write a software product to meet the needs of a set of use cases.

But teams that put time and effort into the problem domain they are working on, can consistently evolve the product to 
meet new business use cases.
-->


---
transition: slide-up
layout: quote
---

# Technical implications of DDD

- Developers should be able to learn about the domain solution by reading the code
- Domain experts should be able to read and understand the code that implements the solution
- You are responsible for providing proof that your implementation works

<!--
The goal of these technical patterns is to make it possible for the engineers reading the code to learn about the domain.
Domain experts should, likewise, be able to read the code (even if they are not devs) and understand what’s going on,
because they understand the solution. The importance of ubiquitous language becomes more evident in this context.

People versed in DDD would probably shrug the oversimplification I do here of the technical implications of DDD.
There are challenges of being prescriptive about architecture details.

However I believe there's still a lot to gain just from focusing on the principles without over-prescribing solutions.
These principles are the good old Clean Code practices and SOLID principles you've likely heard of. DDD just provides a
focus point to enable you to apply these principles at scale.

Now, let's find an answer to the question we had in the beginning: how to maintain simplicity in my code even as the
domain solution becomes more and more complex?
The first hint is: you should first define, protect and focus on your domain. 
-->


---
transition: slide-left
layout: image
image: /assets/images/typical-frontend.webp
backgroundSize: contain
---


<!--
With this in mind, let's look at how a typical architecture design looks like in a frontend codebase.
Note there's a View layer, an API or Infrastructure layer, and everything in between.

See a problem here? Where does the domain go? 
-->


---
transition: slide-up
layout: quote
---


# Resources

- These slides: [github.com/Blindpupil/presentations](https://github.com/Blindpupil/presentations)
- [Article I wrote: betterprogramming.pub/domain-driven-architecture-in-the-frontend](https://betterprogramming.pub/domain-driven-architecture-in-the-frontend-i-d27fb71b5cb0)
- [Same one but free: dev.to/blindpupil/domain-driven-architecture-in-the-frontend](https://dev.to/blindpupil/domain-driven-architecture-in-the-frontend-i-1f41)
- Patterns, Principles, and Practices of Domain-Driven Design. Scott Millett, Nick Tune

---
src: /pages/thank-you.md
---
