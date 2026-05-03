
---




## 🔎 Search + Category Filter (home.ejs Template)

### Search Form
```html
<!-- Search -->
<form action="/" method="get" class="flex items-center gap-2">
  <% if (selectedCategory && selectedCategory !== 'all') { %>
    <input type="hidden" name="category" value="<%= selectedCategory %>">
  <% } %>

  <div class="relative">
    <i class="fa-solid fa-magnifying-glass absolute left-2.5 top-1/2 -translate-y-1/2 text-[10px]" style="color:#4b4b52;"></i>

    <input
      type="text"
      name="search"
      placeholder="search..."
      value="<%= typeof searchQuery !== 'undefined' ? searchQuery : '' %>"
      class="text-xs pl-7 pr-3 py-1.5 rounded-lg w-48"
      style="font-family: inherit;"
    >
  </div>
</form>

<!-- Category filter pills -->
<div class="flex items-center gap-1.5 mb-6 overflow-x-auto pb-1" style="scrollbar-width: none;">
  <% const categories = ['all','coding','education','resource','entertainment','news','other']; %>

  <% categories.forEach(function(cat) { %>
    <a
      href="/?category=<%= cat %><%= typeof searchQuery !== 'undefined' && searchQuery ? '&search=' + searchQuery : '' %>"
      class="filter-btn <%= selectedCategory === cat ? 'active' : '' %>"
    >
      <%= cat %>
    </a>
  <% }); %>
</div>

```

---
Good — this is one of the **most important backend + frontend connection concepts** in Express + EJS apps.

I’ll explain it very deeply but in **very simple language**, like you are building a real app step-by-step.

---

# 🧠 BIG IDEA (MOST IMPORTANT)

You are building this behavior:

👉 User can:

- filter bookmarks by **category**
    
- search bookmarks by **title**
    
- and combine both together
    

Example:

```
/?category=coding&search=react
```

👉 Meaning:

- only coding bookmarks
    
- whose title contains “react”
    

---

# 🧩 STEP 1: WHERE DATA COMES FROM

When user visits:

```
http://localhost:3000/?category=coding&search=react
```

👉 Express receives this:

```js
req.query = {
  category: "coding",
  search: "react"
}
```

---

# 🧠 QUERY = URL DATA

|URL part|Meaning|
|---|---|
|`?category=coding`|filter type|
|`&search=react`|search text|

---

# 🧩 STEP 2: BACKEND LOGIC (VERY IMPORTANT)

```js
const selectedCategory = req.query.category || "all";
const search = req.query.search || "";
```

## 🧠 Meaning:

### 1. category

If user did not select anything:

```js
selectedCategory = "all"
```

👉 default = show everything

---

### 2. search

If user did not type anything:

```js
search = ""
```

👉 empty means no search filter

---

# 🧩 STEP 3: BUILDING FILTER OBJECT

```js
let filter = {};
```

👉 This is your “rule box” for MongoDB.

At first:

```
{}
```

means:  
👉 “no filter → show everything”

---

# 🧩 STEP 4: CATEGORY FILTER

```js
if (selectedCategory !== "all") {
  filter.category = selectedCategory;
}
```

---

## 🧠 Example 1:

User selects:

```
coding
```

Filter becomes:

```js
{
  category: "coding"
}
```

👉 MongoDB meaning:

> “only coding bookmarks”

---

## 🧠 Example 2:

User selects:

```
all
```

Filter:

```js
{}
```

👉 meaning:

> show everything

---

# 🧩 STEP 5: SEARCH FILTER (IMPORTANT)

```js
if (search) {
  filter.title = {
    $regex: search,
    $options: "i"
  };
}
```

---

## 🧠 SIMPLE MEANING:

👉 “Find bookmarks whose title contains this text”

---

## 🔍 Example:

User types:

```
react
```

MongoDB query becomes:

```js
{
  title: {
    $regex: "react",
    $options: "i"
  }
}
```

---

## 🧠 WHAT IS REGEX?

Think like this:

|Search|Matches|
|---|---|
|react|React basics|
|react|learn react js|
|react|REACT tutorial|

---

### `$options: "i"`

👉 means:

> ignore uppercase/lowercase

So:

```
React = react = REACT
```

---

# 🧩 STEP 6: FINAL FILTER COMBINATION

Now BOTH filters combine:

## Example:

User:

```
category=coding
search=react
```

Final filter:

```js
{
  category: "coding",
  title: {
    $regex: "react",
    $options: "i"
  }
}
```

---

## 🧠 Meaning in English:

👉 “Give me coding bookmarks whose title contains react”

---

# 🧩 STEP 7: DATABASE QUERY

```js
const bookmarks = await Bookmark.find(filter);
```

👉 MongoDB reads filter and returns matching data

---

# 🧩 STEP 8: SEND DATA TO EJS

```js
res.render("home", {
  bookmarks,
  selectedCategory,
  searchQuery: search
});
```

---

## 🧠 WHY WE SEND THESE?

Because frontend needs:

|Variable|Purpose|
|---|---|
|bookmarks|show results|
|selectedCategory|highlight button|
|searchQuery|keep text in input|

---

# 🎨 FRONTEND EXPLANATION

Now EJS uses backend data.

---
Good — now we’ll focus only on the **frontend (EJS + HTML)** part and understand it like a system, not just code.

I’ll break it into **small mental pieces**, so you understand what each line is _actually doing in your app logic_.

---

# 🧠 BIG PICTURE (FRONTEND ROLE)

Your frontend is doing only 3 jobs:

### 1. 🧾 Take user input

- search text
    
- category click
    

### 2. 🔁 Send that input to backend

- using URL (`/?category=...&search=...`)
    

### 3. 🎨 Show backend data back to user

- bookmarks list
    
- active category highlight
    
- search text inside input
    

---

# 🧩 PART 1: SEARCH FORM (VERY IMPORTANT)

```html
<form action="/" method="get">
```

## 🧠 Meaning:

👉 “When user submits form, send data to `/` using URL”

So it becomes:

```
/?search=react
```

or

```
/?category=coding&search=react
```

---

## 🧠 Why GET method?

Because:

- data goes in URL
    
- easy to share
    
- page reload is expected
    

---

# 🧩 PART 2: HIDDEN INPUT (MOST IMPORTANT CONCEPT)

```ejs
<% if (selectedCategory && selectedCategory !== 'all') { %>
  <input type="hidden" name="category" value="<%= selectedCategory %>">
<% } %>
```

---

## 🧠 SIMPLE MEANING:

👉 “Don’t lose selected category when user searches”

---

## 🔥 REAL LIFE PROBLEM WITHOUT THIS:

User flow:

1. selects category:
    

```
/?category=coding
```

2. types search:
    

```
react
```

👉 Without hidden input:

```
/?search=react
```

❌ category LOST

---

## ✔ WITH hidden input:

```
/?category=coding&search=react
```

✔ category preserved

---

## 🧠 THINK LIKE THIS:

Hidden input = “silent memory keeper”

It sends extra data automatically.

---

# 🧩 PART 3: SEARCH INPUT BOX

```html
<input
  type="text"
  name="search"
```

---

## 🧠 IMPORTANT RULE:

👉 `name="search"` is what creates URL key

---

## Example:

User types:

```
react
```

Browser sends:

```
?search=react
```

---

## 🧠 THIS LINE:

```ejs
value="<%= typeof searchQuery !== 'undefined' ? searchQuery : '' %>"
```

---

## SIMPLE MEANING:

👉 “If search already exists, show it inside input box”

---

## WHY THIS IS IMPORTANT?

Without this:

User searches:

```
react
```

Page reloads → input becomes empty ❌

---

With this:

```
react stays visible ✔
```

---

## 🧠 REAL LIFE ANALOGY:

Like Google search:

👉 You search “react”  
👉 Page reloads  
👉 input still shows “react”

---

# 🧩 PART 4: CATEGORY FILTER LINKS

```ejs
<a href="/?category=coding&search=react">
```

---

## 🧠 WHAT IS THIS DOING?

👉 This is NOT form — this is direct URL navigation

When user clicks:

```
coding
```

Browser goes to:

```
/?category=coding
```

---

## 🧠 BUT IMPORTANT PART:

```ejs
<%= typeof searchQuery !== 'undefined' && searchQuery ? '&search=' + searchQuery : '' %>
```

---

## SIMPLE MEANING:

👉 “If search exists, don’t lose it when category changes”

---

## EXAMPLE:

User:

```
search = react
category = coding
```

Click “news”

URL becomes:

```
/?category=news&search=react
```

✔ search preserved  
✔ category changed

---

# 🧠 WHY THIS IS NEEDED?

Because without it:

👉 every category click would delete search

---

# 🧩 PART 5: LOOPING CATEGORIES

```ejs
<% categories.forEach(function(cat) { %>
```

---

## 🧠 SIMPLE MEANING:

👉 “Repeat HTML for each category”

So this:

```js
['all','coding','education']
```

becomes:

```html
All
Coding
Education
```

---

# 🧩 PART 6: ACTIVE CATEGORY HIGHLIGHT

```ejs
class="filter-btn <%= selectedCategory === cat ? 'active' : '' %>"
```

---

## 🧠 MEANING:

👉 If this category is selected → add special styling

---

## EXAMPLE:

If:

```
selectedCategory = "coding"
```

Then:

```html
Coding button gets "active"
```

---

## 🧠 WHY IMPORTANT?

It tells user:

👉 “You are currently viewing coding bookmarks”

---

# 🧩 PART 7: COMPLETE USER FLOW (VERY IMPORTANT)

Let’s combine everything:

---

## STEP 1: User opens page

```
/?category=all
```

Backend sends:

```js
selectedCategory = "all"
searchQuery = ""
```

Frontend shows:

- all bookmarks
    
- empty search box
    

---

## STEP 2: User types search

```
react
```

Frontend sends:

```
/?search=react
```

Backend filters results

Frontend shows:

- filtered bookmarks
    
- input still shows "react"
    

---

## STEP 3: User clicks category

```
coding
```

URL becomes:

```
/?category=coding&search=react
```

Frontend shows:

- coding bookmarks only
    
- search still visible
    
- coding button highlighted
    

---

# 🧠 FINAL SIMPLE MIND MODEL

Think frontend like this:

```
USER ACTION → URL UPDATE → BACKEND → DATA → EJS DISPLAY
```

---

# 🧾 SUPER SIMPLE SUMMARY

### SEARCH BOX:

- sends `search` in URL
    
- keeps value after reload
    

### HIDDEN INPUT:

- keeps category alive while searching
    

### CATEGORY LINKS:

- change category
    
- but preserve search
    

### EJS LOGIC:

- decides UI state (active button, input value)
    

---

# 🧠 ONE LINE MEMORY NOTE

```js
/**
 * Frontend = URL builder + UI state renderer
 * search input → creates ?search=
 * hidden input → preserves old filters
 * links → rebuild full URL state
 * EJS → reflects backend state in UI
 */
```

---
