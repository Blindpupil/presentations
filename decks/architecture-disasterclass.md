---
title: Architecture Disasterclass
info: |
  ## What they don't tell you in frontend tutorials
layout: image
image: /assets/images/architecturebg.webp
class: text-center
transition: fade
---

<h1> <span v-mark="{ at: 2, color: 'red', type: 'strike-through', strokeWidth: 5 }">Vue</span> Architecture Disasterclass </h1>
What they don't tell you in frontend tutorials

<!--
In the previous presentation we talked about the importance of placing the Domain at the center when writing code.

Now I want to show you why the way we usually learn to write frontend code doesn't naturally leads you to this way of doing things.

[click] Even though I'll be talking about Vue and its ecosystem, these teachings apply to any frontend framework.
-->


---
transition: slide-left
layout: image
image: /assets/images/typical-frontend.webp
backgroundSize: contain
---


<!--
This is how a typical architecture design looks like.
Note there's a View layer, an API or Infrastructure layer, and everything in between.
-->


---
transition: slide-left
layout: image
image: /assets/images/responsibilities.png
backgroundSize: contain
---

### Exercise

<!--
(After coming back)
Really heard to implement single responsibility here.
That also makes the code hard to unit test because you can't easily pull your domain logic out of the component or the store.

Your business logic becomes tightly coupled with the store. As a matter of fact, in most apps "business logic" usually is means updating the state in some store.

Your components have way too many responsibilities.
The decision of whether a piece of logic lives in the component, a composable or in the store is made for the convenience of the developer working at that point in time that precise ticket. As a future reader, there's nothing to learn from seeing that code in the app.
-->


---
transition: slide-left
layout: image-left
image: https://cover.sli.dev
---

# Architecture Goals
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
Architecture goals will likely vary accross projects. Here are a couple that could apply to most.

1. Adding new features or refactors should not introduce regressions.
2. Your code provides value no matter how far ahead you look into the future. You should be able to grab your business logic out of your Vue application, and use it in a React application, or outside of the browser in a CLI tool, or with a voice interface, whatever. Removes vendor lock-in. 
3. This is the hardest part. New developers should be able to learn about the Domain, (or more accurately, the reality that your Domain is modeling) from reading your code. Likewise, Domain experts without coding expertise should be able to understand your code, because they already understand that reality.

(Go back to the exercise)
-->


---
transition: slide-left
layout: full
---

# Compounding components
Components have way too many responsibilities

```vue {*|30,33-36|39-44|49-69|80-91}{lines:true, maxHeight:'555px'}
<template>
  <div>
      <input v-model="term" type="text" class="search" placeholder="Search" />
  </div>

  <div class="grid" v-for="recipe in recipes" :key="recipe.id">
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
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from "vue";
import { Recipe } from "@/stores/recipe/Types";
import { useRecipeStore } from "@/stores/recipe";
import { EMOJIS } from "@/recipe/constants";

const recipes = ref<Recipe[]>([]); // What the component asks for
let original: Recipe[] = [];
onMounted(async () => {
  if (recipes.value.length) return // <== Caching strategy

  recipes.value = await recipeHttp.getRecipes(); // <== Fetching
  original = recipes.value;
});

const term = ref(""); // UI state coupled with Domain
watch(term, (newVal) => {
  recipes.value = original.filter((recipe) =>
    recipe.name.toLowerCase().includes(newVal.toLowerCase()),
  );
});

const store = useRecipeStore()
const allSelected = ref<string[]>([]); // UI state coupled with Domain
const selectedLast = computed(() => allSelected.value.at(-1));
async function select(recipeId: RecipeId) {
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
    store.addSelection(allSelected.value) // State side effect
    await recipeHttp.select(allSelected.value) // Infra side effect
  } else {
    allSelected.value = [recipeId];
    store.addSelection([recipeId]) // State side effect
    await recipeHttp.select([recipeId]) // Side effect
  }
}
function getIsSelected(recipeId: RecipeId): boolean {
  return allSelected.value.includes(recipeId);
}


const selected = ref(false);
function toggle() {
  selected.value = !selected.value;
}

const mealSizeIcon = computed(() => {
  switch (recipe.portions) {  // View state
    case 1:
      return EMOJIS.single;
    case 2:
      return EMOJIS.couple;
    case 3:
      return EMOJIS.oneKid;
    default:
      return EMOJIS.twoKids;
  }
});
</script>
```

<!--
You have probably seen components like this. This one shows a list of recipes. 
You have: 
1. Some properties that are being shown exactly as they come from the API
2. Some properties that are calculated in the component for this specific display
3. Some user interactions that produce side effects

[click] Here the component is deciding its caching strategy and making the request for what it needs. This shouldn't be here. We want our components to request what they need, and they shouldn't be concerned on whether the data comes from cache, store, api, sockets, LocalStorage, or wherever else. Lets remove this responsibility from here.
[click] Here we have some UI state which is binding a user interaction with the Domain. Again, this shouldn't happen. Ideally there should be a function that handles the user interaction and provides you with the result of that interaction. Here for example, your search functionality shouldn't care at all on whether is searching recipes, cars, meetups, whatever. Doing this with composables is now much easier.
[click] Here we have a user interaction that has side effects. Those side effects can be in the store, or in the infra. Again, the user interaction should be separate its side effects. Ideally your component should access a use case function (in this case, select or deselect recipe), and your component doesn't need to care whatever happens inside that function.
[click] Here we compute new properties required for the current View. This shouldn't be here either. Ideally the data your component receives should already contain everything it needs to be displayed properly.

In short, the only responsibilities your components should have are:
1. Display data it receives without needing to adapt it.
2. Handle user interactions (events) without needing to manage the consequences of those user interactions.

Data that your component receives that it doesn't need to adapt in any way are called **ViewObjects**. 
Functions that your components can call that handle the consequences of user interactions are calld **UseCases**. 
-->


---
transition: fade
layout: full
---

# State mismanagement

```ts {*}{lines:true, maxHeight:'555px'}
export const useRecipeStore = defineStore({
  id: "recipes",
  state: () => ({
    recipes: [] as Recipe[], // what comes from API
  }),
  getters: {
    componentRecipes() { // what the component needs
      return this.recipes.map(fromRecipesToView)
    }
  },
  actions: {
    async fetchRecipes() {
      const { data: recipes } = await axios.get<ApiRecipe[]>("/recipes");
      this.recipes = fromApiToRecipes(recipes);
    },
    async fetchComments() {
      const { data: comments } = await axios.get("/comments");
      return comments; // not even using the store
    },
  },
});
```

<!--
Here the state management library interacts with the API to fetch the data, and also adapts that data to whatever the application needs, and saves the result in the store.
The issue here is that the state management library is taking in more responsibility than it has to. The state management library should ideally only be used for state management. It shouldn’t be the store’s responsibility to run unrelated side-effects, format the data for components, make API calls, etc.

You have probably seen actions being used as containers for API calls, bypassing the store altogether. I sure have.

I have also seen cases where multiple components call actions on created or mounted without anyone ever checking to see if this data is already available in the store. If you open the network tab in these apps and use it, you'll find the same API calls with the same parameters being made over and over again.

All of these problems have, in my experience, a single root cause: a lack of defined responsibilities in the code.

Just like with people, when a piece of code has too many responsibilities at the same time, it’s bound to handle some of them poorly.
-->


---
transition: fade
layout: center
class: text-center
---

# The Solution 🥁 

<p v-click> To be discussed in the next Meetup! </p>


---
src: /pages/thank-you.md
---
