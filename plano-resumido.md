Para uma aula hands-on, eu ensinaria em dias separados, mas com uma integração final.

Aula 1: Zustand

Objetivo: entender gerenciamento de estado local.

Exemplo prático:

Contador
Tema dark/light
Carrinho de compras
Usuário logado

Conceitos:

Store
State
Actions
Selectors
Tela
 ↓
Zustand Store
 ↓
Atualização da UI


O aluno percebe rapidamente o valor da ferramenta.

Aula 2: TanStack Query

Objetivo: entender estado do servidor.

Exemplo:

Consumir API de produtos
Cache
Loading
Error handling
Invalidação de cache
API
 ↓
TanStack Query
 ↓
Cache
 ↓
Tela


Aqui o aluno aprende que dados da API são diferentes do estado local.

Aula 3: Integração dos dois

Quando os alunos já entenderam os conceitos separadamente.

Exemplo completo:

E-commerce

Zustand:

Carrinho
Favoritos
Preferências do usuário

TanStack Query:

Lista de produtos
Categorias
Busca
Perfil vindo da API
API
 ↓
TanStack Query
 ↓
Produtos

Usuário
 ↓
Zustand
 ↓
Carrinho

O erro comum em sala de aula

Ensinar os dois juntos logo no início.

O aluno acaba confundindo:

O que vai para Zustand?
O que vai para Query?
Quando usar cache?
Quando usar store?

E normalmente surge a pergunta:

"Por que não guardar tudo no Zustand?"

Por isso gosto de criar primeiro a distinção mental:

Regra simples

Zustand → Estado do cliente

Tema
Idioma
Carrinho
Modal aberto
Usuário local


TanStack Query → Estado do servidor

Produtos
Pedidos
Clientes
Dados da API

Estrutura que eu faria como professor

Hands-on 1 (2h)

Introdução ao estado
Context API (15 min)
Problemas do Context
Zustand (1h30)

Hands-on 2 (2h)

Fetch tradicional
Problemas de cache
TanStack Query (1h30)

Hands-on 3 (3h)

Projeto real
Login
Produtos
Carrinho
Integração Zustand + Query

Essa sequência costuma gerar muito menos confusão e faz o aluno entender por que existem duas ferramentas diferentes, em vez de apenas aprender a usá-las.