---
theme: seriph
# See https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
title: What is DDD and why should you care
info: |
  ## Introduction to Domain Driven Design
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: fade
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# What is DDD... <br /> and why should you care?

A brief introduction to domain driven design.

<!--
And how those ideas changed the way I think about our craft.

In this occasion I'm not even going to talk about architecture. 

Domain Driven Design is the set of principles that guide your architecture decisions.
-->

---
transition: slide-left
layout: image-right
image: /assets/images/laptop-with-code.jpg
backgroundSize: contain
class: flex flex-col justify-center
---

# Let's start with a simple question

### What do you think your job is, as a developer?
In other words, what do you think they are paying you to do? 

<!--
For the most time, I thought that my job as a developer was to write code. 
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
transition: slide-up
layout: quote
class: flex flex-col justify-center align-center
---

#

<img
    class="opacity-100"
    src="/assets/images/goal.png"
/>

<!--
The goal of DDD is to help identify, protect, and build this Domain.

You want your developers to help you solve your business problem with the tools they know how to use.
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
It is the learning process, not the end goal, which is the greatest strength of DDD.

Any team can write a software product to meet the needs of a set of use cases.

But teams that put time and effort into the problem domain they are working on, can consistently evolve the product to meet new business use cases.
-->

---
layout: image-right
image: /assets/images/gemini.webp
backgroundSize: contain
class: flex flex-col justify-center
---

# Why is it important for developers to focus on Domain?

<!--
- Google gemini fiasco anecdote.
- In a previous startup I was working at, shareholders wanted to see a specific functionality working ASAP.
Backend spent weeks implementing a layer of data analysis around an external api implementation while the investors only wanted to see the thing working.
With more communication and context we could’ve focused on implementing the functionality in the FE, by communicating directly with the external API.
This would've given the shareholders the assurance they wanted, and it would have given our team more time to implement the data analysis for it correctly.
-->

---
transition: slide-up
layout: quote
---

# DDD provides a set of techniques to facilitate the understanding of the Domain

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
These techniques are cultural and organizational, as much as they are technical patterns applied in the code.
-->

---
transition: slide-up
layout: quote
---

# Technical implications of DDD

- Developers should be able to learn about the domain solution by reading the code
- Domain experts should be able to read and understand the code that applies the solution
- You are responsible for providing proof that your implementation works 

<!--
The goal of these technical patterns is to make it possible for the engineers reading the code to learn about the domain. Your domain experts should likewise be able to read the code (even if they are not devs) and understand what’s going on, because they understand the solution. The importance of ubiquitous language becomes more evident in this context.

Uncle Bob's comment on the responsibilities of providing proof that the solution works. 
-->

---
layout: image-right
image: /assets/images/react-server-components.webp
class: flex flex-col justify-center
---

# Re-reading SOLID around the Domain

<!--
How do you re-think Separation of Concerns with a Domain-centre approach in this example? 
-->

---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center
---

# A day in the life
of a dev without ddd

::right::

<div
    class="h-full w-full"
    style="background: url('/assets/images/no-ddd-day-1.png') top;background-size: contain;"
></div>

---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center
---

# Next day in the life
of a dev without ddd

<div
    class="h-full w-full"
    style="background: url('/assets/images/drake-no.png'); background-size: contain; background-repeat: no-repeat"
></div>

::right::

<div class="h-full w-full" style="padding-top: 89px">
    <div
        class="h-full w-full"
        style="background: url('/assets/images/no-ddd-day-2.png'); background-size: contain; background-repeat: no-repeat;"
    ></div>
</div>

---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center
---

# A day in the life
Of a developer on DDD

<div
    class="h-full w-full"
    style="background: url('/assets/images/drake-yes.png'); background-size: contain; background-repeat: no-repeat"
></div>

::right::

<div
    class="h-full w-full"
    style="background: url('/assets/images/ddd-day-1.png') top;background-size: cover;"
></div>

---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center overflow-visible
---

# A day in the life
Of a developer on DDD

::right::

<div
    class="h-full w-full"
    style="background: url('/assets/images/ddd-day-2.png') center bottom;background-size: cover; background-repeat: no-repeat; height: 595px;"
></div>


---
transition: fade
layout: center
---

# Resources

- [Article I wrote: betterprogramming.pub/domain-driven-architecture-in-the-frontend](https://betterprogramming.pub/domain-driven-architecture-in-the-frontend-i-d27fb71b5cb0)
- [Same one but free: dev.to/blindpupil/domain-driven-architecture-in-the-frontend](https://dev.to/blindpupil/domain-driven-architecture-in-the-frontend-i-1f41)
- Patterns, Principles, and Practices of Domain-Driven Design. Scott Millett, Nick Tune

---
layout: end
---

# Thank you

If you're interested and want to hear more, reach out!
