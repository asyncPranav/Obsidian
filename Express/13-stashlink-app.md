
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
