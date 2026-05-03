
---


# **Home.ejs - search & category**



```ejs
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

  

      </div>

  

      <!-- Category filter pills -->

      <div class="flex items-center gap-1.5 mb-6 overflow-x-auto pb-1" style="scrollbar-width: none;">

        <% const categories = ['all','coding','education','resource','entertainment','news','other']; %>

        <% categories.forEach(function(cat) { %>

          <a

            href="/?category=<%= cat %><%= typeof searchQuery !== 'undefined' && searchQuery ? '&search=' + searchQuery : '' %>"

            class="filter-btn <%= selectedCategory === cat ? 'active' : '' %>"

          ><%= cat %></a>

        <% }); %>

      </div>
```