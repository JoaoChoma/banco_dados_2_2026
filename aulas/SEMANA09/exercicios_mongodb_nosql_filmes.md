# Exercícios Práticos de NoSQL com MongoDB Atlas

## Tema: StreamFlix — Plataforma de Streaming

Este material apresenta uma base de dados maior para prática com MongoDB e uma lista de exercícios em níveis evolutivos. A proposta é utilizar o MongoDB Atlas, preferencialmente com o VS Code e a extensão **MongoDB for VS Code**, executando os comandos em arquivos `.mongodb`.

---

## 1. Objetivos da prática

Ao final das atividades, o estudante deverá ser capaz de:

- compreender a estrutura de um banco NoSQL orientado a documentos;
- criar bancos e coleções no MongoDB;
- inserir documentos com estruturas flexíveis;
- realizar consultas simples e com operadores;
- manipular arrays e objetos aninhados;
- realizar atualizações com operadores como `$set`, `$inc`, `$push`, `$pull` e `$unset`;
- remover documentos;
- consultar documentos com campos opcionais;
- compreender diferenças iniciais entre modelagem relacional e modelagem NoSQL;
- iniciar o uso de agregações com `aggregate()`.

---

## 2. Banco de dados

Nome do banco:

```javascript
use("streamflix")
```

Coleções utilizadas:

- `usuarios`
- `conteudos`
- `avaliacoes`
- `assinaturas`
- `historico`

---

## 3. Criação da base de dados

Execute os comandos abaixo em um Playground MongoDB no VS Code.

---

## 3.1 Limpar a base antes de começar

```javascript
use("streamflix")

db.usuarios.drop()
db.conteudos.drop()
db.avaliacoes.drop()
db.assinaturas.drop()
db.historico.drop()
```

---

## 3.2 Inserir usuários

```javascript
use("streamflix")

db.usuarios.insertMany([
  {
    nome: "Ana Souza",
    email: "ana@email.com",
    idade: 22,
    cidade: "Curitiba",
    estado: "PR",
    interesses: ["Ficção Científica", "Drama", "Tecnologia"],
    ativo: true,
    endereco: {
      rua: "Rua das Flores",
      numero: 120,
      bairro: "Centro"
    }
  },
  {
    nome: "Carlos Lima",
    email: "carlos@email.com",
    idade: 31,
    cidade: "Maringá",
    estado: "PR",
    interesses: ["Ação", "Aventura", "Suspense"],
    ativo: false,
    endereco: {
      rua: "Avenida Brasil",
      numero: 450,
      bairro: "Zona 7"
    }
  },
  {
    nome: "Fernanda Rocha",
    email: "fernanda@email.com",
    idade: 27,
    cidade: "Londrina",
    estado: "PR",
    interesses: ["Terror", "Suspense", "Drama"],
    ativo: true
  },
  {
    nome: "João Mendes",
    email: "joao@email.com",
    idade: 19,
    cidade: "Curitiba",
    estado: "PR",
    interesses: ["Comédia", "Animação"],
    ativo: true
  },
  {
    nome: "Marina Costa",
    email: "marina@email.com",
    idade: 35,
    cidade: "São Paulo",
    estado: "SP",
    interesses: ["Drama", "Documentário", "História"],
    ativo: true,
    premium: true
  },
  {
    nome: "Rafael Oliveira",
    email: "rafael@email.com",
    idade: 42,
    cidade: "Rio de Janeiro",
    estado: "RJ",
    interesses: ["Crime", "Drama", "Mistério"],
    ativo: false
  },
  {
    nome: "Beatriz Nunes",
    email: "beatriz@email.com",
    idade: 24,
    cidade: "Maringá",
    estado: "PR",
    interesses: ["Romance", "Comédia", "Drama"],
    ativo: true,
    telefone: "4499999-0000"
  },
  {
    nome: "Lucas Ferreira",
    email: "lucas@email.com",
    idade: 29,
    cidade: "Florianópolis",
    estado: "SC",
    interesses: ["Ação", "Ficção Científica", "Animação"],
    ativo: true
  },
  {
    nome: "Patrícia Alves",
    email: "patricia@email.com",
    idade: 38,
    cidade: "Belo Horizonte",
    estado: "MG",
    interesses: ["Documentário", "Biografia"],
    ativo: true
  },
  {
    nome: "Eduardo Martins",
    email: "eduardo@email.com",
    idade: 33,
    cidade: "Curitiba",
    estado: "PR",
    interesses: ["Terror", "Mistério", "Suspense"],
    ativo: true,
    premium: false
  }
])
```

---

## 3.3 Inserir conteúdos

A coleção `conteudos` armazena filmes, séries e documentários. Essa decisão mostra uma característica comum em bancos NoSQL: documentos com estruturas semelhantes, mas não necessariamente idênticas, podem ser armazenados na mesma coleção.

```javascript
use("streamflix")

db.conteudos.insertMany([
  {
    titulo: "Interestelar",
    tipo: "filme",
    ano: 2014,
    generos: ["Drama", "Ficção Científica"],
    avaliacaoMedia: 9.5,
    duracaoMinutos: 169,
    disponivel: true,
    visualizacoes: 2500000,
    diretor: {
      nome: "Christopher Nolan",
      pais: "Reino Unido"
    },
    elenco: ["Matthew McConaughey", "Anne Hathaway", "Jessica Chastain"]
  },
  {
    titulo: "Matrix",
    tipo: "filme",
    ano: 1999,
    generos: ["Ação", "Ficção Científica"],
    avaliacaoMedia: 9.2,
    duracaoMinutos: 136,
    disponivel: true,
    visualizacoes: 3100000,
    diretor: {
      nome: "Lana Wachowski e Lilly Wachowski",
      pais: "Estados Unidos"
    },
    elenco: ["Keanu Reeves", "Laurence Fishburne", "Carrie-Anne Moss"]
  },
  {
    titulo: "Avatar",
    tipo: "filme",
    ano: 2009,
    generos: ["Aventura", "Ficção Científica"],
    avaliacaoMedia: 8.8,
    duracaoMinutos: 162,
    disponivel: true,
    visualizacoes: 2800000,
    diretor: {
      nome: "James Cameron",
      pais: "Canadá"
    }
  },
  {
    titulo: "O Senhor dos Anéis: A Sociedade do Anel",
    tipo: "filme",
    ano: 2001,
    generos: ["Fantasia", "Aventura"],
    avaliacaoMedia: 9.4,
    duracaoMinutos: 178,
    disponivel: true,
    visualizacoes: 2200000,
    diretor: {
      nome: "Peter Jackson",
      pais: "Nova Zelândia"
    },
    premios: ["Oscar", "BAFTA"]
  },
  {
    titulo: "A Origem",
    tipo: "filme",
    ano: 2010,
    generos: ["Ação", "Ficção Científica", "Suspense"],
    avaliacaoMedia: 9.1,
    duracaoMinutos: 148,
    disponivel: true,
    visualizacoes: 1950000,
    diretor: {
      nome: "Christopher Nolan",
      pais: "Reino Unido"
    }
  },
  {
    titulo: "Divertida Mente",
    tipo: "filme",
    ano: 2015,
    generos: ["Animação", "Comédia", "Família"],
    avaliacaoMedia: 8.7,
    duracaoMinutos: 95,
    disponivel: true,
    visualizacoes: 1750000
  },
  {
    titulo: "Cidade de Deus",
    tipo: "filme",
    ano: 2002,
    generos: ["Drama", "Crime"],
    avaliacaoMedia: 9.0,
    duracaoMinutos: 130,
    disponivel: false,
    visualizacoes: 980000,
    diretor: {
      nome: "Fernando Meirelles",
      pais: "Brasil"
    }
  },
  {
    titulo: "Dark",
    tipo: "serie",
    ano: 2017,
    generos: ["Mistério", "Ficção Científica", "Drama"],
    avaliacaoMedia: 9.1,
    temporadas: 3,
    episodios: 26,
    disponivel: true,
    visualizacoes: 2100000,
    classificacao: "16+"
  },
  {
    titulo: "Breaking Bad",
    tipo: "serie",
    ano: 2008,
    generos: ["Drama", "Crime"],
    avaliacaoMedia: 9.8,
    temporadas: 5,
    episodios: 62,
    disponivel: true,
    visualizacoes: 4200000,
    classificacao: "18+"
  },
  {
    titulo: "Stranger Things",
    tipo: "serie",
    ano: 2016,
    generos: ["Ficção Científica", "Terror", "Aventura"],
    avaliacaoMedia: 8.9,
    temporadas: 4,
    episodios: 34,
    disponivel: true,
    visualizacoes: 3900000,
    classificacao: "14+"
  },
  {
    titulo: "The Office",
    tipo: "serie",
    ano: 2005,
    generos: ["Comédia"],
    avaliacaoMedia: 8.8,
    temporadas: 9,
    episodios: 201,
    disponivel: true,
    visualizacoes: 3300000,
    classificacao: "12+"
  },
  {
    titulo: "Planeta Terra",
    tipo: "documentario",
    ano: 2006,
    generos: ["Documentário", "Natureza"],
    avaliacaoMedia: 9.6,
    episodios: 11,
    disponivel: true,
    visualizacoes: 1250000,
    narrador: "David Attenborough"
  },
  {
    titulo: "O Dilema das Redes",
    tipo: "documentario",
    ano: 2020,
    generos: ["Documentário", "Tecnologia", "Sociedade"],
    avaliacaoMedia: 8.2,
    duracaoMinutos: 94,
    disponivel: true,
    visualizacoes: 890000
  },
  {
    titulo: "Senna",
    tipo: "documentario",
    ano: 2010,
    generos: ["Documentário", "Esporte", "Biografia"],
    avaliacaoMedia: 8.6,
    duracaoMinutos: 106,
    disponivel: false,
    visualizacoes: 650000
  },
  {
    titulo: "Parasita",
    tipo: "filme",
    ano: 2019,
    generos: ["Drama", "Suspense"],
    avaliacaoMedia: 9.0,
    duracaoMinutos: 132,
    disponivel: true,
    visualizacoes: 1850000,
    diretor: {
      nome: "Bong Joon-ho",
      pais: "Coreia do Sul"
    },
    premios: ["Oscar"]
  }
])
```

---

## 3.4 Inserir assinaturas

```javascript
use("streamflix")

db.assinaturas.insertMany([
  {
    usuarioEmail: "ana@email.com",
    plano: "Premium",
    valorMensal: 49.90,
    ativo: true,
    formaPagamento: "Cartão de crédito",
    beneficios: ["4 telas", "4K", "download"]
  },
  {
    usuarioEmail: "carlos@email.com",
    plano: "Básico",
    valorMensal: 24.90,
    ativo: false,
    formaPagamento: "Boleto",
    beneficios: ["1 tela", "HD"]
  },
  {
    usuarioEmail: "fernanda@email.com",
    plano: "Padrão",
    valorMensal: 34.90,
    ativo: true,
    formaPagamento: "Pix",
    beneficios: ["2 telas", "Full HD"]
  },
  {
    usuarioEmail: "marina@email.com",
    plano: "Premium",
    valorMensal: 49.90,
    ativo: true,
    formaPagamento: "Cartão de crédito",
    beneficios: ["4 telas", "4K", "download"]
  },
  {
    usuarioEmail: "lucas@email.com",
    plano: "Padrão",
    valorMensal: 34.90,
    ativo: true,
    formaPagamento: "Cartão de débito",
    beneficios: ["2 telas", "Full HD"]
  },
  {
    usuarioEmail: "eduardo@email.com",
    plano: "Básico",
    valorMensal: 24.90,
    ativo: true,
    formaPagamento: "Pix",
    beneficios: ["1 tela", "HD"]
  }
])
```

---

## 3.5 Inserir avaliações

```javascript
use("streamflix")

db.avaliacoes.insertMany([
  {
    usuarioEmail: "ana@email.com",
    tituloConteudo: "Interestelar",
    nota: 10,
    comentario: "Excelente filme, muito emocionante.",
    data: ISODate("2026-03-10")
  },
  {
    usuarioEmail: "carlos@email.com",
    tituloConteudo: "Matrix",
    nota: 9,
    comentario: "Um clássico da ficção científica.",
    data: ISODate("2026-03-11")
  },
  {
    usuarioEmail: "fernanda@email.com",
    tituloConteudo: "Dark",
    nota: 9,
    comentario: "Série complexa e envolvente.",
    data: ISODate("2026-03-12")
  },
  {
    usuarioEmail: "marina@email.com",
    tituloConteudo: "Planeta Terra",
    nota: 10,
    comentario: "Documentário visualmente impressionante.",
    data: ISODate("2026-03-13")
  },
  {
    usuarioEmail: "lucas@email.com",
    tituloConteudo: "Stranger Things",
    nota: 8,
    comentario: "Boa mistura de aventura e suspense.",
    data: ISODate("2026-03-14")
  },
  {
    usuarioEmail: "beatriz@email.com",
    tituloConteudo: "The Office",
    nota: 9,
    comentario: "Muito divertida.",
    data: ISODate("2026-03-15")
  },
  {
    usuarioEmail: "eduardo@email.com",
    tituloConteudo: "Parasita",
    nota: 10,
    comentario: "Roteiro excelente.",
    data: ISODate("2026-03-16")
  },
  {
    usuarioEmail: "patricia@email.com",
    tituloConteudo: "O Dilema das Redes",
    nota: 8,
    comentario: "Importante para refletir sobre tecnologia.",
    data: ISODate("2026-03-17")
  }
])
```

---

## 3.6 Inserir histórico de visualização

```javascript
use("streamflix")

db.historico.insertMany([
  {
    usuarioEmail: "ana@email.com",
    tituloConteudo: "Interestelar",
    tipo: "filme",
    progressoPercentual: 100,
    finalizado: true,
    dataVisualizacao: ISODate("2026-04-01")
  },
  {
    usuarioEmail: "ana@email.com",
    tituloConteudo: "Dark",
    tipo: "serie",
    temporada: 1,
    episodio: 3,
    progressoPercentual: 45,
    finalizado: false,
    dataVisualizacao: ISODate("2026-04-03")
  },
  {
    usuarioEmail: "carlos@email.com",
    tituloConteudo: "Matrix",
    tipo: "filme",
    progressoPercentual: 100,
    finalizado: true,
    dataVisualizacao: ISODate("2026-04-02")
  },
  {
    usuarioEmail: "fernanda@email.com",
    tituloConteudo: "Stranger Things",
    tipo: "serie",
    temporada: 2,
    episodio: 5,
    progressoPercentual: 80,
    finalizado: false,
    dataVisualizacao: ISODate("2026-04-05")
  },
  {
    usuarioEmail: "marina@email.com",
    tituloConteudo: "Planeta Terra",
    tipo: "documentario",
    progressoPercentual: 100,
    finalizado: true,
    dataVisualizacao: ISODate("2026-04-06")
  },
  {
    usuarioEmail: "lucas@email.com",
    tituloConteudo: "Avatar",
    tipo: "filme",
    progressoPercentual: 60,
    finalizado: false,
    dataVisualizacao: ISODate("2026-04-07")
  },
  {
    usuarioEmail: "beatriz@email.com",
    tituloConteudo: "The Office",
    tipo: "serie",
    temporada: 3,
    episodio: 10,
    progressoPercentual: 100,
    finalizado: true,
    dataVisualizacao: ISODate("2026-04-08")
  },
  {
    usuarioEmail: "eduardo@email.com",
    tituloConteudo: "Parasita",
    tipo: "filme",
    progressoPercentual: 100,
    finalizado: true,
    dataVisualizacao: ISODate("2026-04-09")
  },
  {
    usuarioEmail: "patricia@email.com",
    tituloConteudo: "Senna",
    tipo: "documentario",
    progressoPercentual: 30,
    finalizado: false,
    dataVisualizacao: ISODate("2026-04-10")
  }
])
```

---

# 4. Exercícios evolutivos

---

## Nível 1 — Primeiros contatos com documentos e coleções

### Exercício 1
Liste todos os documentos da coleção `usuarios`.

### Exercício 2
Liste todos os documentos da coleção `conteudos`.

### Exercício 3
Liste todos os usuários da cidade de `Curitiba`.

### Exercício 4
Liste todos os conteúdos do tipo `filme`.

### Exercício 5
Busque o conteúdo cujo título é `Matrix`.

### Exercício 6
Insira um novo usuário na coleção `usuarios` com os campos:

- nome;
- email;
- idade;
- cidade;
- estado;
- interesses;
- ativo.

### Exercício 7
Insira um novo conteúdo do tipo `filme` com os campos:

- título;
- tipo;
- ano;
- gêneros;
- avaliação média;
- duração em minutos;
- disponível.

---

## Nível 2 — Operadores de comparação

### Exercício 8
Liste os conteúdos com avaliação média maior que `9`.

### Exercício 9
Liste os usuários com idade maior que `30`.

### Exercício 10
Liste os conteúdos lançados antes do ano `2010`.

### Exercício 11
Liste os conteúdos lançados a partir de `2015`.

### Exercício 12
Liste os conteúdos cuja avaliação média seja menor ou igual a `8.8`.

### Exercício 13
Liste os usuários que não são do estado `PR`.

---

## Nível 3 — Consultas com arrays

### Exercício 14
Liste os conteúdos que possuem o gênero `Drama`.

### Exercício 15
Liste os conteúdos que possuem o gênero `Ficção Científica`.

### Exercício 16
Liste os conteúdos que possuem os gêneros `Drama` e `Crime` ao mesmo tempo.

### Exercício 17
Liste os usuários que possuem interesse em `Suspense`.

### Exercício 18
Liste os conteúdos que possuem pelo menos um dos seguintes gêneros:

- `Terror`
- `Mistério`

### Exercício 19
Liste os conteúdos que não possuem o gênero `Comédia`.

---

## Nível 4 — Objetos aninhados

### Exercício 20
Liste os conteúdos dirigidos por `Christopher Nolan`.

### Exercício 21
Liste os conteúdos cujo diretor é do `Reino Unido`.

### Exercício 22
Liste os usuários cujo bairro seja `Centro`.

### Exercício 23
Liste os usuários que possuem o campo `endereco`.

### Exercício 24
Liste os usuários que não possuem o campo `endereco`.

---

## Nível 5 — Atualizações básicas

### Exercício 25
Atualize o usuário `Carlos Lima` para que o campo `ativo` passe a ser `true`.

### Exercício 26
Atualize o conteúdo `Cidade de Deus` para que o campo `disponivel` passe a ser `true`.

### Exercício 27
Adicione o campo `idiomaOriginal` ao filme `Matrix`, com o valor `Inglês`.

### Exercício 28
Adicione o campo `classificacao` ao filme `Interestelar`, com o valor `10+`.

### Exercício 29
Atualize a avaliação média de `Avatar` para `9.0`.

---

## Nível 6 — Atualizações com operadores

### Exercício 30
Incremente em `1` a quantidade de visualizações do conteúdo `Matrix`.

### Exercício 31
Incremente em `1000` a quantidade de visualizações de todos os conteúdos disponíveis.

### Exercício 32
Adicione o gênero `Clássico` ao filme `Matrix`.

### Exercício 33
Remova o gênero `Clássico` do filme `Matrix`.

### Exercício 34
Remova o campo `telefone` da usuária `Beatriz Nunes`.

### Exercício 35
Adicione o benefício `sem anúncios` aos usuários do plano `Premium` na coleção `assinaturas`.

---

## Nível 7 — Operadores lógicos

### Exercício 36
Liste os conteúdos que são filmes e possuem avaliação média maior que `9`.

### Exercício 37
Liste os usuários que são de `Curitiba` ou de `Maringá`.

### Exercício 38
Liste os conteúdos que são séries ou documentários.

### Exercício 39
Liste os conteúdos que possuem avaliação maior que `9` e visualizações acima de `2000000`.

### Exercício 40
Liste os usuários ativos com idade menor que `30`.

---

## Nível 8 — Campos opcionais e flexibilidade NoSQL

### Exercício 41
Liste os conteúdos que possuem o campo `premios`.

### Exercício 42
Liste os conteúdos que não possuem o campo `diretor`.

### Exercício 43
Liste os usuários que possuem o campo `premium`.

### Exercício 44
Liste os conteúdos que possuem o campo `temporadas`.

### Exercício 45
Explique por que os documentos da coleção `conteudos` podem ter campos diferentes.

---

## Nível 9 — Remoção de documentos

### Exercício 46
Remova o usuário que você criou no Exercício 6.

### Exercício 47
Remova o conteúdo que você criou no Exercício 7.

### Exercício 48
Remova todas as avaliações com nota menor que `8`.

### Exercício 49
Remova os registros de histórico cujo progresso seja menor que `40`.

---

## Nível 10 — Consultas em coleções diferentes

### Exercício 50
Liste todas as assinaturas ativas.

### Exercício 51
Liste todas as assinaturas do plano `Premium`.

### Exercício 52
Liste o histórico de visualização do usuário `ana@email.com`.

### Exercício 53
Liste os conteúdos finalizados no histórico.

### Exercício 54
Liste as avaliações com nota igual a `10`.

### Exercício 55
Liste os usuários que possuem assinatura ativa.  
Observação: neste momento, resolva consultando separadamente as coleções `usuarios` e `assinaturas`.

---

## Nível 11 — Introdução à agregação

### Exercício 56
Conte quantos documentos existem na coleção `conteudos`.

### Exercício 57
Conte quantos conteúdos existem por tipo:

- filme;
- série;
- documentário.

### Exercício 58
Calcule a média das avaliações médias dos conteúdos.

### Exercício 59
Liste os conteúdos ordenados pela avaliação média em ordem decrescente.

### Exercício 60
Liste os cinco conteúdos com maior número de visualizações.

### Exercício 61
Calcule a média das notas da coleção `avaliacoes`.

### Exercício 62
Conte quantas avaliações cada conteúdo recebeu.

### Exercício 63
Conte quantos usuários existem por estado.

---

## Nível 12 — Desafio de modelagem NoSQL

### Exercício 64
Crie uma nova coleção chamada `perfis_completos`.

Cada documento deve representar um usuário com suas informações principais, assinatura e histórico de visualização em um único documento.

Exemplo de estrutura esperada:

```javascript
{
  nome: "Ana Souza",
  email: "ana@email.com",
  idade: 22,
  assinatura: {
    plano: "Premium",
    ativo: true,
    valorMensal: 49.90
  },
  historico: [
    {
      tituloConteudo: "Interestelar",
      progressoPercentual: 100,
      finalizado: true
    },
    {
      tituloConteudo: "Dark",
      temporada: 1,
      episodio: 3,
      progressoPercentual: 45,
      finalizado: false
    }
  ]
}
```

### Exercício 65
Explique a diferença entre manter as informações separadas em várias coleções e armazenar tudo em um único documento.

### Exercício 66
Explique uma vantagem e uma desvantagem de usar documentos aninhados no MongoDB.

### Exercício 67
Explique em quais situações seria melhor usar referência entre coleções.

### Exercício 68
Explique em quais situações seria melhor usar dados incorporados no mesmo documento.

---

# 5. Sugestão de organização em aulas

## Aula 1
- Conceitos de NoSQL;
- documentos;
- coleções;
- inserção;
- consultas simples;
- exercícios 1 a 13.

## Aula 2
- arrays;
- objetos aninhados;
- operadores de existência;
- exercícios 14 a 24.

## Aula 3
- atualização;
- remoção;
- operadores lógicos;
- exercícios 25 a 49.

## Aula 4
- múltiplas coleções;
- histórico;
- assinaturas;
- exercícios 50 a 55.

## Aula 5
- agregações iniciais;
- modelagem NoSQL;
- embedding versus referencing;
- exercícios 56 a 68.

---

# 6. Comandos úteis de referência

## Listar todos os documentos

```javascript
db.conteudos.find()
```

## Filtrar por igualdade

```javascript
db.conteudos.find({ titulo: "Matrix" })
```

## Maior que

```javascript
db.conteudos.find({ avaliacaoMedia: { $gt: 9 } })
```

## Menor que

```javascript
db.conteudos.find({ ano: { $lt: 2010 } })
```

## Buscar em array

```javascript
db.conteudos.find({ generos: "Drama" })
```

## Buscar com `$in`

```javascript
db.conteudos.find({ generos: { $in: ["Terror", "Mistério"] } })
```

## Buscar com `$all`

```javascript
db.conteudos.find({ generos: { $all: ["Drama", "Crime"] } })
```

## Buscar campo aninhado

```javascript
db.conteudos.find({ "diretor.nome": "Christopher Nolan" })
```

## Verificar existência de campo

```javascript
db.conteudos.find({ diretor: { $exists: true } })
```

## Atualizar campo

```javascript
db.conteudos.updateOne(
  { titulo: "Avatar" },
  { $set: { avaliacaoMedia: 9.0 } }
)
```

## Incrementar valor

```javascript
db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $inc: { visualizacoes: 1 } }
)
```

## Adicionar item em array

```javascript
db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $push: { generos: "Clássico" } }
)
```

## Remover item de array

```javascript
db.conteudos.updateOne(
  { titulo: "Matrix" },
  { $pull: { generos: "Clássico" } }
)
```

## Remover campo

```javascript
db.usuarios.updateOne(
  { nome: "Beatriz Nunes" },
  { $unset: { telefone: "" } }
)
```

## Remover documento

```javascript
db.conteudos.deleteOne({ titulo: "Matrix" })
```

## Agregação simples: agrupar por tipo

```javascript
db.conteudos.aggregate([
  {
    $group: {
      _id: "$tipo",
      total: { $sum: 1 }
    }
  }
])
```

## Ordenar por avaliação

```javascript
db.conteudos.find().sort({ avaliacaoMedia: -1 })
```

---

# 7. Observação final

Esta base foi construída para evidenciar características importantes do MongoDB:

- documentos com estruturas flexíveis;
- campos opcionais;
- arrays;
- objetos aninhados;
- coleções relacionadas por valores como `email` e `titulo`;
- possibilidade de modelagem incorporada ou referenciada.

Esses elementos ajudam a discutir, desde o início, que NoSQL não significa ausência de organização, mas sim uma forma diferente de modelar dados conforme o padrão de acesso da aplicação.
