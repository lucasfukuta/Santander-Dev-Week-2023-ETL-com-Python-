# Santander Dev Week 2023 (ETL com Python)
## Contexto: 
Você é um cientista de dados no Santander e recebeu a tarefa de envolver seus clientes de maneira mais personalizada. Seu objetivo é usar o poder da IA Generativa para criar mensagens de marketing personalizadas que serão entregues a cada cliente.

## Condições do Problema:
  1) Você recebeu uma planilha simples, em formato CSV ('SDW2023.csv'), com uma lista de IDs de usuário do banco:

      (lista criada para resolver esse desafio)
```python
     UserID
     4680
     4681
     4682
     4684
     4686
```

  2) Seu trabalho é consumir o endpoint GET https://sdw-2023-prd.up.railway.app/users/{id} (API da Santander Dev Week 2023) para obter os dados de cada cliente.
  3) Depois de obter os dados dos clientes, você vai usar a API do ChatGPT (OpenAI) para gerar uma mensagem de marketing personalizada para cada cliente. Essa mensagem deve enfatizar a importância dos investimentos.
  4) Uma vez que a mensagem para cada cliente esteja pronta, você vai enviar essas informações de volta para a API, atualizando a lista de "news" de cada usuário usando o endpoint PUT https://sdw-2023-prd.up.railway.app/users/{id}.

```python
    # Utilize sua própria URL se quiser ;)
    # Repositório da API: https://github.com/digitalinnovationone/santander-dev-week-2023-api
    sdw2023_api_url = 'https://sdw-2023-prd.up.railway.app'
```
## Extract
Extrai a lista de IDs de usuário a partir do arquivo CSV criado. Para cada ID, fiz uma requisição GET para obter os dados do usuário correspondente.
```python
import pandas as pd

df = pd.read_csv('UserID.csv')
user_ids = df['UserID'].tolist()
print(user_ids)
```
### Saída:
```
[4680, 4681, 4682, 4684, 4686]
```

```python
import requests
import json

def get_user(id):
  response = requests.get(f'{sdw2023_api_url}/users/{id}')
  return response.json() if response.status_code == 200 else None

users = [user for id in user_ids if (user := get_user(id)) is not None]
print(json.dumps(users, indent=2))
```
### Saída:
```
{
    "id": 4680,
    "name": "Moira",
    "account": {
      "id": 4956,
      "number": "xxxx xxxx xxxx 1505",
      "agency": "00005-5",
      "balance": 1000.0,
      "limit": 1000.0
    },
    "card": {
      "id": 4543,
      "number": "xxxx xxxx xxxx 1505",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 8714,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Invista hoje, colha amanh\u00e3. #FuturoSeguro"
      },
      {
        "id": 8715,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Moira, investir \u00e9 primordial p/ crescimento financeiro. Construa seu futuro c/ sabedoria!\ud83d\udcb0\ud83d\udcaa\ud83c\udf1f"
      },
      {
        "id": 8716,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Os investimentos te garantem prosperidade no futuro. Cres\u00e7a o seu dinheiro, n\u00e3o s\u00f3 o gaste."
      }
    ]
  },
  {
    "id": 4681,
    "name": "Rein",
    "account": {
      "id": 4958,
      "number": "xxxx xxxx xxxx 1504",
      "agency": "00004-4",
      "balance": 1000.0,
      "limit": 1000.0
    },
    "card": {
      "id": 4544,
      "number": "xxxx xxxx xxxx 1504",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 8717,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Invista para o futuro e garanta sua estabilidade financeira!"
      },
      {
        "id": 8718,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Rein, invista p/ prosperar! Use a\u00e7\u00f5es, t\u00edtulos e fundos futuros hoje e garanta estabilidade amanh\u00e3!"
      },
      {
        "id": 8719,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Rein, investir \u00e9 crucial para crescimento financeiro. Transforma seu dinheiro em mais dinheiro. Aja agora!"
      }
    ]
  },
  {
    "id": 4682,
    "name": "Lucio",
    "account": {
      "id": 4960,
      "number": "xxxx xxxx xxxx 1503",
      "agency": "00003-3",
      "balance": 1000.0,
      "limit": 1000.0
    },
    "card": {
      "id": 4545,
      "number": "xxxx xxxx xxxx 1503",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 8720,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Invista no seu futuro financeiro!"
      },
      {
        "id": 8721,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Lucio, invista seu $ p/ futuro seguro. Diversificar gera renda e protege seu patrim\u00f4nio. Aja j\u00e1!"
      },
      {
        "id": 8722,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Lucio, investir \u00e9 crucial para aumentar seu patrim\u00f4nio e garantir seu futuro financeiro. Vamos come\u00e7ar?"
      }
    ]
  },
  {
    "id": 4684,
    "name": "Dva",
    "account": {
      "id": 4963,
      "number": "xxxx xxxx xxxx 1502",
      "agency": "00002-2",
      "balance": 1000.0,
      "limit": 1000.0
    },
    "card": {
      "id": 4547,
      "number": "xxxx xxxx xxxx 1502",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 8723,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Invista hoje, conquiste um futuro seguro."
      },
      {
        "id": 8724,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Dva, invista bem: aumente sua seguran\u00e7a financeira, maximize retornos e alcance objetivos futuros!"
      },
      {
        "id": 8725,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Dva, investir \u00e9 essencial para multiplicar o dinheiro. Fa\u00e7a o dinheiro trabalhar para voc\u00ea!"
      }
    ]
  },
  {
    "id": 4686,
    "name": "Mei",
    "account": {
      "id": 4965,
      "number": "xxxx xxxx xxxx 1501",
      "agency": "00001-1",
      "balance": 1000.0,
      "limit": 1000.0
    },
    "card": {
      "id": 4549,
      "number": "xxxx xxxx xxxx 1501",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 8726,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Invista hoje, garanta seu amanh\u00e3!"
      },
      {
        "id": 8727,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Investir \u00e9 crucial, Mei. Garante seguran\u00e7a financeira, gera renda adicional e ajuda a atingir objetivos."
      }
    ]
  }
]
```

## Transform
Utilizei a API do OpenAI GPT-4 para gerar uma mensagem de marketing de investimentos bancários personalizado para cada usuário.
```python
!pip install openai
```
### Saída:
```
Collecting openai
  Downloading openai-0.28.1-py3-none-any.whl (76 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 77.0/77.0 kB 2.3 MB/s eta 0:00:00
Requirement already satisfied: requests>=2.20 in /usr/local/lib/python3.10/dist-packages (from openai) (2.31.0)
Requirement already satisfied: tqdm in /usr/local/lib/python3.10/dist-packages (from openai) (4.66.1)
Requirement already satisfied: aiohttp in /usr/local/lib/python3.10/dist-packages (from openai) (3.8.5)
Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (3.3.0)
Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (3.4)
Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (2.0.6)
Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests>=2.20->openai) (2023.7.22)
Requirement already satisfied: attrs>=17.3.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (23.1.0)
Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (6.0.4)
Requirement already satisfied: async-timeout<5.0,>=4.0.0a3 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (4.0.3)
Requirement already satisfied: yarl<2.0,>=1.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (1.9.2)
Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (1.4.0)
Requirement already satisfied: aiosignal>=1.1.2 in /usr/local/lib/python3.10/dist-packages (from aiohttp->openai) (1.3.1)
Installing collected packages: openai
Successfully installed openai-0.28.1
```
```python
# Documentação Oficial da API OpenAI: https://platform.openai.com/docs/api-reference/introduction
# Informações sobre o Período Gratuito: https://help.openai.com/en/articles/4936830

# Para gerar uma API Key:
# 1. Crie uma conta na OpenAI
# 2. Acesse a seção "API Keys"
# 3. Clique em "Create API Key"
# Link direto: https://platform.openai.com/account/api-keys

# Substitua o texto TODO por sua API Key da OpenAI, ela será salva como uma variável de ambiente.
openai_api_key = '###################################'
```
```python
import openai

openai.api_key = openai_api_key

def generate_ai_news(user):
  completion = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[
      {"role": "system", "content": "Você é um especialista em investimentos bacários."},
      {"role": "user", "content": f"crie uma mensagem para {user['name']} sobre a importância dos investimentos (máximo de 100 caracteres)"}
  ]
)
  return completion.choices[0].message.content.strip('\"')

for user in users:
  news = generate_ai_news(user)
  print(news)
  user['news'].append({
      "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
      "description": news
  })
```
### Saída:
```
Investir é crucial, Moira. Garante futuro financeiro estável, gera renda passiva e constrói riqueza.
Rein, invista para assegurar seu futuro. Dinheiro parado perde valor. Cresça seu patrimônio!
Lucio, investir é crucial para crescimento financeiro e segurança futura. Comece hoje!
Dva, investir é crucial para ampliar seu patrimônio e manter sua segurança financeira futura.
Mei, investir é essencial para aumentar sua renda, proteger seu futuro e realizar sonhos. Vamos começar?
```
## Load
Atualizei a lista de "news" de cada usuário na API com a nova mensagem gerada.
```python
def update_user(user):
  response = requests.put(f"{sdw2023_api_url}/users/{user['id']}", json=user)
  return True if response.status_code == 200 else False

for user in users:
  success = update_user(user)
  print(f"User {user['name']} updated? {success}!")
```
### Saída:
```
User Moira updated? True!
User Rein updated? True!
User Lucio updated? True!
User Dva updated? True!
User Mei updated? True!
```
