# Frifas Bio API - Documentação Completa

> API para atualização de biografia de jogadores do Free Fire, utilizando token EAT ou credenciais manuais (UID + senha).

A Frifas Bio API oferece endpoints para desenvolvedores que desejam atualizar a biografia de jogadores do Free Fire. Esta documentação descreve detalhadamente os endpoints, payloads de resposta e tratamentos de erro.

---

## Informações Gerais

*   **Base URL:** `https://fluxdevservice.com/api/frifas`
*   **Formato de Dados:** JSON
*   **Autenticação:** Chave de API via parâmetro `key`
*   **Timeout Padrão:** 30 segundos
*   **Região Padrão:** `br`

---

## 1. Atualizar Bio via Token (Account)

Atualiza a biografia de uma conta utilizando o token EAT (access token).

**Endpoint:** `GET /update-bio/account`

### Parâmetros

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `key` | `string` | Sim | Chave de API válida |
| `eat_token` | `string` | Sim | Token EAT da conta |
| `newbio` | `string` | Sim | Nova biografia (máximo 500 caracteres) |

### Resposta de Sucesso (200 OK)

```json
{
  "sucesso": true,
  "status": "SUCESSO_UPDATE",
  "mensagem": "Biografia atualizada com sucesso!",
  "statuscode": 200,
  "dados": [
    {
      "conta": {
        "nome_conta": "Hubs-Xua0wnY",
        "id_conta": "14755249219",
        "regiao": "BR"
      },
      "assinatura": {
        "bio_antiga": "Antiga biografia aqui",
        "bio_nova": "Nova biografia inserida"
      }
    }
  ]
}
```

### Códigos de Erro

| HTTP | Código | Descrição |
| :--- | :--- | :--- |
| 400 | `ERRO_TOKEN_AUSENTE` | `eat_token` não informado |
| 400 | `ERRO_BIO_AUSENTE` | `newbio` não informado |
| 400 | `ERRO_BIO_EXCEDE_LIMITE` | Biografia excede 500 caracteres |
| 400 | `ERRO_ATUALIZACAO` | Falha na atualização (token inválido/serviço) |
| 401 | `INVALID_KEY` | Chave de API inválida ou ausente |
| 502 | `ERRO_CONEXAO` | Falha de conexão com o serviço externo |

---

## 2. Atualizar Bio via Guest (UID + Senha)

Atualiza a biografia de uma conta utilizando UID e senha. O endpoint obtém os tokens automaticamente e realiza a atualização.

**Endpoint:** `GET /update-bio/guest`

### Parâmetros

| Parâmetro | Tipo | Obrigatório | Descrição |
| :--- | :--- | :--- | :--- |
| `key` | `string` | Sim | Chave de API válida |
| `uid` | `string` | Sim | UID da conta do Free Fire |
| `password` | `string` | Sim | Senha da conta |
| `newbio` | `string` | Sim | Nova biografia (máximo 500 caracteres) |
| `region` | `string` | Não | Região da conta (`br`, `us`, etc.). Padrão: `br` |

### Resposta de Sucesso (200 OK)

```json
{
  "sucesso": true,
  "status": "SUCESSO_UPDATE",
  "mensagem": "Biografia atualizada com sucesso!",
  "statuscode": 200,
  "dados": [
    {
      "conta": {
        "nome_conta": "Hubs-Xua0wnY",
        "id_conta": "14755249219",
        "regiao": "BR"
      },
      "assinatura": {
        "bio_antiga": "Antiga biografia aqui",
        "bio_nova": "Nova biografia inserida"
      },
      "guest": {
        "jwt": "eyJhbGciOiJIUzI1NiIs...",
        "access_token": "4af1c19c53b8cffb...",
        "open_id": "c99127a7631e01ea...",
        "uid": "4522060299"
      }
    }
  ]
}
```

### Códigos de Erro

| HTTP | Código | Descrição |
| :--- | :--- | :--- |
| 400 | `ERRO_UID_AUSENTE` | `uid` não informado |
| 400 | `ERRO_PASSWORD_AUSENTE` | `password` não informado |
| 400 | `ERRO_BIO_AUSENTE` | `newbio` não informado |
| 400 | `ERRO_BIO_EXCEDE_LIMITE` | Biografia excede 500 caracteres |
| 400 | `ERRO_ATUALIZACAO` | Credenciais inválidas ou falha na API externa |
| 401 | `INVALID_KEY` | Chave de API inválida ou ausente |
| 502 | `ERRO_CONEXAO` | Falha de conexão com o serviço externo |
| 503 | `ERRO_SERVICE_OFFLINE` | Serviço de biografia offline |
| 504 | `ERRO_TIMEOUT` | Tempo limite de 30 segundos excedido |

---

## Regras de Negócio

*   **Limite de caracteres:** A biografia não pode exceder 500 caracteres.
*   **Timeout:** 30 segundos – após esse período, retorna erro 504.
*   **Região padrão:** `br` (Brasil) quando não informada.
*   **Tokens no modo guest:** Os campos `jwt`, `access_token` e `open_id` são retornados para possível reutilização no endpoint `/account`.

---
*Frifas API Service - Documentação Oficial*
