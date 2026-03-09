# 🌤️ Agente de Clima com n8n + Telegram

## 📌 Descrição

Este projeto demonstra como criar um **chatbot no Telegram utilizando n8n** para consultar informações de clima através de uma API de previsão do tempo.

O bot recebe uma **cidade enviada pelo usuário no Telegram** e responde com as **informações meteorológicas atuais**.

---

# 📋 Pré-requisitos

Antes de iniciar, você precisará ter instalado:

* Node.js
* n8n
* Conta no **Telegram**
* Conta em uma **API de clima**
* **ngrok** (para expor o webhook do n8n)

---

# 📂 Estrutura do Projeto

```
telegram-weather-chatbot
│
├── workflow-telegram-chatbot.json
└── README.md
```

Arquivo principal do workflow:

```
workflow-telegram-chatbot.json
```

Este arquivo contém todo o **workflow configurado para o n8n**.

---

# 📥 Importando o Workflow no n8n

Antes de configurar o bot, é necessário **baixar o workflow do projeto**.

Baixe o arquivo:

```
workflow-telegram-chatbot.json
```

Após baixar o arquivo, siga os passos abaixo para importá-lo no **n8n**.

---

## 🔹 Abrindo o n8n

Primeiro inicie o **n8n** com o comando:

```bash
n8n start
```

O n8n será iniciado localmente na porta padrão:

```
http://localhost:5678
```

Abra esse endereço no navegador para acessar a interface do n8n.

---

## 🌐 Expondo o n8n para o Telegram usando ngrok

O Telegram precisa acessar o **webhook do n8n pela internet**.
Como o n8n está rodando localmente, precisamos utilizar o **ngrok** para criar um túnel público.

### Baixar o ngrok

Acesse:

[https://ngrok.com/download](https://ngrok.com/download)

Baixe a versão correspondente ao seu sistema operacional e instale na sua máquina.

---

### Autenticar o ngrok

Após criar uma conta no ngrok, copie seu **authtoken** no painel do site e execute:

```bash
ngrok config add-authtoken SEU_TOKEN
```

---

### Expor a porta do n8n

Como o n8n está rodando na porta **5678**, execute:

```bash
ngrok http 5678
```

O ngrok irá gerar uma URL pública semelhante a:

```
https://abcd-1234.ngrok-free.app
```

---

### Usar a URL do ngrok no n8n

No **Telegram Trigger** dentro do n8n, utilize a URL gerada pelo ngrok como base para o webhook.

Exemplo:

```
https://abcd-1234.ngrok-free.app
```

Isso permitirá que o **Telegram envie mensagens para o seu workflow local**.

---

## 🔹 Importar Workflow via Arquivo

1. Abra o **n8n**
2. No menu lateral clique em **Workflows**
3. Clique em **Import**
4. Selecione **From File**
5. Escolha o arquivo:

```
workflow-telegram-chatbot.json
```

6. Clique em **Import**

O workflow será carregado no editor.

---

# 🤖 Criando o Bot no Telegram

1. Abra o Telegram
2. Procure por:

```
@BotFather
```

3. Execute o comando:

```
/start
```

4. Depois execute:

```
/newbot
```

5. Escolha:

* Nome do bot
* Username do bot

Exemplo:

```
Weather Bot
weather_example_bot
```

Após criar o bot, o **BotFather fornecerá um Token** semelhante a:

```
123456789:AAExampleTokenExample
```

Guarde esse token.

---

# 🔑 Configurando Credenciais do Telegram no n8n

No workflow importado:

1. Abra o node **Telegram Trigger**
2. Clique em **Credentials**
3. Clique em **Create New**
4. Cole o **Token do BotFather**
5. Salve

---

# 🌤️ Criando Conta na API de Clima OpenWeather

1.  Acesse: https://openweathermap.org/
2.  Faça login
3.  Vá em **API Keys**
4.  Clique em **Create Key**
5.  Copie sua chave

⚠️ A ativação pode levar alguns minutos.

Exemplo:

```
abc123exampleapikey
```

---

# ⚙️ Configurando o Node WeatherAPI

No workflow existe um node responsável por consultar o clima.

Configure da seguinte forma.

**Node:** `HTTP Request`

### Método

```
GET
```

### URL

```
https://api.openweathermap.org/data/2.5/weather
```

### Query Parameters

Adicione os parâmetros:

| Nome  | Valor                        |
| ----- | ---------------------------- |
| appid | SUA_API_KEY                  |
| q     | {{$json["message"]["text"]}} |
| units | metric                       |
| lang  | pt                           |

Explicação:

* **appid** → sua chave da API
* **q** → cidade enviada pelo usuário no Telegram
* **units=metric** → temperatura em Celsius
* **lang=pt** → resposta em português

---

# 💬 Configurando a Resposta do Bot

No node **Telegram Send Message**, configure o campo **Text** com algo semelhante a:

```
🌤️ Clima em {{$json["name"]}}

Temperatura: {{$json["main"]["temp"]}} °C
Sensação térmica: {{$json["main"]["feels_like"]}} °C
Umidade: {{$json["main"]["humidity"]}}%
Condição: {{$json["weather"][0]["description"]}}
```

---

# ▶️ Ativando o Workflow

Depois de configurar tudo:

1. Clique no botão **Activate** no topo do n8n
2. Abra o Telegram
3. Envie uma cidade para o bot

Exemplo:

```
Rio de Janeiro
```

Resposta esperada:

```
🌤️ Clima em Rio de Janeiro

Temperatura: 29°C
Sensação térmica: 31°C
Umidade: 70%
Condição: céu parcialmente nublado
```
