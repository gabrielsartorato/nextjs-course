# NextJS

## Criar um novo projeto

- yarn create next-app **nome-projeto**.

## Adicionar TypeScript

- yarn add typescript @types/react @types/node -D.

## Páginas e Rotas

Dentro da pasta pages, cada vez que criamos um novo arquivo com extensão .tsx ele automaticamente vira uma nova página, criando uma nova pasta (ex: catalog e um arquivo product) podemos fazer um submenu.

### Rotas Dinâmicas

Para a criação de uma rota dinâmica, antes da extênção .tsx, devemos colocar [] e dentro dele o nome do parâmetro que deseja ser enviado, ex: [id].tsx

## Requisições

- Client Sidde Fetching: usada quando a página não precisa ser indexada aos motores de busca, usado normalmente axios.
  
- Server Side Rendering: usada quando à página precisa ser indexada totalmente, ex:
- Somente ser utilizada quando for necessária os dados serem utilizados para os motores de busca.
  
```js
  export const getServerSideProps: GetServerSideProps<HomeProps> = async () => {
    const response = await fetch('http://localhost:3333/recommended');
    const recommendedProducts = await response.json();

    return {
      props: {
        recommendedProducts,
      },
    };
  };
```

- Static Site Generation: utilizado quando a página é totalmente statica, não tendo alterações, exemplo de uso seria as postagens de um blog.
- Usando a função o revalidate a página será utilizada

```js
  export const getStaticProps: GetStaticProps<Top10Props> = async (context) => {
  const response = await fetch('http://localhost:3333/products');
  const products = await response.json();

  return {
    props: {
      products,
    },
    revalidate: 5,
  };
};
```

- Páginas estáticas dinâmicas

A função getStaticPaths é utilizada quando necessitamos que às página sejam estáticas e ao mesmo tempo dinâicas, ele faz sua chamada a API armazenando os dados em memória.

fallback é utilizado para tentar gerar após a build do projeto uma nova página ainda não renderizada.

Quando o fallback é utilizado, é necessário usar uma condicional do useRouter para que caso o primeiro usuário utilize uma nova página e ela não esteja pronta, seja mostrado um aviso ou carregando, ex:

```js
  if (router.isFallback) {
      return <p>Carregando...</p>;
  }
```

```js
export const getStaticPaths: GetStaticPaths = async () => {
  const response = await fetch(`http://localhost:3333/categories`);
  const categories = await response.json();

  const paths = categories.map((category) => {
    return {
      params: { slug: category.id },
    };
  });

  return {
    paths,
    fallback: false,
  };
};
```

## Imports Dinâmicos

- Usados quando não a necessidade de se fazer o import da lib toda a todo momento, somente quando ele for utilizado ele será importado.

```js
import dynamic from 'next/dynamic';

// Componentes
const AddToCartModal = dynamic(
  () => import('../../../components/AddToCartModal'),
  { loading: () => <p>Carregando...</p> }
);

```

## Váriaves de ambiente

Para se usar devemos cria o arquivo .env.developement, esse arquivo irá para o github e afins para o time de desenvolvimento. Caso queira usar uma secreta devemos usar dentro de um arquivo .env.local.

- Podemos deixar a variavél de ambiente pública usando o prefixo NEXT_PUBLIC_NOME_VARAIVEL

## Paths TypeScript

Dentro do tsconfig.json adicionar as seguintes linhas em compilerOptions:

```js
"baseUrl": "./src",
    "paths": {
      "@/*": ["./*"]
    }
```

## MetaTags

```js
<meta httpEquiv="x-ua-compatible" content="IE=edge,chrome=1" />
<meta name="MobileOptimized" content="320" />
<meta name="HandheldFriendly" content="True" />
<meta name="theme-color" content="#121214" />
<meta name="msapplication-TileColor" content="#121214" />
<meta name="referrer" content="no-referrer-when-downgrade" />
<meta name="google" content="notranslate" />

<meta property="og:title" content={pageTitle} />
<meta property="og:description" content={description} />
<meta property="og:locale" content="pt_BR" />
<meta property="og:type" content="website" />
<meta property="og:site_name" content={pageTitle} />
<meta property="og:image" content={pageImage} />
<meta property="og:image:secure_url" content={pageImage} />
<meta property="og:image:alt" content="Thumbnail" />
<meta property="og:image:type" content="image/png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />

<meta name="twitter:title" content={pageTitle} />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@rocketseat" />
<meta name="twitter:creator" content="@rocketseat" />
<meta name="twitter:image" content={pageImage} />
<meta name="twitter:image:src" content={pageImage} />
<meta name="twitter:image:alt" content="Thumbnail" />
<meta name="twitter:image:width" content="1200" />
<meta name="twitter:image:height" content="620" />
```

## Custom Document

Usado para fazer alterações em fontes, fav icon etc

Dentro do _document, inserir as seguintes informaçẽos

```js
import Document, {
  DocumentContext,
  Html,
  Head,
  Main,
  NextScript,
} from 'next/document';
import { ServerStyleSheet } from 'styled-components';

render() {
    return (
      <Html lang="pt">
        <Head>
          <meta charSet="utf-8" />

          <link
            href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap"
            rel="stylesheet"
          />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
```
