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
        # Consultar podem ter comentários!
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

Observe que neste exemplo, o campo *friends* retorna uma matriz de itens. As consultas do GraphQL parecem as mesmas para itens únicos ou listas de itens, no entanto, sabemos qual deles é esperado com base no que é indicado no schema.

### Arguments

Se a única coisa que poderíamos fazer era atravessar objetos e seus campos, o GraphQL já seria uma linguagem muito útil para a busca de dados. Mas quando você adiciona a capacidade de passar argumentos para campos, as coisas ficam muito mais interessantes.

```javascript
{
    human(id: "1000") {
        name
        height
    }
}
```

Resultado:

```javascript
{
    "data": {
        "human": {
            "name": "Luke Skywalker",
            "height": 1.72
        }
    }
}
```

Em um sistema como o REST, você só pode passar um único conjunto de argumentos - os parâmetros de consulta e os segmentos de URL em sua solicitação. Mas no GraphQL, cada campo e objeto aninhado pode obter seu próprio conjunto de argumentos, tornando o GraphQL um substituto completo para fazer várias buscas de API. Você pode até mesmo passar argumentos para campos escalares, para implementar transformações de dados uma vez no servidor, em vez de em cada cliente separadamente.

```javascript
{
    human(id: "1000") {
        name
        height(unit: FOOT)
    }
}
```

Resultado:

```javascript
{
    "data": {
        "human": {
            "name": "Luke Skywalker",
            "height": 5.6430448
        }
    }
}
```

Argumentos podem ser de muitos tipos diferentes. No exemplo acima, usamos um tipo Enumeration, que representa um de um conjunto finito de opções (neste caso, unidades de comprimento, METER ou FOOT). O GraphQL vem com um conjunto padrão de tipos, mas um servidor GraphQL também pode declarar seus próprios tipos personalizados, desde que eles possam ser serializados em seu formato de transporte.

[Leia mais sobre o sistema de tipos GraphQL aqui.](schema)

### Aliases

Se você tiver um olhar mais aguçado, você deve ter notado que, como os campos do objeto de resultado correspondem ao nome do campo na consulta, mas não incluem argumentos, não é possível consultar diretamente o mesmo campo com argumentos diferentes. É por isso que você precisa de aliases - eles permitem renomear o resultado de um campo para qualquer coisa que você queira.

```javascript
{
    empireHero: hero(episode: EMPIRE) {
        name
    }
    jediHero: hero(episode: JEDI) {
        name
    }
}
```

Resultado:

```javascript
{
    "data": {
        "empireHero": {
            "name": "Luke Skywalker"
        },
        "jediHero": {
            "name": "R2-D2"
        }
    }
}
```

No exemplo acima, os dois campos hero teriam entrado em conflito, mas como podemos alia-los a nomes diferentes, podemos obter os dois resultados em uma solicitação.