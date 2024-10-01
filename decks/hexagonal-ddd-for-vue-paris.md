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

<div class="flex mt-12">
  <section class="mr-8">
    <h3 class="italic">For the organization</h3>
    <ul class="text-left">
      <p>- Domain expertise is made available</p>
      <p>- Develop an Ubiquitous Language</p>
    </ul>
  </section>

  <section>
    <h3 class="italic">For the developer</h3>
    <ul class="text-left">
      <p>- Code should teach readers about the domain</p>
      <p>- Use domain language in the code</p>
    </ul>
  </section>
</div>

<!--
The tagline for Eric Evans book is "Tackling complexity in the heart of software". The solution can be resumed with: 
"Put Domain at the centre of your focus". This idea has implications not only for the developer, but for the entire
organization. DDD provides a set of techniques to facilitate the implementation of these ideas.

Matter of fact, almost all of your common activities as a developer change in a team that takes this seriously: code
reviews are a big one, but also sprint planning, retros and even the way you do your daily standup changes. Agile (or
any other iterative development process) is actually core to DDD. 

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
Not as pretty, it's still a WIP 

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

# Domain example: Domain Model
Finally, some code

```ts {*|1-4|7-17|19-26|28-56}{lines:true, maxHeight:'555px'}
import type { IngredientProperties } from "@/domain/ingredient/types";
import { Unit } from "@/domain/ingredient/enums";
import { InvalidUnitForConversionException } from "@/domain/ingredient/exceptions/InvalidUnitForConversionException";
import { OUNCE_IN_GRAMS } from "@/domain/ingredient/constants";

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
[click] Import your types, constants and enums.

[click] The constructor is private, and the properties should be private and start as readonly, and only become editable when
necessary.

[click] The way you instantiate the Domain is with that fromProperties method. And you access the properties via that properties
getter. The properties represent a snapshot of the current state of the domain model. All of these are measures in place
to protect the domain.

[click] Hopefully that exception that the Domain method throws would teach the reader that you can't convert volume to mass.

Needing to decide what to put here is the main reason why you spend so much more time discussing with the other team 
members. (This reminds me of an anecdote that exemplifies what I said earlier about how the way you conduct your usual 
agile meetings change when you internalize these practice. I was in a sprint planning doing scrum poker to define the
complexity of a ticket: the task was to change the way a certain type of input behaves. It turns out that those specific
type of inputs can't possibly do one of the actions that all other type of inputs do. The other team members gave that
ticket a very low score, but I gave it a higher one. And when they asked me why I said it's because everyone thinks that
the task is to remove that capability from those inputs, but the task is actually finding a way to make sure that his 
knowledge is made obvious in the code for everyone else reading it in the future. That knowledge should be part of the 
domain model.)
-->


---
transition: fade
layout: full
---

# Domain example: Repository

```ts {*|2,7-10}{lines:true, maxHeight:'555px'}
import type { RecipeId, RecipeToSave } from "@/domain/recipe/types";
import type { Recipe } from "@/domain/recipe/Recipe";
import type { UserId } from "@/domain/user/types";
import type { RecipeProperties } from "@/domain/recipe/types";

export interface RecipeRepository {
  getRecipes(userId: UserId): Promise<Recipe[]>;
  getFavoriteRecipes(userId: UserId): Promise<Recipe[]>;
  createRecipe(userId: UserId, form: RecipeToSave): Promise<Recipe>;
  updateRecipe(form: RecipeProperties): Promise<Recipe>;
  deleteRecipe(recipeId: RecipeId): Promise<void>;
}

```

<!--
The Repository is far simpler. It's the contract Secondary needs to abide by.
This is what makes your domain portable: as long as the contract is followed, your domain always should work.

[click] The important thing to note here is the type of those returned objects: Repositories always return domain models.
-->


---
transition: slide-left
layout: image-left
image: /assets/images/hexagon-secondary.png
class: flex flex-col justify-center
---

# Secondary
Implements the Domain Repository interface (that is its single responsibility)

It does so via the **Resource**

<!--
Secondary definition:
It's your infrastructure layer. Everything external to your application is secondary. All the data sources are secondary: 
local storage, IndexDB, and even your Store (Pinia, Vuex, Redux, whatever) could be considered secondary. If you don't 
have control over what's coming in, it's secondary. If you decide it's secondary, it's secondary.  

Repository Interface definition:
There's a reason why Domain is at the center. The repository in
the Domain is a list of methods that either return Domain Objects, or do stuff that the Domain needs to have done. Your
domain objects are your domain classes or interfaces.
[The domain is like a bad boss that tells everyone what they should do, but does nothing itself, unless there's real money
to be made, then that's where it shows it cares]
-->


---
transition: fade
layout: full
---

# Secondary: Resource

```ts {*|10|11|13-15|19-21|23-25}{lines:true, maxHeight:'555px'}
import type { RecipeRepository } from "@/domain/recipe/RecipeRepository";
import type { UserId } from "@/domain/user/types";
import type { RecipeId, RecipeToSave } from "@/domain/recipe/types";
import type { RecipeProperties } from "@/domain/recipe/types";
import { Recipe } from "@/domain/recipe/Recipe";

import type { RecipeHttp } from "@/secondary/recipe/RecipeHttp";
import { useRecipeStore } from "@/secondary/recipe/RecipeStore";

export class RecipeResource implements RecipeRepository {
  constructor(private readonly recipeHttp: RecipeHttp) {}

  get store() {
    return useRecipeStore();
  }

  async getRecipes(): Promise<Recipe[]> {
    const recipesInStore = this.store.recipes;
    if (recipesInStore.length) {
      return recipesInStore.map(Recipe.fromProperties);
    }

    const recipes = await this.recipeHttp.getRecipes();
    this.store.saveRecipes(recipes.map((recipe) => recipe.properties));
    return recipes;
  }

  async getFavoriteRecipes(userId: UserId): Promise<Recipe[]> {
    return await this.recipeHttp.getFavoriteRecipes(userId);
  }

  async createRecipe(userId: UserId, form: RecipeToSave): Promise<Recipe> {
    const recipe = await this.recipeHttp.createRecipe(userId, form);
    this.store.addRecipe(recipe.properties);
    return recipe;
  }

  async updateRecipe(form: RecipeProperties): Promise<Recipe> {
    const recipe = await this.recipeHttp.patchRecipe(form);
    this.store.updateRecipe(recipe.properties);
    return recipe;
  }

  async deleteRecipe(recipeId: RecipeId): Promise<void> {
    await this.recipeHttp.deleteRecipe(recipeId);
    this.store.removeRecipe(recipeId);
  }
}
```

<!--
The most important line of this file is this one: **implements RecipeRepository**, the fact that it implements the domain
repository. Everything else is just implementation details:

[click] Here there's a http client to access some API data. You can imagine here you can have as many clients as you can
possibly need. 

[click] I consider the store secondary here. It's saving API properties, and it serves as a local cache. 

[click] Here is where the caching strategy is defined. Away from the store action, and more importantly, away from the 
component. 

[click] Finally, here you can see that it saves properties in the store, but it returns the Domain object, as required
by the contract.  
-->


---
transition: slide-left
layout: image-right
image: /assets/images/hexagon-primary.png
---

# Primary
We have our domain models, created from the data provided by secondary. How to use it? 

- Primary uses the domain to provide the specific **use cases** for your application
- Primary can never expose domain objects, it always returns **View Objects** (also called **Views**)

<!--
Your application is implemented in Primary. The objects and data structures that your primary produces are to be adapted
to, in the case of web developers, the browser context (mostly). Domain defines what it needs, but Primary exposes the
data that is useful to how I'm going to present it in the browser, and how the users will interact with it.
-->


---
transition: fade
layout: full
---

# Primary: Use Cases

```ts {*|12|7-10,13-14|16-21}{lines:true, maxHeight:'555px'}
import type { RecipeRepository } from "@/domain/recipe/RecipeRepository";
import type { UserRepository } from "@/domain/user/repository/UserRepository";

import { RecipeView } from "@/primary/recipe/RecipeView";

export class GetRecipesUseCase {
  constructor(
    private readonly recipeRepository: RecipeRepository,
    private readonly userRepository: UserRepository,
  ) {}

  async execute(): Promise<RecipeView[]> {
    const user = this.userRepository.getCurrentUser();
    const recipes = await this.recipeRepository.getRecipes(user.properties.id);

    return recipes.map((recipe) =>
      RecipeView.fromProperties(
        recipe.properties,
        recipe.properties.ingredients,
      ),
    );
  }
}
```

<!--
A use case is something your application needs to do. 

[click] It consists of a class with a single public method "execute". 

[click] Check it out though, in the constructor there's something you've already seen. It's the Repository defined by
the domain. It's NOT the Resource implementation (that's the D of SOLID, dependency inversion: depend upon abstractions,
the Domain abstraction in this case, and not the concrete implementation of the Secondary Resource). Use cases can use
as many Repositories as it may need to accomplish its goal. 

[click] Also important note here that the type of those returned objects are Views. The domain model being used is never
exposed. 
-->


---
transition: fade
layout: full
---

# Primary: Views

```ts {*|9-16|18|20|33|37-39|46-57}{lines:true, maxHeight:'555px'}
import type { RecipeId, RecipeProperties } from "@/domain/recipe/types";
import type { IngredientProperties } from "@/domain/ingredient/types";

import { IngredientView } from "@/primary/ingredient/IngredientView";
import { EMOJIS } from "@/primary/recipe/constants";
import { recipeService } from '@/services/recipe'

export class RecipeView {
  private constructor(
    public readonly id: RecipeId,
    public name: string,
    public readonly ingredients: IngredientView[],
    public readonly instructions: string,
    public portions: number,
    public readonly updatedAt: string,
  ) {}

  public isSelected = false;

  static fromProperties(
    recipe: RecipeProperties,
    ingredients: IngredientProperties[],
  ) {
    const { id, name, instructions, portions, updatedAt } = recipe;
    const ingredientViews = ingredients.map(IngredientView.fromProperties);

    return new RecipeView(
      id,
      name,
      ingredientViews,
      instructions,
      portions,
      updatedAt.toLocaleDateString(),
    );
  }
  
  set name(value: string) {     // ==> <input v-model="recipeView.name />"
    recipeService.rename(value)
  }

  toggle() {
    this.isSelected = !this.isSelected;
    return this;
  }

  get mealSizeIcon() {
    switch (this.portions) {
      case 1:
        return EMOJIS.single;
      case 2:
        return EMOJIS.couple;
      case 3:
        return EMOJIS.oneKid;
      default:
        return EMOJIS.twoKids;
    }
  }
}
```

<!--
Views is what your UI uses. 

[click] Properties not as protected as the Domain. There are no generals rules for the properties. You do what fits you. 

[click] For example, you could have properties in the View that hold their application state.  

[click] If possible, it pays to maintain the same instantiation process across the architecture.

[click] While your domain may expect a Data instance here, your View is free to expose how that date is expected to be
presented in the UI. This can allow you, for example, to have a consistent way of displaying certain date/times across
your models. 

[click] I came up with this next trick, for the cases where it fits, it's just beautiful. This setter is called when a 
v-model changes, which will trigger the use case, which will in turn update the UI. This allows for the cleanest 
components I have ever seen in my career.

[click] Finally, as an example I also show this getter that is supposed to give the UI the correct icon based on the 
recipe portion size. I commonly get the question: why isn't that just another component, a meal-size-icon that takes the
portion as a prop? To that I say, sure, that's upto you whether that makes more sense for your project. 
As a matter of fact, many of the things here in the Views could be made into smaller and smaller components. 
However, those components also become more and more bound to that specific domain.
-->


---
transition: fade
layout: full
---

# Primary: Services (use-case indexes)

```ts {*|8-23|25-31}{lines:true, maxHeight:'555px'}
// @/primary/Recipe/use-cases/index.ts
import type { RecipeRepository } from "@/hexagon/domain/recipe/RecipeRepository";
import type { RecipeToSave } from "@/hexagon/domain/recipe/types";
import type { UserRepository } from "@/hexagon/domain/user/repository/UserRepository";
import { CreateRecipeUseCase } from "@/hexagon/primary/recipe/use-cases/CreateRecipeUseCase";
import { GetRecipesUseCase } from "@/hexagon/primary/recipe/use-cases/GetRecipesUseCase";

export class RecipeService {
  private getRecipesUseCase: GetRecipesUseCase;
  private createRecipeUseCase: CreateRecipeUseCase;

  constructor(
    private readonly recipeRepository: RecipeRepository,
    private readonly userRepository: UserRepository,
  ) {
    this.getRecipesUseCase = new GetRecipesUseCase(
      recipeRepository,
      userRepository,
    );
    this.createRecipeUseCase = new CreateRecipeUseCase(
      recipeRepository,
      userRepository,
    );
  }

  async getRecipes() {
    return await this.getRecipesUseCase.execute();
  }

  async createRecipe(form: RecipeToSave) {
    return await this.createRecipeUseCase.execute(form);
  }
}
```

<!--
Don't worry. Your components are not expected to import the use cases one by one. There's this neat thing called Service
that indexes all the use cases for you. A service all it does is grab all the use cases from a domain and expose their
"execute" methods. 

[click] The service is constructed with all the Domain Repositories (again, not the Secondary Resources) needed by the 
use cases and passes them onward to them.
-->


---
transition: fade
layout: full
---

# Providing service instances

```ts {*|9,15-19,28}{lines:true, maxHeight:'555px'}
// main.ts
import { type InjectionKey, createApp } from "vue";
import { createPinia } from "pinia";
import App from "./App.vue";
import router from "./app-router";

// Services
import { UserResource } from "@/hexagon/secondary/user/UserResource";
const userResource = new UserResource();

import { RecipeResource } from "@/hexagon/secondary/recipe/RecipeResource";
import { RecipeService } from "@/hexagon/primary/recipe/use-cases";
import { restClient } from "@/hexagon/secondary/RestClient";
import { RecipeHttp } from "@/hexagon/secondary/recipe/RecipeHttp";
const recipeHttp = new RecipeHttp(restClient);
const recipeResource = new RecipeResource(recipeHttp);

const recipeService = new RecipeService(recipeResource, userResource);
const RecipeServiceKey = Symbol() as InjectionKey<RecipeService>;


// Setup
const app = createApp(App);
const pinia = createPinia();
app.use(pinia);

app.use(router);
app.provide(RecipeServiceKey, recipeService);

app.mount("#app");
```

<!--
Finally, all you need to do is provide the service instances wherever in the component tree they may be needed. In this
case they are provided in the root.
-->



---
transition: fade
layout: full
---

# Providing service instances

```ts {*|9,15-19,28}{lines:true, maxHeight:'555px'}
// main.ts
import { type InjectionKey, createApp } from "vue";
import { createPinia } from "pinia";
import App from "./App.vue";
import router from "./app-router";

// Services
import { UserResource } from "@/hexagon/secondary/user/UserResource";
const userResource = new UserResource();

import { RecipeResource } from "@/hexagon/secondary/recipe/RecipeResource";
import { RecipeService } from "@/hexagon/primary/recipe/use-cases";
import { restClient } from "@/hexagon/secondary/RestClient";
import { RecipeHttp } from "@/hexagon/secondary/recipe/RecipeHttp";
const recipeHttp = new RecipeHttp(restClient);
const recipeResource = new RecipeResource(recipeHttp);

const recipeService = new RecipeService(recipeResource, userResource);
const RecipeServiceKey = Symbol() as InjectionKey<RecipeService>;


// Setup
const app = createApp(App);
const pinia = createPinia();
app.use(pinia);

app.use(router);
app.provide(RecipeServiceKey, recipeService);

app.mount("#app");
```

<!--
Finally, all you need to do is provide the service instances wherever in the component tree they may be needed. In this
case they are provided in the root.
-->


---
transition: fade
layout: center
class: flex flex-col justify-center items-center
---

# ...And the components?

<img src="/assets/images/drumroll.gif"  />


---
transition: fade
layout: center
---

They look like this:

```vue {*}{lines:true, maxHeight:'555px'}
<!-- RecipesPage -->
<template>
  <h1>Recipes</h1>

  <div class="heading">
    <div>
      <input v-model="term" type="text" class="search" placeholder="Search" />
    </div>
    <button class="clear" :disabled="!isClearable" @click="clear()">
      Clear
    </button>
    <label>
      Multiselect
      <input v-model="isMultiselectable" type="checkbox" />
    </label>
  </div>

  <div class="grid" v-if="recipes.length">
    <RecipeCard_composition
      v-for="recipe in recipes"
      :is-selected="getIsSelected(recipe.id)"
      :recipe="recipe"
      :key="recipe.id"
      class="recipe"
      @click="select(recipe.id)"
    />
  </div>

  <div v-else>
    <p>No recipes found.</p>
    <button>Create your first!</button>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from "vue";
import RecipeCard_composition from "@/classic/components/recipe/RecipeCard_composition.vue";
import { type Recipe, recipeHttp } from "@/http/recipeHttp";
import { useRecipeStore } from "@/stores/recipeStore";

const recipeStore = useRecipeStore();
const recipes = ref<Recipe[]>([]); // What the component asks for
let original: Recipe[] = [];
onMounted(async () => {
  if (recipes.value.length) return // <== Caching strategy

  recipes.value = await recipeHttp.getRecipes(); // <== Fetching
  original = recipes.value;
});

const term = ref("");    // UI state coupled with domain
watch(term, (newVal) => {
  recipes.value = recipeStore.searchByName(newVal);
});

const allSelected = ref<number[]>([]);  // UI state coupled with Domain
const selectedLast = computed(() => allSelected.value.at(-1));
async function select(recipeId: number) {
  const isAlreadySelected = Boolean(
    allSelected.value.find((id) => recipeId === id),
  );

  if (isAlreadySelected) {
    allSelected.value = allSelected.value.filter((id) => recipeId !== id);
    await recipeHttp.deselect(allSelected.value) // Side effect
    return;
  }

  if (isMultiselectable.value) {
    allSelected.value.push(recipeId);
    recipeStore.addSelection(allSelected.value) // State side effect
    await recipeHttp.select(allSelected.value) // Infra side effect
  } else {
    allSelected.value = [recipeId];
    recipeStore.addSelection([recipeId]) // State side effect
    await recipeHttp.select([recipeId]) // Side effect
  }
}
function getIsSelected(recipeId: number): boolean {
  return allSelected.value.includes(recipeId);
}

const isClearable = computed(() => Boolean(allSelected.value.length));
function clear() {
  allSelected.value = [];
}

const isMultiselectable = ref(false);
watch(isMultiselectable, (newValue) => {
  if (newValue || !selectedLast.value) return;
  const last = selectedLast.value;

  clear();
  select(last);
});
</script>
```


---
transition: fade
layout: center
---

Sorry! My bad. Like this:

```vue {*}{lines:true, maxHeight:'555px'}
<template>
  <h1>Recipes</h1>

  <div class="heading">
    <div v-if="collection">
      <input v-model="term" type="text" class="search" placeholder="Search" />
    </div>
    <button
      class="clear"
      :disabled="!collection.isClearable"
      @click="collection.clear()"
    >
      Clear
    </button>
    <label>
      Multiselect
      <input v-model="collection.isMultiselectable" type="checkbox" />
    </label>
  </div>

  <div class="grid" v-if="collection?.items?.length">
    <RecipeCard
      v-for="recipe in collection.items"
      class="recipe"
      :recipe="recipe"
      :key="recipe.id"
      @click="collection.select(recipe.id)"
    />
  </div>

  <div v-else>
    <p>No recipes found.</p>
    <button>Create it!</button>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import RecipeCard from "@/ui/RecipeCard.vue";
import { useRecipeCollection } from "@/composables/useRecipeCollection";

const collection = await useRecipeCollection();

const term = ref("");
watch(term, (newVal) => collection.value.searchByName(newVal));
</script>
```


---
transition: fade
layout: two-cols
class: flex
---

```vue {*}{lines:true, maxHeight:'555px'}
<template>
  <div :class="{ selected: isSelected }" @click="toggle">
    <h2 class="header">{{ recipe.name }}</h2>
    <sub>
      Serves {{ recipe.portions }} <span>{{ mealSizeIcon }}</span>
    </sub>

    <ul>
      <li v-for="ingredient in recipe.ingredients" :key="ingredient.id">
        {{ ingredient.quantity }} {{ ingredient.name }} {{ ingredient.unit }}
      </li>
    </ul>

    <p>{{ recipe.instructions }}</p>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import type { Recipe } from "@/http/recipeHttp";

const props = defineProps<{ recipe: Recipe; isSelected: boolean }>();

const selected = ref(false);
function toggle() {
  selected.value = !selected.value;
}

const mealSizeIcon = computed(() => {
  switch (props.recipe.portions) {
    case 1:
      return "🧍";
    case 2:
      return "👫";
    case 3:
      return "👪";
    default:
      return "👨‍👩‍👧‍👦";
  }
});
</script>
```

::right::

```vue {*}{lines:true, maxHeight:'555px'}
<!-- RecipeCard.vue -->
<template>
  <div :class="{ selected: recipe.isSelected }">
    <h2 class="header">{{ recipe.name }}</h2>
    <sub>
      Serves {{ recipe.portions }} <span>{{ recipe.mealSizeIcon }}</span>
    </sub>

    <ul>
      <li v-for="ingredient in recipe.ingredients" :key="ingredient.id">
        {{ ingredient.quantity }} {{ ingredient.name }} {{ ingredient.unit }}
      </li>
    </ul>

    <p>{{ recipe.instructions }}</p>
  </div>
</template>

<script setup lang="ts">
import type { RecipeView } from "@/primary/recipe/RecipeView";

defineProps<{
  recipe: RecipeView;
}>();
</script>
```


---
transition: fade
layout: image-left
image: /assets/images/danger-meme.jpg
---

# ⚠️

- Hexagonal DDD is not for every project
- Hexagonal DDD has a cost
- Learn the rules so you know when to break them

Also:
- Tests are required at every step of the way

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
