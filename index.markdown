---
layout: default
title: "Página Inicial"
---

<div class="posts-list">
  {% for post in site.posts limit: 10 %}
    <div class="post">
      <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      <p class="excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
      <p><a href="{{ post.url | relative_url }}">Leia mais</a></p>
    </div>
  {% endfor %}
</div>

<!-- Botão "Carregar mais" -->
<button id="load-more-btn" onclick="loadMorePosts()">Carregar mais</button>

<script>
  let currentLimit = 10;
  const homeUrl = "{{ '/' | relative_url }}";

  function loadMorePosts() {
    currentLimit += 10;
    window.location.href = `${homeUrl}?limit=${currentLimit}`;
  }
</script>
