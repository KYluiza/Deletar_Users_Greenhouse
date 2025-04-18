### 📄 Automação para desabilitar Usuários no Greenhouse automaticamente com Log Diário

Este script automatiza a **revogação de acessos de colaboradores desligados** via API do Greenhouse, com base em uma planilha do Google Sheets, e gera um log diário com os resultados.

---

## ✅ O que este script faz?

- Lê abas mensais (`Jan/25`, `Fev/25`, etc.) de uma planilha de desligamentos
- Verifica se o colaborador foi desligado até hoje
- Desabilita o e-mail correspondente na Greenhouse via API (`PATCH`)
- Cria um log `.csv` diário com os e-mails processados e seus status

---

## 🧩 Estrutura esperada da planilha

A planilha precisa conter:

- Abas com nomes no formato `Mes/25` (ex: `Abr/25`, `Mai/25`)
- Linha 2 com os nomes das colunas (linha 1 é visual)
- Pelo menos estas colunas:

| Coluna               | Descrição                                    |
|----------------------|----------------------------------------------|
| `E-mail Corporativo` | E-mail do colaborador (usado na API)         |
| `Data Rescisão`      | Data da rescisão (formato `dd/mm/aa`)        |

---

## ⚙️ Requisitos

- Google Colab para autenticação automática com Google Sheets
- Permissão de leitura na planilha
- Permissão de escrita na API da Greenhouse (Harvest API)

---

## 🚀 Como usar

1. Acesse o [Google Colab](https://colab.research.google.com/)
2. Cole e execute o código Python completo
3. Autentique com sua conta Google quando solicitado
4. O script:
   - Seleciona abas do tipo `Mês/25`
   - Verifica todos os desligamentos até a data atual
   - Desabilita os e-mails via Greenhouse
   - Gera um log no formato `log_desabilitacoes_20250417.csv`

---

## 🔐 Como obter as credenciais do Greenhouse

### 1. API Token (Harvest)

1. Acesse: `Configure > Dev Center > API Credential Management`
2. Clique em **Create New API Key**
3. Tipo: `Harvest`
4. Nome: `Disable Users Script`
5. Salve o token gerado

> **IMPORTANTE:** o token precisa ser usado com o prefixo `"Basic "`  
> Exemplo:
> ```python
> API_TOKEN = "Basic SEU_TOKEN_AQUI"
> ```

---

### 2. On-Behalf-Of ID

- Esse ID representa o `user_id` que executa a requisição na API
- Para obter:
  - Use `GET /v1/users` da API
  - Ou peça ao administrador da Greenhouse

```python
ON_BEHALF_OF_ID = "1234567890"
```

---

## 📦 Sobre o log diário

O script gera um arquivo `.csv` por dia com os resultados:

- Nome: `log_desabilitacoes_YYYYMMDD.csv`
- Colunas:
  - `email`
  - `data_rescisao`
  - `data_execucao`
  - `status`

---

## 🧠 Exemplo de saída

```text
🔍 Abas selecionadas automaticamente:
['Mar/25', 'Abr/25', 'Mai/25']

✅ joana.silva@empresa.com desabilitado (rescisão em 2025-04-14)
❌ Erro ao desabilitar maria.lima@empresa.com — 404 | User not found

📦 Log salvo com 2 registros em: /content/log_desabilitacoes_yyyymmdd.csv
```

---

## 📌 Recomendações

- Execute diariamente (ex: agende via Google Colab ou servidor)
- Armazene os logs no Google Drive ou outro local persistente
- Revise o log manualmente se houver erros (`status` começa com `erro`)

---

## 🛡️ Licença

MIT License



