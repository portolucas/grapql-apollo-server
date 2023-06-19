# Apollo GraphQL

Olá! Meu nome é Lucas, sou professor da pós-graduação da **PUC Minas**. Este é um projeto para os alunos da pós-graduação.

# Start

Vamos começar pela abordagem GraphQL.

Vamos utilizar o Apollo Server. 

Crie um diretório:

```
mkdir graphql-server-example cd graphql-server-example
```
Inicie o projeto Node sinalizando que vamos usar ESModules, o que simplifica nossos exemplos e nos permite usar arquivos `await`.
```
npm init --yes && npm pkg set type="module"
```
Vamos instalar as dependências do Apollo e do GraphQL:
```
npm install @apollo/server graphql
```

## Files and folders

Crie um arquivo na raiz do projeto chamado **index.js**. 
Primeiro, vamos importar nossas bibliotecas:
```
import  {  ApolloServer  }  from  "@apollo/server";
import  {  startStandaloneServer  }  from  "@apollo/server/standalone";
```
Agora, vamos simular uma base de dados:
```
const books = [
  {
    title: "The Awakening",
    author: "Kate Chopin",
  },
  {
    title: "City of Glass",
    author: "Paul Auster",
  },
];
```
Com uma base de dados, podemos inferir as definições de tipo.
```
const typeDefs = `#graphql

  type Book {
    title: String
    author: String
  }

  type Query {
    books: [Book]
  }
`;
```
E então, criar os *resolvers* para, inicialmente, recuperar os nossos dados com base nas definições de tipos:
```
const resolvers = {
  Query: {
    books: () => books,
  },
};
```
Finalmente, podemos iniciar o nosso servidor Apollo:
```
const server = new ApolloServer({
  typeDefs,
  resolvers,
});

const { url } = await startStandaloneServer(server, {
  listen: { port: 4000 },
});

console.log(`🚀  Server ready at: ${url}`);
```
Acesse o endereço do servidor e execute uma query. 

Agora vamos adicionar *Mutation* para adicionar dados a nossa base fictícia.
Vamos criar uma definição de tipo para a nossa mutation.
```
// adicione dentro do typeDefs
 type Mutation {
    addBook(title: String, author: String): Book
  }
```
Agora podemos utilizar essa mutation no nosso resolver:
```
// adicione dentro do resolvers
 Mutation: {
    addBook: (_, { title, author }) => {
      const book = { title, author };
      books.push(book);
      return book;
    },
  },
  ```
  Reinicie o servidor e execute uma mutation. Você pode notar que um novo objeto foi adicionado à nossa lista de livros.


