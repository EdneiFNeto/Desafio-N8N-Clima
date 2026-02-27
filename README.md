# 🌤️ Agente de Clima com n8n + Telegram

## 📌 Descrição

Este projeto implementa um **Agente de Clima** utilizando **n8n** integrado ao **Telegram**.

O agente permite que usuários consultem a **temperatura atual e as condições climáticas** de qualquer cidade do Brasil diretamente pelo Telegram.

### 🔄 Fluxo da aplicação

* Telegram → interação com o usuário
* n8n → orquestração do fluxo
* OpenAI → interpretação da mensagem
* OpenWeather → dados climáticos atualizados

📌 Exemplo de resposta:

> 🌧️ A temperatura em Minas Gerais é de 18°C com chuva leve.

---

# 📋 Pré-requisitos

Antes de começar, você precisa ter:

* n8n instalado
* Conta no Telegram
* Conta na OpenAI
* Conta na OpenWeather
* ngrok instalado

---

# 🔑 Criando as API Keys

## 🔹 OpenAI

1. Acesse: [https://platform.openai.com/](https://platform.openai.com/)
2. Faça login
3. Vá em **API Keys**
4. Clique em **Create new secret key**
5. Copie a chave gerada

---

## 🔹 OpenWeather

1. Acesse: [https://openweathermap.org/](https://openweathermap.org/)
2. Faça login
3. Vá em **API Keys**
4. Clique em **Create Key**
5. Copie sua chave

⚠️ A ativação pode levar alguns minutos.

---

# 🤖 Criando o Bot no Telegram

1. Abra o Telegram
2. Procure por **@BotFather**
3. Execute:

```
/start
/newbot
```

4. Defina:

   * Nome do bot
   * Username (deve terminar com `bot`)

5. Copie o **Bot Token**

---

# ⚙️ Configurando o n8n

## 🔹 Configurar OpenAI

1. Abra o workflow
2. No node **Chat Model / OpenAI**
3. Crie nova credencial
4. Cole sua **API Key**
5. Salve

---

## 🔹 Adicionar o Node HTTP Request (OpenWeather)

1. Clique em **+**
2. Procure por **HTTP Request**
3. Adicione ao fluxo

### 🔸 Configuração

**Método:** `GET`
**URL:**

```
https://api.openweathermap.org/data/2.5/weather
```

---

## 🔹 Configurar Query Parameters

Clique em **Add Parameter** e adicione:

| Nome  | Valor                        |
| ----- | ---------------------------- |
| q     | Cidade,UF,BR                 |
| appid | {{$env.OPENWEATHER_API_KEY}} |
| units | metric                       |
| lang  | pt_br                        |

### 🔎 Explicação

* `q` → Cidade recebida do usuário + ,BR
* `appid` → Sua API Key
* `units=metric` → Temperatura em Celsius
* `lang=pt_br` → Resposta em português

---

## 🧪 Testar a Requisição

Clique em **Execute Node**.

Se estiver correto, você receberá algo como:

```json
{
  "name": "Recife",
  "main": {
    "temp": 29.5
  },
  "weather": [
    {
      "description": "céu limpo"
    }
  ]
}
```

---

# ▶️ 4️⃣ Executando o Projeto Localmente

## 🔹 Iniciar o n8n

```bash
n8n start
```

Acesse:

```
http://localhost:5678
```

---

## 🔹 Expor com ngrok (Webhook HTTPS)

O Telegram exige um endpoint HTTPS público.

Instale: [https://ngrok.com/](https://ngrok.com/)

Execute:

```bash
ngrok http 5678
```

Copie a URL HTTPS gerada.

---

## 🔹 Configurar variável WEBHOOK_URL

### Linux / Mac

```bash
export WEBHOOK_URL=https://sua-url-ngrok
n8n start
```

### Windows (PowerShell)

```powershell
$env:WEBHOOK_URL="https://sua-url-ngrok"
n8n start
```

---

# 🧪  Como Usar

No Telegram, envie:

```
Rio de Janeiro, RJ
```

O bot responderá com a temperatura atual e descrição do clima.

---

# ❌ Tratamento de Erros

O agente retorna mensagem com 😢 quando:

❌ Cidade não encontrada

Use o formato:

```
Cidade,UF
Ex: São Paulo,SP
```

---

# 🛠️ Tecnologias Utilizadas

* n8n
* OpenAI
* Telegram Bot API
* OpenWeather
* ngrok

---

# 📎 Observações

* O **ngrok** é necessário para permitir que o Telegram acesse o n8n local via HTTPS.
* Este projeto demonstra a integração de **Agentes de IA com automação e mensageria**.
