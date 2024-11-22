---
title: Domain Driven Design for Frontend Developers
info: |
  ## Ideas to guide your architecture decisions
layout: image
image: /assets/images/dddbg.webp
class: text-center
---

# Plan

### 1. Why am I here
### 2. What's DDD and who is it for
### 3. Practical examples for frontend devs


<style>
h3 {
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
- How looking to solve impostor syndrome led me to realize I needed learn about architecture.

- Why learning about new coding patterns wasn't enough.
At that point of my career, I chose coding pattern and newer apis just because I liked the syntax, or because I found
them intuitive for me. Other than that, I didn't have the arguments to prefer one way over the other. 

At the end of this talk, you should have the tools you need to come up with those arguments, and defend your 
architectural choices beyond "I like it" or "that's the only way I know how to do it". 

At that point, good architecture was just code you're able to easily understand when you get back to it after some time.
That is maintainable code, because I can maintain it tomorrow without throwing the laptop out the window.
In short, I wanted simplicity: how do I maintain my code simple as things get more complex?
-->


---
transition: slide-up
layout: image
image: /assets/images/the-light-of-ddd.jpg
---

# Find <br> DDD

<!--
In the search of simplicity, I stumbled upon a Domain Driven Design meetup in Lyon where I lived.
Luck smiled at me and I got hired by a company implementing these ideas where some of the people of the meetup were 
also working.

I've never been happier to go to work that when I worked in a team that embraced this philosophy. 
I'm here preaching the DDD gospel to anyone who listens because I want to persuade devs to try out these ideas, and 
perhaps increase the chances that I find myself in a team working like that again.

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
DDD is a very simple idea at its core. What is "Domain" though?

Your company provides a solution that solves some problem: products.
At first glance, Domain is the logic in that solution.

Beyond business logic though, Domain is whatever provides value to your company.
What makes your solution worth it, what is its unique selling point.

It takes some practice to begin getting a sense of what is Domain in your codebase. Domain can be very very different
from one company to another, and even from one team to another within the same company.

No one knows your Domain better than you do. All I can do is give you a couple of tools to help you guide your thought 
process.

I have a couple of examples that could hint you in the right direction:
- If you're a web developer you're building something for the browser, but imagine that you're tasked to move that 
application so that it works in the terminal as a command line application. What code could you keep unchanged? Imagine
you're moving it to a voice interface like an Alexa. What code could you keep unchanged? It's likely that's your Domain.
- An example closer to web development: you're implementing a feature in your frontend, among all the logic that you 
have to implement ask yourself, what is there because it's a requirement from the
designer? What is there because it's a limitation of your backend or your infrastructure? What is there because it's a
product requirement? Only the latter is likely to be part of your Domain. 
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
If even after my definition of Domain, it still sounds hard for you to know what is Domain in your application, then
you're not alone. This is the problem that DDD is trying to solve. DDD provides a set of techniques to facilitate the
understanding of the Domain. The promise of DDD is that by following these practices, you'll be able to
consistently and sustainably identify, protect and extend your Domain, regardless of it's level of complexity. The 
simplicity I was looking for, it makes it possible and lays out a path for you to get there.

"The better your understanding of the domain problem, the better the code you'll be able to write for the solution".

The idea of placing Domain at the center has implications not only for the developers, but for the entire organization. 
DDD provides a set of techniques to facilitate the implementation of these ideas.

Matter of fact, almost all of your common activities as a developer change in a team that takes this seriously: code
reviews are a big one, but also sprint planning, retros, and even the way you do your daily standup changes. Agile (or
any other iterative development process) is actually core to DDD. 

(Anecdote about the sprint planning ticket point designation)

This mentality is not just to bother or because I like it. It has saved my bacon in practice more than once.
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
This is the elevator pitch when you can use when trying to sell DDD to a team or an organization.

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
The goal of DDD is to provide engineers with the appropriate understanding of the Domain.
A big leap in that direction is to make it possible for the engineers reading the code to learn about the Domain.

Likewise, Domain experts should be able to read the code (even if they are not devs) and understand what’s going on,
just because they understand the solution. The importance of ubiquitous language becomes more evident in this context.

People versed in DDD would probably shrug the oversimplification I do here of the technical implications of DDD.

That's on purpose. There are challenges in being prescriptive about architecture details, coding practices in general.
-->


---
transition: slide-up
layout: quote
---

# The challenge of prescriptive architecture

<h3 v-click> DRY: don't repeat yourself </h3>

<h3 v-click> WET: write everything twice </h3>

<h3 v-click> AHA: avoid hasty abstractions </h3>

<div v-click>
<p> Functions/Classes/Files should not be too long </p>
<p> No Comments vs Fully Commented </p>
</div>

<style>
h3 {
  margin: 2rem;
  font-family: system-ui;
}
</style>

<!--
Take the idea behind DRY for example. Sounded like a good idea until people started taking it too far. And so then 
someone came up with the idea of WET (Write Everything Twice) to counteract that. But then people took that one to heart
and then we had AHA (Avoid Hasty Abstractions). DRY is a sound idea. But it's also better to have duplication over the
wrong abstraction. 
Same can be said about the keeping the length of files or functions to low. If you have this rule and what you end-up 
doing is removing empty lines or inlining statements to keep your function to a certain size, then you're missing the
point.
Same can be said about the discussion on whether to write comments in your code or not. 

In all of these cases is not the principle itself that matters. Is how it's applied in the context of your Domain.
There's no single silver bullet principle you can apply all the time, everywhere.
But there's still a lot to gain just from focusing on the principles without over-prescribing solutions.

(Anecdote about long classes and long functions)
(Anecdote about commented code)

These principles are the good old Clean Code practices and SOLID principles you've likely heard of. With DDD you learn 
to apply these principles at scale.

Now, let's find an answer to the question we had in the beginning: how to maintain simplicity in my code even as the
domain solution becomes more and more complex?

As you might've guessed by now: you should probably first identify and define your domain.
-->


---
transition: slide-left
layout: image
image: /assets/images/typical-frontend.webp
backgroundSize: contain
---

<img v-click src="/assets/gifs/where-is-it.gif">


<!--
With this in mind, let's look at how a typical architecture design looks like in a frontend codebase.
Note there's a View layer, an API or Infrastructure layer, and everything in between.

See a problem here? Where does the domain go? 
-->


---
transition: fade
---

# Try adding a Domain directory in your Frontend app
If it makes sense to do so

At first, it will contain the models (classes and interfaces) that make up your application.

Put here what you think your application needs.

<img src="/assets/gifs/what-i-want.gif" width="360px">

<div class="flex justify-center">
<img src="/assets/images/simple-domain.png">
</div>
<!--
Just the fact that you have will to think about what to put in there will make you a better developer.
This also provides a place where devs can quickly see what your application is about.
-->


---
transition: slide-up
layout: quote
---

# Define what you need in a Contract in your Domain
If it makes sense to do so

If whatever your application needs is provided externally (api, socket, whatever), make a contract defining it:

```typescript
export interface RecipeContract {
  recipes: () => Promise<Recipe[]>
  save: (data: RecipeToSave) => Promise<Recipe>
  recipe: (id: RecipeId) => Promise<Recipe>
}
```

<div class="flex justify-between">
<img src="/assets/images/simple-inside-domain.png" width="288px">
<img src="/assets/images/contract.webp">
</div>
<!--
Remember I said to imagine that you're moving your application to a different platform like a CLI or Alexa? 
With this you can be certain that whatever you keep in your Domain, you can keep unchanged as you move it.
-->


---
transition: slide-up
layout: quote
---

# Have something implement your contract
If it makes sense to do so

```typescript
export class RecipeResource implements RecipeContract {
  recipes(): Promise<Recipe[]> {
    // ...
  }
  save(data: RecipeToSave): Promise<Recipe> {
    // ...
  }
  recipe(id: RecipeId): Promise<Recipe> {
    // ...
  }
}
```

<div class="flex justify-between">
<img v-click src="/assets/gifs/where-it-goes.webp">
<img v-click src="/assets/images/simple-structure-1.png" style="object-fit: contain">
</div> 

<!--
Where does this go though? This cannot be in the Domain directory, anymore. Why? Because it's an implementation detail.

This thing here that implements the contract has a different responsibility than the Domain. 

This can be part of your infrastructure, or external layer, or whatever you want to call it. I learned to call it 
Secondary.
-->


---
transition: slide-up
layout: quote
---

# Distinguish between your presentation logic and your Domain 
If it makes sense to do so

Keep your components as dumb as possible from the Domain.

How about a service?

```typescript
export class RecipeService {
  constructor(private contract: RecipeContract) {}
  async saveRecipe(recipeToSave: RecipeToSave) {
    // Business logic here
    return this.contract.save(recipeToSave)
  }
}
```
<div class="flex justify-center">
<img v-click src="/assets/images/simple-structure-2.png" style="object-fit: contain">
</div>

<!--
Now you have a Domain directory for that.
Your components now don't need to know about the Domain details.
Your components now don't need to know about the API details, that's secondary.
Now your components need to only focus on what they do best: displaying stuff and handling events and user interactions.
That's, by the way, what the frontend frameworks are good at.
-->


---
transition: slide-up
layout: quote
---

# Distinguish reusable components from application-specific ui
When would it not make sense to do this?

This will allow you to reuse components across different applications, and also simplify your application-specific ui
components.

<div class="flex justify-center">
<img src="/assets/images/simple-structure-3.png" style="object-fit: contain">
</div>


---
transition: fade
layout: quote
---

# Rethinking ideas with DDD
How to make your opinion

<strong>Frameworks</strong>
<div class="align-center" style="display: grid; grid-template-columns: repeat(6, auto)">
    <img src="/assets/images/react-logo.png" style="object-fit: contain" width="100px">
    <img src="/assets/images/vue-logo.png" style="object-fit: contain" width="120px">
    <img src="/assets/images/angular-logo.png" style="object-fit: contain" width="110px">
    <img src="/assets/images/qwik-logo.png" style="object-fit: contain">
    <div class="flex align-center">
    <img src="/assets/images/solid-logo.png" style="object-fit: contain">
    </div>
    <img src="/assets/images/svelte-logo.png" style="object-fit: contain" width="100px">
</div>

<strong>State management</strong>
<div class="align-center" style="display: grid; grid-template-columns: repeat(4, auto)">
    <img src="/assets/images/mobx-logo.png" style="object-fit: contain" width="120px">
    <img src="/assets/images/redux-logo.png" style="object-fit: contain" width="120px">
    <img src="/assets/images/pinia-logo.svg" style="object-fit: contain" width="100px">
    <div class="flex align-center">
    <img src="/assets/images/xstate-logo.svg" style="object-fit: contain" width="120px">
    </div>
</div>


<!--
How do you re-think Separation of Concerns and Single Responsibility with a Domain-centre approach in this example?

(Anecdote about DDD on unique fields in the database)
-->


---
layout: image-right
image: /assets/images/react-server-components.webp
class: flex flex-col justify-center
---

# Rethinking ideas with DDD


<!--
How do you re-think Separation of Concerns and Single Responsibility with a Domain-centre approach in this example?

(Anecdote about DDD on unique fields in the database)
-->


---
transition: fade
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
