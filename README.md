# json-server-base

Esse é o repositório com a base de JSON-Server + JSON-Server-Auth já configurada, feita para ser usada no desenvolvimento das API's.

## Endpoints

Assim como a documentação do JSON-Server-Auth traz (https://www.npmjs.com/package/json-server-auth), existem 3 endpoints que podem ser utilizados para cadastro e 2 endpoints que podem ser usados para login.

### Cadastro

POST /register(default) <br/>
POST /signup <br/>
POST /users

Qualquer um desses 3 endpoints irá cadastrar o usuário na lista de "Users", sendo que os campos obrigatórios são os de email e password.
Você pode ficar a vontade para adicionar qualquer outra propriedade no corpo do cadastro dos usuários.


### Login

POST /login(default) <br/>
POST /signin

Qualquer um desses 2 endpoints pode ser usado para realizar login com um dos usuários cadastrados na lista de "Users"
#json-server-t13-eCOMPANY-Friendly

<h1 align="center">
   ♻️ eCOMPANY Friendly - API ♻️
</h1>

<p align = "center">
Este é o backend da aplicação eCOMPANY Friendly - Um grupo que é amigo das empresas que querem ajudar o planeta e investir em um desenvolvimento sustentável, utilizando materiais que não seriam reciclados, através de doações.
</p>

<blockquote align="center">“Reduza, reuse e recicle.”</blockquote>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

## **Endpoints**

A API tem um total de 11 endpoints - podendo cadastrar seu perfil e produtos que deseja descartar para que outras empresas possam coletar e reutilizar. <br/>

baseUrl da API: https://json-server-ecompany.herokuapp.com

## Essas rotas necessitam de autenticação

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

Após o usuário estar logado, ele deve conseguir visualizar os descartes publicados na aplicação, revindicar descarte, publicar descarte, editar e deletar publicação de descarte.

Nessa aplicação o usuário após fazer o login ou se cadastrar pode ver a empresa e os produtos já cadastradas na plataforma, na API podemos acessar a lista dessa forma:

##
## Rota para buscar usuário, e seus produtos cadastrados:

`GET /users/userId - FORMATO DA RESPOSTA - STATUS 200`


```json
{
	"email": "ecompany@mail.com",
	"password": "$2a$10$9eFY2.uKjJc2LCRUimNcT.DekRbBfWC9XnWA1ZnnSnIzY9Iso45DW",
	"name": "ECOmpany friendly",
	"tellphone": "1195090-9009",
	"id": 3,
	"products": [
                 {
                  "userId": 3,
                  "name": "Geladeira",
                  "type": "Metal",
                  "weight": null,
                  "city": "São Paulo",
                  "country": "São Paulo",
                  "image": null,
                  "id": 1
                },
                {
                  "userId": 3,
                  "name": "Fogão",
                  "type": "Metal",
                  "weight": null,
                  "city": "Minas Gerais",
                  "country": "Belo Horizonte",
                  "image": null,
                  "id": 2
                },
                {
                  "userId": 3,
                  "status": "Disponível",
                  "name": "Material Descarte",
                  "type": "Metal",
                  "weight": null,
                  "city": "Curitba",
                  "country": "Paraná",
                  "image": null,
                  "id": 4
                },
                {
                  "userId": 3,
                  "status": "Disponível",
                  "name": "Material Descarte",
                  "type": "Metal",
                  "weight": null,
                  "city": "Curitba",
                  "country": "Paraná",
                  "image": null,
                  "id": 5
                }
	]
}
```

Podemos utilizar os query params para mudar a lista, mudando a paginação, podemos alterar quantos usuários queremos no perPage, e alterar a página no parâmetro page. Uma requisição apenas no /users irá trazer 15 usuários na página 1.
Com o parâmetro tech, podemos filtrar por tecnologia.

`GET /users?perPage=15&page=1&tech=React - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    A páginação não está funcionando, ainda estou verificando.  
  }
]
```

Lembrando que no cabeçalho da resposta, temos as informações sobre a paginação, e o nextUrl para acessar a próxima página.

Cabeçalho da resposta:

> nextUrl: https://kenziehub.herokuapp.com/users?perPage=15&page=2 <br/>
> page: 1 <br/>
> perPage: 15

##
## Rota para fazer buscas pelo município e cidade:

`GET /products - FORMATO DA RESPOSTA - STATUS 200`

```json
[
	{
		"userId": 3,
		"name": "Fogão",
		"type": "Metal",
		"weight": null,
		"city": Minas Gerais,
		"country": Belo Horizonte,
		"image": null,
		"id": 2
	},
	{
		"userId": 1,
		"name": "Fogão",
		"type": "Metal",
		"weight": null,
		"city": Minas Gerais,
		"country": Belo Horizonte,
		"image": null,
		"id": 3
	}
]
```

##
<h2 align ='center'> Criação de usuário </h2>

##
## Rota para criação de usuário:

`POST /register - FORMATO DA REQUISIÇÃO`

```json
{
	"name": "ECOmpany friendly",
	"email": "ecompany@mail.com",
	"image": "",
	"password": "123456",
	"tellphone": "1195090-9009"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /users - FORMATO DA RESPOSTA - STATUS 201`

```json
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImVjb21wYW55QG1haWwuY29tIiwiaWF0IjoxNjY3MTU1MDY1LCJleHAiOjE2NjcxNTg2NjUsInN1YiI6IjMifQ.XDnQoDrme9a8ADg0SJvId_DgqqBejlsNCk3O1-KLufE",
	"user": {
		"email": "ecompany@mail.com",
		"name": "ECOmpany friendly",
		"tellphone": "1195090-9009",
		"id": 3
	}
}
```

Email já cadastrado:

`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": "Email already exists"
}
```

##
<h2 align = "center"> Login </h2>

##
## Rota para fazer login na aplicação:

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "ecompany@mail.com",
	"password": "123456"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImVjb21wYW55QG1haWwuY29tIiwiaWF0IjoxNjY3MjI4MDg3LCJleHAiOjE2NjcyMzE2ODcsInN1YiI6IjMifQ.KnnFd2z6ilNUhiOcEBnew9Tr50YfAV0yvmYqkHmWOTw",
	"user": {
		"email": "ecompany@mail.com",
		"name": "ECOmpany friendly",
		"tellphone": "1195090-9009",
		"id": 3
	}
}
```

Com essa resposta, vemos que temos duas informações, o userId e o token respectivo.

##
<h2 align ='center'> Buscar Perfil do usuário logado (id) </h2>

##
## Rota para buscar produtos sem o usuário:

`GET /users - FORMATO DA REQUISIÇÃO`

<blockquote>Na requisição apenas é necessário o userId, a aplicação ficará responsável em buscar e retornar o usuário.</blockquote>

<br>

`GET /users/idUser/products - FORMATO DA RESPOSTA - STATUS 200`

```json
	{
		"userId": 3,
		"name": "Geladeira",
		"type": "Metal",
		"weight": null,
		"city": "São Paulo",
		"country": "São Paulo",
		"image": null,
		"id": 1
	},
	{
		"userId": 3,
		"name": "Fogão",
		"type": "Metal",
		"weight": null,
		"city": "Minas Gerais",
		"country": "Belo Horizonte",
		"image": null,
		"id": 2
	},
	{
		"userId": 3,
		"name": "Material Descarte",
		"type": "Metal",
		"weight": null,
		"city": "Curitba",
		"country": "Paraná",
		"image": "",
		"id": 4
	}
```

##
## Rota para buscar produtos pelo tipo:

`GET /products - FORMATO DA REQUISIÇÃO`

```json
	{
		"userId": 3,
		"name": "Material Descarte",
		"type": "Plastico",
		"weight": null,
		"city": "Curitba",
		"country": "Paraná",
		"image": "",
		"id": 4,
		"user": {
			"email": "ecompany@mail.com",
			"password": "$2a$10$9eFY2.uKjJc2LCRUimNcT.DekRbBfWC9XnWA1ZnnSnIzY9Iso45DW",
			"name": "ECOmpany friendly",
			"tellphone": "1195090-9009",
			"id": 3
		}
	}
```

<blockquote>Na requisição apenas é necessário o userId, a aplicação ficará responsável em buscar e retornar o usuário.</blockquote>

<br>

`GET /users/idUser/products - FORMATO DA RESPOSTA - STATUS 200`

##
<h2 align ='center'> Publicar descarte disponível </h2>

## Rota para publicar:

`POST /products - FORMATO DA REQUISIÇÃO`

```json
{
	"userId": 3,
	"status": true,
	"name": "Material Descarte",
	"type": "Madeira",
	"weight": null,
	"city": "Curitba",
	"country": "Paraná",
	"image": null
}
```

##
## Rota para deletar:

`DELETE /users/products/id`

```
Não é necessário um corpo da requisição.
```

##
<h2 align ='center'> Atualizando publicações do perfil </h2>

Estes endpoints devem ser usados com o token no cabeçalho da requisição e, são para atualizar seus dados como, foto da publicação, nome, ou qualquer outra informação em relação ao que foi utilizado na criação da publicação do descarte.

Endpoint para atualizar a foto de perfil:

`PATCH /products/id - FORMATO DA REQUISIÇÃO`

```multipart
image: <Arquivo de imagem>
```

---

Feito com 💚 by equipe eCOMPANY Friendly :wave:

