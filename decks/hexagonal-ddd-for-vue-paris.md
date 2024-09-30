---
title: Hexagonal DDD In Vue, for Paris
info: |
  ## Architecture proposal for the frontend
layout: image
image: https://cover.sli.dev
class: text-center
---

# Plan

- Why am I here
- What's DDD
- The Hexagon
- When should you ignore me


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

-->


---
transition: slide-up
layout: two-cols
class: flex flex-col justify-center items-center
---

# A better way of working

<img
src="/assets/images/drake-yes.png"
alt="Drake approves"
class="ml-34"
/>

::right::

- Discuss with product owner (who made the ticket)
- Discuss with the designer
- Discuss with other team members
- Implement a tested solution you can be proud of

<!--
No more fighting with bugs. Not that bugs never appear, is just that when they do, they usually reveal and underlying
misunderstanding of the domain. They aren't really bugs, more like unidentified requirements.
So even bug tickets are interesting.

Full disclosure: Hexagonal DDD wasn't the only reason this worked for me, we also has 99% coverage across the whole test
pyramid, and that was my first time using full TypeScript everywhere. That was back when we had to use Vue class components.  
-->


---
transition: fade
layout: center
class: text-center
---

# Domain Driven Design (DDD)
The problem: increasing complexity over time. <br> The solution: DDD 

<div class="flex">
  <section class="mr-8">
    <h3>For the organization</h3>
    <ul class="text-left">
      <p>Domain expertise is made available</p>
      <p>Ubiquitous language</p>
    </ul>
  </section>

  <section>
    <h3>For the developer</h3>
    <ul class="text-left">
      <p>Code should teach readers about the domain</p>
      <p>Use domain language in the code</p>
    </ul>
  </section>
</div>

<!--
The tagline for Eric Evans book is "Tackling complexity in the heart of software". The solution can be resumed with: 
"Put Domain at the centre of your focus". This idea has implications not only for the developer, but for the entire
organization. DDD provides a set of techniques to facilitate the implementation of these ideas.

This makes the difference between a team simply capable of writing software to meet a specified set of use cases, and a
team capable of consistently evolving the product to meet new business use cases.

"The better your understanding of the domain problem, the better the code you'll be able to write for the solution".
-->


---
transition: slide-left
layout: two-cols
class: flex flex-col justify-center items-center
---

# Hexagonal architecture (applied DDD)
... in the frontend?

::right::


<img
  v-click.hide
  src="/assets/images/hexagonal-architecture-with-control-flow.png"
  alt="Hexagon representation 1" 
  attribution="https://www.happycoders.eu/software-craftsmanship/hexagonal-architecture/"
/>
<img 
  v-click="1"
  src="/assets/images/hexagon-representation-2.webp" 
  alt="Hexagon representation 2" 
  attribution="https://medium.com/ssense-tech/hexagonal-architecture-there-are-always-two-sides-to-every-story-bc0780ed7d9c"
/>

<!--
Hexagonal architecture is one way to apply DDD in your code which comes with many advantages and a few drawbacks.
This is how I've learned to apply it in the frontend so far. 

If you type Hexagonal Architecture in Google, you will first see this diagram which, if you're a backend developer, it
may look familiar, if you're a frontend it may scare you off. I made a new one that is more friendly to frontend developers:
-->


---
layout: image
backgroundSize: contain
image: /assets/images/frontend-hexagon.webp
class: flex flex-col items-center
---

# One for the front

<!--
(Sidenote, backend developers in my company are now pointed to my frontend architecture articles as part of the 
documentation for the backend, but is actually more to learn about the principles than the actual implementation.)

Hexagonal architecture is all about single responsibility and separation of concerns. As a matter of fact, most of the
issues I come across when write frontend code are just a function of elements not having clear responsibilities: store
state can hold anything, the stores actions can do whatever, the store getters can do what they please, you can do 
whatever you need in your components, all that matters is that it works. That's the mentality I first learned frontend 
in. 

So let's define responsibilities real quick, and then we can dive in a bit deeper into each one:
- Domain is what your application needs. All the classes, interfaces and __core domain__ complexity should be in the domain.
- Secondary is everything you consider external to your application: all your services, your APIs, etc .- Infra
- Primary is how your application implements the domain .- Application

Your domain doesn't care about how that data is obtained: you can swap your secondary layer without touching the domain.
For example, move from REST to GraphQL, move from a Pinia Store to IndexDB, etc.  
Your domain doesn't care about how that data is used: you can swap out your primary layer without touching the domain.
For example, move the web application to a command line interface, or to a voice interface like an Alexa.
-->


---
transition: slide-left
layout: two-cols
class: flex flex-col justify-center
---

# Directory structure

::right::
<img src="/assets/images/directories.webp" alt="Hexagon directories" class="w-fit">

<!--
This translates to this directory structure.

In your Vue app there's also going to be more things: namely components, composables, assets, router, and services.

But these are the ones will be focusing on.
-->


---
transition: slide-left
layout: two-cols
class: flex flex-col justify-center
---

# Domain

A look inside the domain:

::right::
<img src="/assets/images/directories.webp" alt="Hexagon directories" class="w-fit mb-4">
<img src="/assets/images/inside-domain.png" alt="Inside domain directory" class="w-fit">

<!--

-->


---
transition: fade
layout: full
---

# A Domain model example
Finally, some code

```ts {*|1-4|7-17|19-26|28-56}{lines:true, maxHeight:'555px'}
import type { IngredientProperties } from "@/hexagon/domain/ingredient/types";
import { Unit } from "@/hexagon/domain/ingredient/enums";
import { InvalidUnitForConversionException } from "@/hexagon/domain/ingredient/exceptions/InvalidUnitForConversionException";
import { OUNCE_IN_GRAMS } from "@/hexagon/domain/ingredient/constants";

export class Ingredient {
  private constructor(
    private readonly id: number,
    private readonly name: string,
    private quantity: number,
    private unit: Unit,
  ) {}

  static fromProperties(properties: IngredientProperties): Ingredient {
    const { id, name, quantity, unit } = properties;
    return new Ingredient(id, name, quantity, unit);
  }

  get properties(): IngredientProperties {
    return {
      id: this.id,
      name: this.name,
      quantity: this.quantity,
      unit: this.unit,
    };
  }

  changeGramsToOunces() {
    if (this.unit !== Unit.g) {
      throw new InvalidUnitForConversionException(
        this.unit,
        Unit.oz,
        this.changeGramsToOunces.name,
      );
    }

    const quantityInOunces = this.quantity / OUNCE_IN_GRAMS;
    this.unit = Unit.oz;
    this.quantity = quantityInOunces;
    return this;
  }

  changeOuncesToGrams() {
    if (this.unit !== Unit.oz) {
      throw new InvalidUnitForConversionException(
        this.unit,
        Unit.g,
        this.changeOuncesToGrams.name,
      );
    }

    const quantityInGrams = this.quantity * OUNCE_IN_GRAMS;
    this.unit = Unit.g;
    this.quantity = quantityInGrams;
    return this;
  }
}
```

<!--
Import your types, constants and enums.

The constructor is private, and the properties should be private and start as readonly, and only become editable when
necessary.

The way you instantiate the Domain is with that fromProperties method. And you access the properties via that properties
getter. The properties represent a snapshot of the current state of the domain model. All of these are measures in place
to protect the domain.

Hopefully that exception that the Domain method throws would teach the reader that you can't convert volume to mass.
-->

---
transition: slide-left
layout: image-left
image: /assets/images/hexagon-secondary.png
---

# Secondary
Implements the Domain Repository interface (that is its single responsibility)

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
