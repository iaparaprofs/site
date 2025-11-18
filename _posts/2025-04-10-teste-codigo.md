---
layout: default
title: "Teste - Código"
date: 2025-04-10 10:00:00 +0000
categories: demonstracao
---

Nem toda postagem precisa ser apenas texto corrido. Às vezes, um exemplo de código ajuda a mostrar rapidamente uma ideia, e a formatação com destaque deixa tudo legível.

```python
from datetime import datetime


def formata_data_br(data):
    """Recebe um datetime e devolve a data no padrão brasileiro."""
    return data.strftime("%d/%m/%Y")


hoje = datetime.now()
print(f"Hoje é {formata_data_br(hoje)}")
```

Para casos menores, você também pode usar `code inline` para nomear funções ou variáveis sem quebrar a leitura.
