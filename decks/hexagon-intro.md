---
title: Introduction to Hexagonal Architecture
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

<!--
This is the 3rd one in a series of talks I've been giving about frontend architecture.
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
Domain Driven Design is a philosophy that allows developers to rethink their job as solving domain problems, as opposed
to closing tickets or meeting requirements. Once you incorporate this philosophy, many things in your day-to-day will
likely start to look different:
- During a sprint planning you will want to make sure that the product owner doesn't describe the technical solution in the ticket,
but describes the business problem instead. 
- When reviewing the design implementation, you will want to make sure you understand the specific business uses for each button and each workflow. 
- During code reviews you will start paying attention to the language used in the code. And you will start caring not 
only about having the code work, but also about having it be consistent with your understanding of the domain.
-->
---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center
---

# 2. The way you're taught to write frontend code is not DDD-friendly

- Overreaching stores: most current client-side frontend applications rely on a global store that is responsible for fetching the data, caching it, adapting it, and managing its state.
- No separation between UI and domain state: you can have a property for the open-closed state of a modal, right next to a list of recipes.
- Overreaching components: no clear definition of what logic belongs in components and what logic belongs in the store.

::right::

<img
class="w-86"
src="/assets/images/typical-frontend.webp"
alt=""
/>

<!--
In the second talk I called "architecture disasterclass" we discussed the limitations of the current way we learn to write frontend code.
In that talk we went over many of the issues we face if we apply this blindly, and I left you in this cliffhanger  
-->


---
transition: fade
layout: center
class: text-center
---

# The Solution 🥁

To be discussed in the next Meetup!


---
transition: slide-left
layout: image-left
image: https://cover.sli.dev
---

# Remembering Architecture Goals
Your architecture should enable you to:

1. Produce maintainable code. <span v-click class="ml-4 text-red-600">Tested SOLID</span>
2. Produce future-proof code. <span v-click class="ml-4 text-red-600">Hexagonal</span>
<ul>
  <li> Replace View Layer without touching Domain or Infrastructure (api layer, data source) </li>
  <li> Evolve the Domain without touching Infra or View </li>
  <li> Replace the Store with as little friction as possible </li>
</ul>
3. Produce educational code. <span v-click class="ml-4 text-red-600">DDD</span>


<!--
I hope you're ready for the solution, because it's coming. Before we get there though, let's go over the architecture 
goals: what is it we want to accomplish by implementing this architecture?

1. Adding new features or refactors should not introduce regressions.
2. Future-proof your code: you should be able to grab your business logic out of your Vue application, and use it in a React application, or outside the browser in a CLI tool, or with a voice interface, whatever. Removes vendor lock-in. 
3. New developers (as soon as they are familiarized with the concepts in the architecture) should be able to learn the Domain from the code.

[click] In the previous presentation I briefly mentioned that you should be able to get to your first goal by implementing Test SOLID code.

[click] The second goal is achieved by implementing Hexagonal Architecture which is the topic of this talk.

[click] The third goal is achieved by engraving the DDD philosophy on everyone that touches your codebase, and specially everyone that reviews it.

Truth is that Hexagonal Architecture won't only help you write future-proof code, it will also help you write better 
Tested and SOLID code, and will also help you write educational code.
-->


---
transition: slide-left
layout: image-left
image: /assets/images/classic-hexagon.png
---

# Hexagonal Architecture... in the frontend? 

Yes... and no. 

<!--
If you type Hexagonal Architecture in Google, you will first see this diagram which, if you're a frontend developer, it
may look familiar, but as a frontend it may scare you off. I made a new one that is more friendly to frontend developers:
-->


---
layout: image
backgroundSize: contain
image: /assets/images/frontend-hexagon.webp
class: flex flex-col justify-center
---

# 

<!--
(Sidenote, backend developers in my company are now pointed to my frontend architecture articles as part of the 
documentation for the backend, even though is actually more to learn about the principles than the actual implementation 
in the code.)

Hexagonal architecture is all about single responsibility and separation of concerns. As a matter of fact, all of the
issues I pointed out with the current way we learn to write frontend code are just a function of elements not having clear
responsibilities: store state can hold anything, the stores actions can do whatever, the store getters can do what they
please, you can do whatever you need in your components, all that matters is that it works. That's the mentality I first
learned frontend in (and if you go back to my old projects, you can clearly see that mentality in the code). 

So let's define responsibilities real quick, and then we can dive in a bit deeper into each one:
- Secondary is everything that's external to your application: all your services, your APIs, etc.
- Domain is what your application needs. All the classes, interfaces and __core domain__ complexity should be in the domain.
- Primary is how your application implements the domain.

Only with this you can already start seeing how this helps you future-proof your code: since your domain doesn't care how that
data is obtained, you can swap your secondary layer without touching the domain (this is the D in SOLID, dependency inversion).

At the same time, your domain should (ideally) not care about where your application is running: core domain logic is the
same whether your app is running in the browser, in a CLI tool, or in a voice interface. This helps you understand what
goes in your domain and what doesn't.
-->


---
transition: slide-left
layout: image-left
image: /assets/images/hexagon-secondary.png
---

# Secondary
- Secondary implements the Domain Repository interface (that is its single responsibility)

From that single responsibility we can infer the following rules for secondary: 
- It's the only place where domain objects are instantiated (secondary always return domain objects)
- It's the only place that interacts with external services
- It's the only place where the caching strategy is defined


<!--
Secondary definition:
It's your infrastructure layer. Everything external to your application is secondary. All the data sources are secondary: 
local storage, IndexDB, and even your Store (Pinia, Vuex, Redux, whatever) could be considered secondary. If you don't 
have control over what's coming in, it's secondary. If you decide it's secondary, it's secondary. 
[example of project where backend 3d developer asked me if there was a way to make sure frontends cannot access Pinia directly...] 

Repository Interface definition:
There's a reason why Domain is at the center. Whatever Secondary does, it's decided by an interface in the Domain called
Repository (if the naming doesn't make sense to you, decide whatever naming makes sense for your team). The repository in
the Domain is a list of methods that either return Domain Objects, or do stuff that the Domain needs to have done. Your
domain objects are your domain classes or interfaces.
[The domain is like a bad boss that tells everyone what they should do, but does nothing itself, unless there's real money
to be made, then that's where it shows it cares]
-->


---
transition: slide-left
layout: image-right
image: /assets/images/hexagon-primary.png
---

# Primary
- Primary uses the domain to provide the use cases for your application

From that single responsibility we can infer the following rules for primary:
- It's the only place domain methods can be called
- It can never expose the domain objects it uses (primary always returns view objects)

<!--
Primary definition:
However your application is implemented is done in Primary. The objects and data structures that your primary produces
are to be adapted to, in the case of web developers, the browser context. Domain defines whatever it needs, but in Primary
I'm going to expose the data that is useful to how I'm going to present it in the browser, and how the users will interact with it. 

View Object definition:
Since Domain objects can't be accessed anywhere in the components (that's one of the ways we protect it), we have View
Objects instead. We can make the View Objects as adapted as they need to be to our application. View Objects can contain 
data that pertains only to how that data is displayed in the UI. And your UI should never need to care about the Domain,
only about the View Objects. This is also how the Hexagon helps us future-proof our app: if we want to make the same app
in a different context (like a CLI tool or whatever) we can keep our domain and our secondary and only ditch the primary.
-->


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
