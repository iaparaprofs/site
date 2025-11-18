---
layout: default
title: "Página Inicial"
---

<div id="posts-list" class="posts-list"></div>
<div id="scroll-sentinel" aria-hidden="true"></div>

<template id="post-template">
  <article class="post fade-in">
    <p class="post-date"></p>
    <h2 class="post-title"></h2>
    <p class="excerpt"></p>
    <p class="read-more"></p>
  </article>
</template>

<script>
  const postsData = [
    {% for post in site.posts %}
    {
      title: {{ post.title | jsonify }},
      excerpt: {{ post.excerpt | strip_html | strip_newlines | jsonify }},
      url: {{ post.url | relative_url | jsonify }},
      date: {{ post.date | date: "%d %b %Y" | jsonify }}
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  const postsListElement = document.getElementById('posts-list');
  const sentinel = document.getElementById('scroll-sentinel');
  const postTemplate = document.getElementById('post-template');
  const searchInput = document.getElementById('post-search');

  const batchSize = 6;
  let renderedCount = 0;
  let searchTerm = '';

  function renderPost(post) {
    const templateContent = postTemplate.content.cloneNode(true);
    const postDate = templateContent.querySelector('.post-date');
    const titleElement = templateContent.querySelector('.post-title');
    const excerptElement = templateContent.querySelector('.excerpt');
    const readMoreElement = templateContent.querySelector('.read-more');

    postDate.textContent = post.date;
    const titleLink = document.createElement('a');
    titleLink.href = post.url;
    titleLink.textContent = post.title;
    titleElement.appendChild(titleLink);

    excerptElement.textContent = post.excerpt;
    const readMoreLink = document.createElement('a');
    readMoreLink.href = post.url;
    readMoreLink.textContent = 'Leia mais';
    readMoreElement.appendChild(readMoreLink);

    postsListElement.appendChild(templateContent);
  }

  function filterPosts() {
    if (!searchTerm) return postsData;
    const normalized = searchTerm.toLowerCase();
    return postsData.filter(post =>
      post.title.toLowerCase().includes(normalized) ||
      post.excerpt.toLowerCase().includes(normalized)
    );
  }

  function clearPosts() {
    postsListElement.innerHTML = '';
    renderedCount = 0;
  }

  function renderNextBatch() {
    const filteredPosts = filterPosts();

    if (renderedCount === 0 && filteredPosts.length === 0) {
      const emptyState = document.createElement('p');
      emptyState.className = 'empty-state';
      emptyState.textContent = 'Nenhum post encontrado.';
      postsListElement.appendChild(emptyState);
      sentinel.classList.add('hidden');
      return;
    }

    const nextPosts = filteredPosts.slice(renderedCount, renderedCount + batchSize);
    nextPosts.forEach(renderPost);
    renderedCount += nextPosts.length;

    if (renderedCount >= filteredPosts.length) {
      sentinel.classList.add('hidden');
    } else {
      sentinel.classList.remove('hidden');
    }
  }

  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        renderNextBatch();
      }
    });
  }, {
    rootMargin: '200px'
  });

  observer.observe(sentinel);

  function resetAndRender() {
    clearPosts();
    renderNextBatch();
  }

  if (searchInput) {
    searchInput.addEventListener('input', (event) => {
      searchTerm = event.target.value.trim();
      clearPosts();
      renderNextBatch();
    });
  }

  document.addEventListener('DOMContentLoaded', () => {
    resetAndRender();
  });
</script>
