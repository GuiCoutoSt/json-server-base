# vaccine-my-pet-api 🐕

Seja bem-vindo(a) à my-pet-API. Essa API te o objetivo de cadastrar seu pet e registrar sua vacinação, para acompanhar a saúde e bem estar do seu amiguinho.

# URL Base 🔗

`https://vaccine-my-pet-api.herokuapp.com/`

# Endpoints 🔚

Essa API possuí 4 endpoints: `register`, `login`, `animals` e `vaccines`. É possível cadastrar usuários, realizar login, cadastrar animais e quais vacinas foram aplicadas. A listagem dos usuário e das vacinas necessita de autenticação. Já a de animais pode ser visualizada sem autenticação.

# Rotas que não precisam de autenticação 🔓

## Listando Animais 🐶

Para listar animais basta fazer uma requisição `GET` no endpoint `/animals`. Essa requisição retornará a lista de todos os animais registrados no sistema. 

`GET /animals - FORMATO DA RESPOSTA - STATUS 200`

```json
[
    {
        "name": "Barth",
    	"specie": "Dog",
    	"breed": "Undefined",
    	"userId": 1,
    	"id": 1
    }
]
```

## Cadastro de Usuários 👤

Para cadastrar um usuário é necessário realizar uma requisição `POST` com o endpoint `/register`.

O `body` da requisição deve conter os seguintes campos, sendo obrigatórios o email e a senha:

`POST /register - FORMATO DA REQUISIÇÃO`

```json
{
    "email": "jhondoe@mail.com",
    "password": "******",
    "name": "Jhon Doe",
}
```

Caso corra tudo bem, esse é o formato da resposta:

`POST /register - FORMATO DA RESPOSTA - STATUS 201`

```json
{
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Impob25kb2VAbWFpbC5jb20iLCJpYXQiOjE2NDIwOTI0NTMsImV4cCI6MTY0MjA5NjA1Mywic3ViIjoiMSJ9.f5wwehC5t5ER2dgmA-o560MjcOhzyyLxG7XrDvjwSuQ",
        "user": {
	    "email": "jhondoe@mail.com",
	    "name": "Jhon Doe",
            "id": 1
}
```

### Possíveis Erros 💣

`POST /register - FORMATO DA RESPOSTA - STATUS 400`

```
"Email and password are required"
```

`POST /register - FORMATO DA RESPOSTA - STATUS 400`

```
"Email already exists"
```

`POST /register - FORMATO DA RESPOSTA - STATUS 400`

```
"Email format is invalid"
```

## Login 💻

Alguns endpoints exigem que o usuário esteja logado. Essa é a estrutura base do `body` da requisição:

`POST /login FORMATO DA REQUISIÇÃO`

```json
{
    "email": "jhondoe@mail.com",
    "password": "123456"
}
```

Se tudo der certo, essa é a resposta esperada:

`POST /login FORMATO DA RESPOSTA - STATUS 201`

```json
{
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Impob25kb2VAbWFpbC5jb20iLCJpYXQiOjE2NDIxMDA5NDEsImV4cCI6MTY0MjEwNDU0MSwic3ViIjoiMSJ9.kayLOprKoebmHalz5vb7mFj7zQ39-QrCVZTc8FEgbu4",
        "user": {
	    "email": "jhondoe@mail.com",
            "name": "Jhon Doe",
            "id": 1
        }
}
```

### Possíveis Erros 💣

`POST /register - FORMATO DA RESPOSTA - STATUS 400`

```
"Cannot find user"
```

`POST /register - FORMATO DA RESPOSTA - STATUS 400`

```
"Incorrect password"
```

`POST /register - FORMATO DA RESPOSTA - STATUS 400`

```
"Email and password are required"
```

# Rotas que necessitam de autenticação 🔒

Essas requisições necessitam do Bearer Token do usuário para serem efetuadas!

## Cadastrando Animais 🐯

Para cadastrar os animais, é necessário que o usuário esteja logado. Entretanto, qualquer um pode requisitar a listagem dos animais. O `body` da requisição tem o seguinte formato:

`POST /animals - FORMATO DA REQUISIÇÂO`

```json
{
    "name": "Barth",
    "specie": "Dog",
    "breed": "Undefined",
    "userId": 1,
}
```

`POST /animals - FORMATO DA RESPOSTA - STATUS 201`

```json
{
    "name": "Bethoven",
    "specie": "Dog",
    "breed": "Undefined",
    "userID": 1,
    "id": 2
}
```

## Cadastrando Vacinas 💉

O usuário só pode cadastrar vacinas se for o dono do animal. Dessa forma é necessário informar o `userId` do usuário que registrou o animal. O `body` da requisição deve seguir esse formato:

`POST /vaccines - FORMATO DA REQUISIÇÃO` 

```json
{
    "name": "Parvovirus",
    "vaccination-year": "2021",
    "animalId": 1,
    "userId": 1,
    "id": 1
}
```

`POST /vaccines - FORMATO DA RESPOSTA - STATUS 201`

```json
{
    "name": "Rabies",
    "vaccination-year": "2021",
    "animalId": 1,
    "userId": 1,
    "id": 2
}
```

### Possíveis Erros 💣

`POST /vaccine - FORMATO DA RESPOSTA - STATUS 400`

```
"Private resource creation: request body must have a reference to the owner id"
```

## Listando Vacinas 💉

Para listar as vacinas o usuário deve estar logado.

`GET /vaccines - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
      "name": "Parvovirus",
      "vaccination-year": "2021",
      "animalId": 1,
      "userId": 1,
      "id": 1
  },
  {
      "name": "Rabies",
      "vaccination-year": "2021",
      "animalId": 1,
      "userId": 1,
      "id": 2
  }
]
```

### Possíveis Erros 💣

`GET /vaccines - FORMATO DA RESPOSTA - STATUS 401`

```
"Missing authorization header"
```

# Query Params 🗄️

Para mais informações sobre os query params que podem ser utilizados nesta aplicação, visite a biblioteca do [JSON-Server](https://github.com/typicode/json-server#routes).
