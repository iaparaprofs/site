---
layout: default
title: Todos os posts
---

<section class="posts-page">
  <div class="posts-page__header">
    <h1 class="brand-gradient-text">Todos os posts</h1>
    <form class="search-form" role="search" onsubmit="event.preventDefault();">
      <input
        id="post-search"
        class="form-control"
        type="search"
        placeholder="Buscar posts"
        aria-label="Buscar posts"
        autocomplete="off"
      >
    </form>
  </div>

  {% assign posts_with_titles = site.posts | where_exp: "post", "post.title" %}
  <div id="posts-list" class="posts-list"></div>
  <p id="empty-state" class="empty-state hidden">Nenhum post encontrado.</p>
</section>

<script>
  const postsData = [
    {% for post in posts_with_titles %}
      {
        url: "{{ post.url | relative_url }}",
        title: "{{ post.title | escape }}",
        date: "{{ post.date | date: '%d/%m/%Y' }}",
        excerpt: "{{ post.excerpt | strip_newlines | escape }}"
      }
      {% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  const postsList = document.getElementById('posts-list');
  const searchInput = document.getElementById('post-search');
  const emptyState = document.getElementById('empty-state');

  const createPostMarkup = (post) => `
    <article class="post fade-in">
      <header class="post-header">
        <p class="post-date"><a href="${post.url}">${post.date}</a></p>
        <h2 class="post-title"><a href="${post.url}">${post.title}</a></h2>
      </header>
      <p>${post.excerpt}</p>
    </article>
  `;

  const renderPosts = (posts) => {
    if (!posts.length) {
      postsList.innerHTML = '';
      emptyState.classList.remove('hidden');
      return;
    }

    postsList.innerHTML = posts.map(createPostMarkup).join('');
    emptyState.classList.add('hidden');
  };

  renderPosts(postsData);

  searchInput.addEventListener('input', (event) => {
    const query = event.target.value.toLowerCase().trim();

    const filtered = postsData.filter((post) =>
      post.title.toLowerCase().includes(query) ||
      post.excerpt.toLowerCase().includes(query)
    );

    renderPosts(filtered);
  });
</script>
