---
title: Hexagonal Architecture Implementation Example
info: |
  ## Architecture proposal for the frontend
layout: image
image: https://cover.sli.dev
class: text-center
---

# Recap
How did we get here?

Previously:

1. Introduction to Domain Driven Design
2. Architecture Disasterclass
3. Introduction to Hexagonal Architecture

<!--
This is the 4rth one in a series of talks I've been giving about frontend architecture.
Let's go over the main points we've learned so far.
-->


---
transition: slide-left
layout: image-right
image: /assets/images/laptop-with-code.jpg
backgroundSize: contain
class: flex flex-col justify-center
---

#  1. Place Domain at the centre of your focus

- The better your understanding of the domain, the better the code you'll be able to write for it
- The practice of Domain Driven Design is aimed to facilitating the understanding of the Domain (ubiquitous language, domain experts access, event storming, etc.)
- Developers should be able to learn about the domain by reading the code
- DDD has a cost

<!--

-->


---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center
---

# 2. The way we're taught to write frontend code is not DDD-friendly

- There's no single responsibility defined for the elements of our FE app (components, composables, store state, getters, actions)
- There's little to no separation between UI and Domain
- There's usually no separation between reusable components and domain-specific components

::right::

<img
class="w-86"
src="/assets/images/typical-frontend.webp"
alt=""
/>

<!--

-->


---
transition: slide-left
layout: image-left
image: https://cover.sli.dev
---

# Remembering Architecture Goals
Your architecture should enable you to:

1. Produce maintainable code. <span v-click class="ml-4 text-red-600">SOLID and Tested</span>
2. Produce future-proof code. <span v-click class="ml-4 text-red-600">Hexagonal</span>
<ul>
  <li> Replace View Layer without touching Domain or Infrastructure (api layer, data source) </li>
  <li> Evolve the Domain without touching Infra or View </li>
  <li> Replace the Store with as little friction as possible </li>
</ul>
3. Produce educational code. <span v-click class="ml-4 text-red-600">DDD</span>

<!--
Let's go over the architecture goals one last time: what is it we want to accomplish by implementing this architecture?

1. Adding new features or refactors should not introduce regressions. 
2. Future-proof your code: your application layers should be easily replaceable (domain does not depend on UI, application does not depend on framework, domain does not depend infrastructure, etc). 
3. Code should teach developers about the domain.

[click] In the previous presentation I briefly mentioned that you should be able to get to your first goal by implementing Test SOLID code.

[click] The second goal is achieved by implementing Hexagonal Architecture which is the topic of this talk.

[click] The third goal is achieved by engraving the DDD philosophy on everyone that touches your codebase, and specially everyone that reviews it.

Hexagonal Architecture helps you in all of these aspects.
-->


---
layout: image
backgroundSize: contain
image: /assets/images/frontend-hexagon.webp
class: flex flex-col justify-center
---

# Let the Hexagon help you

<!--
Hexagonal architecture is all about single responsibility and separation of concerns.So let's define responsibilities 
real quick, and then we can dive in a bit deeper into each one:
- Secondary is everything that's external to your application: all your services, your APIs, etc.
- Domain is what your application needs. All the classes, interfaces and __core domain__ complexity should be in the domain.
- Primary is how your application implements the domain.
- UI is where the domain-specific Vue (or any other framework) components will live. 

Your domain doesn't care how that data is obtained. You can swap your secondary layer without touching the domain.
At the same time, your domain should (ideally) not care about where your application is running: core domain logic is the
same whether your app is running in the browser, in a CLI tool, or in a voice interface.
Your Primary is the one who cares about your platform (the browser). 
Your UI is the one who cares about the Frontend framework.
-->


---
transition: fade
layout: center
class: text-center
---

# To the Code ! 🥁


---
transition: fade
layout: center
---

# Resources

- [Article I wrote: betterprogramming.pub/domain-driven-architecture-in-the-frontend](https://betterprogramming.pub/domain-driven-architecture-in-the-frontend-i-d27fb71b5cb0)
- [Same one but free: dev.to/blindpupil/domain-driven-architecture-in-the-frontend](https://dev.to/blindpupil/domain-driven-architecture-in-the-frontend-i-1f41)
- Patterns, Principles, and Practices of Domain-Driven Design. Scott Millett, Nick Tune

---
src: /pages/thank-you.md
---
