
### 📄  Automação de Desabilitação de Usuários Greenhouse com Google Sheets

Este script automatiza a desativação de usuários desligados da empresa via API da Greenhouse, a partir de dados armazenados em uma planilha do Google Sheets.

---

## ✅ O que este script faz?

- Acessa uma planilha com abas mensais (`Jan/25`, `Fev/25`, etc.) com dados de desligamento
- Lê e-mails e datas de rescisão a partir da **linha 2** da planilha
- Verifica se o colaborador foi desligado **até a data atual**
- **Desabilita o usuário na Greenhouse** via requisição `PATCH` na API
- Evita duplicidade através de um arquivo de log local (`log_desabilitados.csv`)

---

## 🧩 Estrutura esperada da planilha

A planilha precisa conter:

- Abas nomeadas no padrão `Mês/25` (ex: `Abr/25`, `Mai/25`)
- Linha 2 com os nomes das colunas
- Duas colunas obrigatórias:
  - **`E-mail Corporativo`**: endereço de e-mail da pessoa a ser desabilitada
  - **`Data Rescisão`**: data da rescisão no formato `dd/mm/aa`

---

## ⚙️ Requisitos

- Rodar o script no Google Colab
- Ter acesso à planilha do Google Sheets
- Ter acesso à API da Greenhouse

---

## 🚀 Como usar

1. **Abra no Google Colab**
2. **Execute todas as células**
3. Quando solicitado, autentique com sua conta Google para acesso ao Google Sheets
4. O script:
   - Verifica as abas da planilha com nome `/25`
   - Processa os e-mails com `Data Rescisão <= hoje`
   - Verifica se o e-mail **já está no log**
   - Se não estiver, **envia o PATCH para desabilitação**
   - Salva o resultado no arquivo `log_desabilitados.csv`

---

## 📦 Dependências

As bibliotecas são instaladas automaticamente no início do script:

```python
!pip install --upgrade gspread pandas gspread_dataframe
```

---

## 🔐 Como obter as credenciais da Greenhouse

### 1. API Token

1. Acesse o Greenhouse com uma conta de administrador
2. Vá em: `Configure > Dev Center > API Credential Management`
3. Clique em **Create New API Key**
   - Tipo: `Harvest`
   - Nome: `Disable Users Script`
4. Salve o token gerado
5. Use no código assim:

```python
API_TOKEN = "Basic SEU_TOKEN"
```

> ⚠️ O token precisa estar no formato `Basic <seu_token>`, com **Base64 encoding** se necessário.

---

### 2. On-Behalf-Of ID

- Este ID representa o `user_id` do usuário em nome do qual a requisição será feita
- Você pode encontrá-lo em:
  - `GET /v1/users` via API
  - Ou consultar via interface com ajuda da Greenhouse

Use no script assim:

```python
ON_BEHALF_OF_ID = "1234567890"
```

---

## 📄 Sobre o log `log_desabilitados.csv`

- Armazena todos os e-mails processados com:
  - `email`
  - `data_rescisao`
  - `data_execucao`
  - `status` (desabilitado ou erro)
- Evita que o mesmo e-mail seja processado novamente
- Pode ser baixado manualmente pelo Colab ou enviado diretamente para sua pasta no drive

---

## 🧠 Exemplo de saída

```text
🔍 Abas selecionadas automaticamente:
['Mar/25', 'Abr/25', 'Mai/25']

✅ joana.silva@empresa.com desabilitado (rescisão em 2025-04-14)
⏩ paulo.souza@empresa.com já foi processado anteriormente — ignorando.
❌ Erro ao desabilitar maria.lima@empresa.com — 404 | User not found

📦 Log salvo em: /content/log_desabilitados-dd/mm/yyyy.csv
```

---

## 📌 Recomendações

- Agende este script para rodar diariamente no Colab ou via cron no servidor
- Salve o log externamente (ex: Google Drive) para persistência entre execuções
- Avalie mover o log para uma aba de log no próprio Google Sheets, se quiser manter tudo na nuvem

---

## 🛡️ Licença

MIT License

