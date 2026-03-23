# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

---

## Estratégia de Integração

### Como os dados são carregados?

> Descreva como seu agente acessa a base de conhecimento.

Os arquivos JSON e CSV são carregados no início da execução da aplicação utilizando bibliotecas como pandas e json em Python.

Durante a inicialização do sistema:

- Os arquivos CSV são convertidos em DataFrames para facilitar consultas e análise de dados.
- Os arquivos JSON são carregados como estruturas de dados (dicionários/listas) para permitir acesso rápido às informações.

Esses dados ficam disponíveis na memória da aplicação e podem ser consultados sempre que o usuário fizer uma pergunta relacionada às suas finanças.

Exemplo simplificado:

```

import pandas as pd
import json

transacoes = pd.read_csv("transacoes.csv")

with open("perfil_investidor.json", "r") as f:
    perfil = json.load(f)

```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados são utilizados de duas formas principais:

### 1. Contexto no Prompt

Antes de enviar a pergunta do usuário para o modelo de linguagem, o sistema monta um contexto com informações relevantes, como:

* Perfil do investidor
* Histórico recente de transações
* Interações anteriores

Esse contexto é incluído no prompt enviado ao modelo, permitindo respostas mais personalizadas.

### 2. Consulta Dinâmica

Alguns dados são consultados dinamicamente no momento da pergunta, por exemplo:

* Análise de gastos recentes
* Identificação de categorias de despesas
* Verificação de produtos financeiros compatíveis com o perfil do usuário

Dessa forma, o agente evita carregar grandes volumes de dados no prompt e utiliza apenas as informações necessárias para gerar a resposta.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```