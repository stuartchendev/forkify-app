# 🍴 Forkify — Engineering-Focused JavaScript Project

A recipe search and management application built with **vanilla JavaScript**, focusing on **state-driven data flow**, **MVC architecture**, and **maintainable UI updates**.

This project is used as an **engineering showcase**, emphasizing *design decisions*, *trade-offs*, and *data flow clarity* rather than framework usage.

👉 **Live Demo**: [https://forkify-sturartchen.netlify.app/](https://forkify-sturartchen.netlify.app/)

---

> **Attribution & Scope**
>
> This project was originally based on the Forkify project from  
> “The Complete JavaScript Course” by Jonas Schmedtmann.
>
> I used the course as a baseline, then focused on:
> - restructuring the data flow
> - making design decisions explicit
> - handling edge cases beyond the course scope
>
> The goal of this repository is to demonstrate how I reason about frontend architecture and trade-offs.

---

## ✨ Why This Project Matters

Most recipe apps look similar on the surface.
What makes this project different is **how problems are modeled and solved**:

* Clear separation of concerns using **MVC**
* A centralized application **state** instead of scattered DOM state
* Explicit handling of **edge cases** in UI updates
* Decisions made with **stability and maintainability** in mind

This repository reflects how I approach frontend problems in a production-oriented way.

---

## 🧠 Beyond the Tutorial (Engineering Decisions)

Although this project started from a course tutorial, I went beyond the baseline by making several explicit engineering decisions around data flow, responsibility boundaries, and UI stability.

Key areas I extended beyond the tutorial include:

- **Centralized state ownership in the Model (MVC contract)**

    The Model owns all application state (recipe, search, bookmarks, filters), while the Controller coordinates user intent and view updates.
    This makes the data flow predictable and easier to debug without relying on implicit DOM state.

- **Page-based data enrichment before transformation**

    Because the search API does not provide all required fields, I introduced a page-based enrichment step to fetch additional data only for the visible results.
    After enrichment, sorting and filtering are treated as pure transformations on the enriched data.

- **Update vs render strategy based on DOM compatibility**
   
    The lightweight update() strategy assumes a compatible DOM node list (e.g. sorting).
    When filtering breaks this assumption by changing list length, I intentionally fall back to guarded updates or re-render behavior to keep the UI stable without overengineering.
---

## 🧠 Core Engineering Highlights

* **MVC Architecture**

    * Model manages application state, API calls, and data normalization
    * View layer focuses purely on rendering and DOM updates
    * Controller orchestrates user interactions and data flow

* **State-Driven Data Flow**

    * Single source of truth for recipes, search results, bookmarks, and UI state
    * UI is derived from state instead of imperative DOM manipulation

* **Predictable UI Updates**

    * Reusable base `View` class with `render()` and `update()` methods
    * Designed to minimize unnecessary re-renders while keeping logic readable

* **Intentional Scope Control**

    * No framework abstractions added
    * Complexity kept explicit to make data flow and decisions transparent

---

## 🧩 Features

* Search recipes from a public API
* View recipe details and ingredients
* Update servings with automatic ingredient recalculation
* Paginated search results
* Bookmark recipes (persisted via `localStorage`)
* Upload custom recipes
* Sort search results **after async resolution** (edge-case handled explicitly)

---

## 🏗 Architecture Overview (MVC)

### Model

* API communication and async data fetching
* Centralized application state
* Bookmark persistence and sorting logic

### View

* Dedicated view classes per UI responsibility
* Shared base `View` class for rendering, updating, and error handling
* DOM updates optimized for predictable behavior

### Controller

* Connects user actions to state changes
* Coordinates between Model and View
* Keeps side effects isolated from rendering logic

---

## 📂 Project Structure

```
Guide-forkify/
 ├── controller.js        # Orchestrates Model <-> View
 ├── model.js             # State management + API logic
 ├── config.js            # API endpoints and configuration
 ├── views/
 │    ├── View.js         # Base view abstraction
 │    ├── recipeView.js
 │    ├── resultsView.js
 │    ├── paginationView.js
 │    ├── bookmarksView.js
 │    ├── searchView.js
 │    ├── sortView.js
 │    └── addRecipeView.js
 ├── index.html
 └── ...
```

---

## 🧪 Engineering Note — Update vs Render (Filter / Sort Edge Case)

While implementing search result filtering and sorting, I encountered an edge case related to the `update()` method.

The `update()` logic assumes a **stable DOM structure** and performs a diff based on index comparison.
This works well for **sorting**, where elements are reordered but not removed.

However, **filtering changes the DOM structure**, causing some existing nodes to no longer exist.

To prevent unsafe DOM mutations, I added a guard condition:

```js
if (curEl && !newEl.isEqualNode(curEl)) {
  curEl.setAttribute(attr.name, attr.value);
}
```

This ensures that:

* `update()` safely becomes a no-op when DOM structure is incompatible
* Filtering does not introduce runtime errors
* Sorting behavior remains predictable

**Trade-off**:
A key-based diff or centralized UI state could handle this more generally, but was intentionally avoided to keep the project focused and explicit.

---

## 🚀 Getting Started

```bash
git clone <repo-url>
cd forkify-app
npm install
npm run dev
```

---

## 🌐 Deployment

* Deployed as a client-side JavaScript application on **Netlify**
* No server-side configuration required
* Automatic deployment via GitHub integration

---

## 📌 Why I Keep This Project Vanilla

This project intentionally avoids frameworks to make **data flow, state responsibility, and UI updates explicit**.

The goal is not to build faster —
but to build in a way that is **easy to reason about, debug, and explain**.

