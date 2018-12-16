# Queries e Mutations

Nesta página, você aprenderá em detalhes sobre como consultar um servidor GraphQL.

### Fields

Em sua forma mais simples, o GraphQL é sobre a solicitação de fields específicos em objetos. Vamos começar olhando para uma consulta muito simples e o resultado que obtemos quando a rodamos:

```javascript
{
    hero {
        name
    }
}
```

Resultado:

```javascript
{
    "data": {
        "hero": {
            "name": "R2-D2"
        }
    }
}
```

Você pode ver imediatamente que a consulta tem exatamente a mesma forma que o resultado. Isso é essencial para o GraphQL, porque você sempre recebe de volta o que espera, e o servidor sabe exatamente quais campos o cliente está pedindo.

O nome do campo retorna uma String, nesse caso o nome do herói principal de Star Wars, "R2-D2".

> Ah, mais uma coisa - a consulta acima é interativa. Isso significa que você pode mudá-la como quiser e ver o novo resultado. Tente adicionar um nov campo ao objeto na consulta e veja o novo resultado.

No exemplo anterior, apenas pedimos o nome de nosso herói que retornou uma String, mas os campos também podem se referir a Objects. Nesse caso, você pode fazer uma sub-seleção de campos para esse objeto. As consultas GraphQL podem percorrer os objetos relacionados e seus campos, permitindo que os clientes obtenham muitos dados relacionados em uma solicitação, em vez de fazer várias viagens de ida e volta, como seria necessário em uma arquitetura REST clássica.

```javascript
{
    hero {
        name
        # Queries can have comments!
        friends {
            name
        }
    }
}
```

Resultado:

```javascript
{
    "data": {
        "hero": {
            "name": "R2-D2",
            "friends":[
                {
                    "name": "Luke Skywalker"
                },
                {
                    "name": "Han Solo"
                },
                {
                    "name": "Leia Organa"
                }
            ]
        }
    }
}
```