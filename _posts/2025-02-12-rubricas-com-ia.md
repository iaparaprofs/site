---
layout: post
title: "Post de teste 2"
date: 2025-02-12 10:00:00 -0300
categories: teste
---

Segundo post de teste, agora mostrando um exemplo de função JavaScript para calcular médias.

```javascript
function media(valores) {
  if (!Array.isArray(valores) || valores.length === 0) return 0;
  const total = valores.reduce((soma, valor) => soma + valor, 0);
  return total / valores.length;
}

console.log(media([7, 8.5, 9]));
```
