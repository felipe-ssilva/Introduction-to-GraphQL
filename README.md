# Introdução ao GraphQl

GraphQL é uma linguagem de consulta para sua API e um tempo de execução do lado do servidor para executar consultas usando um sistema de tipos que você define para seus dados. O GraphQL não está vinculado a nenhum banco de dados ou mecanismo de armazenamento específico e, em vez disso, é respaldado pelo código e pelos dados existentes.

Um serviço GraphQL é criado definindo tipos e campos nesses tipos e, em seguida, fornecendo funções para cada campo em cada tipo. Por exemplo, um serviço GraphQL que nos diz quem é o usuário logado (me) e também o nome desse usuário pode ser algo como isto:

```javascript
type Query {
    me: User
}

type User {
    id: ID
    name: String
}
```

Junto com funções para cada campo em cada tipo:

```javascript
function Query_me(request) {
    return request.auth.user;
}

function User_name(user) {
    return user.getName();
}
```

Juntamente com funções para cada campo em cada tipo: Uma vez que um serviço GraphQL está sendo executado (geralmente em uma URL em um serviço da web), ele pode ser enviado consultas GraphQL para validar e executar. Uma consulta recebida é primeiro verificada para garantir que ela se refira apenas aos tipos e campos definidos e, em seguida, executa as funções fornecidas para produzir um resultado.

Por exemplo, a query:

```javascript
{
    me {
        name
    }
}
```

Nos daria o resultado do JSON:

```javascript
{
    "me": {
        "name": "Luke Skywalker"
    }
}
```

[Continue lendo - Queries e Mutations](queries)