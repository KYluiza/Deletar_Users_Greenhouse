
### 📄 `README.md`

```markdown
# 🔐 Automação para Desabilitar Usuários Demitidos via Greenhouse API

Este script automatiza o processo de **desabilitação de usuários no Greenhouse** com base em uma planilha de desligamentos no Google Sheets. Ele verifica as datas de rescisão e, se já passaram (inclusive hoje), envia um `PATCH` para desabilitar o colaborador usando a API do Greenhouse.

---

## 🚀 O que este script faz?

- Conecta com uma planilha do Google Sheets que contém os desligamentos da empresa
- Lê abas nomeadas no padrão `Jan/25`, `Fev/25`, etc.
- Verifica se a **data de rescisão já passou ou é hoje**
- Se sim, **envia uma requisição PATCH para desabilitar o usuário na Greenhouse** com base no e-mail

---

## 📋 Requisitos

- Conta Google com acesso à planilha de desligamentos
- Permissão para usar a [API do Greenhouse Harvest](https://developers.greenhouse.io/harvest.html)
- Acesso ao [Google Colab](https://colab.research.google.com/) ou ambiente com as bibliotecas abaixo

---

## 📦 Instalação de dependências

Este script já começa com a instalação automática via Colab:

```python
!pip install --upgrade gspread pandas gspread_dataframe
```

---

## 🔑 Como obter acesso à API

### 1. Autenticação com Google Sheets

Este script usa a autenticação integrada do Google Colab (`google.auth.default()`), então **basta rodar no Colab**, autenticar com sua conta e conceder as permissões solicitadas.

### 2. Obter Token da API do Greenhouse

1. Acesse sua conta de administrador no Greenhouse
2. Vá até **Integrations > API Credentials**
3. Crie uma credencial do tipo **Harvest API**
4. Copie o `API Token` e substitua a variável `API_TOKEN` no código:

```python
API_TOKEN = "Basic SEU_TOKEN_AQUI"
```

> 🔒 Dica: use o `Basic <seu_token_base64>` com o prefixo `Basic` + um espaço, ou codifique o token corretamente.

### 3. Obter o campo `On-Behalf-Of-ID`

Você precisa do ID do administrador autorizado. Peça esse valor ao time de TI ou verifique com a Greenhouse qual o `user_id` correspondente ao usuário da integração.

```python
ON_BEHALF_OF_ID = "1234567890"
```

---

## 🧠 Estrutura da Planilha Esperada

Cada aba da planilha deve seguir o formato de nomeação `Mes/25` (ex: `Jan/25`, `Fev/25` etc.)

A partir da **linha 2** (ou seja, a linha 1 é título visual), são esperadas no mínimo as seguintes colunas:

| Coluna               | Descrição                                  |
|----------------------|--------------------------------------------|
| `E-mail Corporativo` | E-mail que será desabilitado               |
| `Data Rescisão`      | Data da demissão (formato: dd/mm/aa)       |

> 📝 Observação: se o cabeçalho estiver na primeira linha, altere esta linha no código:
>
> ```python
> df = pd.DataFrame(dados[2:], columns=dados[1])
> ```
> para:
> ```python
> df = pd.DataFrame(dados[1:], columns=dados[0])
> ```

---

## ⚙️ Como o código funciona

1. **Autenticação com Google**
2. Abre a planilha informada na variável `URL_PLANILHA_DE_RESCISÕES`
3. Seleciona todas as abas com nome no padrão `/25`
4. Lê os dados a partir da segunda linha (usando a linha 2 como cabeçalho)
5. Para cada colaborador:
   - Verifica se a data de rescisão já passou (ou é hoje)
   - Envia um `PATCH` para a API do Greenhouse com o e-mail do usuário
6. Imprime o resultado da operação (`✅` ou `❌`)

---

## ✅ Exemplo de resultado no console

```
🔍 Abas selecionadas automaticamente:
['Jan/25', 'Fev/25', 'Mar/25']

✅ maria@empresa.com desabilitado (rescisão em 2025-04-15)
❌ Erro ao desabilitar joao@empresa.com — 404 | User not found
```

---

## 📌 Notas finais

- O script **não salva histórico ou log** por padrão, mas pode ser estendido para isso.
- Idealmente, este código deve rodar **diariamente** para garantir a revogação oportuna de acessos.
- Para evitar duplicidade de requisições, você pode armazenar os e-mails já processados num log externo (CSV, banco de dados etc.)

---

## 🤝 Contribuições

Pull requests são bem-vindos. Para alterações significativas, abra uma issue primeiro para discutir o que você gostaria de mudar.

---

## 🛡️ Licença

Este projeto está sob a licença MIT.
```

---

Se quiser, posso gerar esse `README.md` já formatado para você colar direto ou subir no GitHub. Quer que eu gere o arquivo pronto?
