Excelente tema para uma disciplina de React Native. Eu estruturaria os 3 encontros como uma evolução natural do conceito de estado local → estado remoto → arquitetura moderna.

Hands-on 1: Gerenciamento de Estado com Zustand

Duração: 2 horas

Objetivos

Ao final da aula o aluno será capaz de:

Entender o problema do prop drilling
Criar uma store global
Manipular estado global
Consumir dados da store em múltiplas telas
Aplicar boas práticas com Zustand
Parte 1 - Motivação (15 min)
Exemplo inicial sem Zustand

Criar:

App
 └─ Home
     └─ ProductList
         └─ ProductCard


Passar um contador por props.

Demonstrar:

count
setCount


atravessando vários componentes.

Pergunta aos alunos:

O que acontece quando temos 20 componentes?

Introduzir o problema do prop drilling.

Parte 2 - Primeiro Store (30 min)

Criar projeto:

npx create-expo-app zustand-demo
npm install zustand


Estrutura:

src/
 ├── store/
 │   └── counterStore.ts
 ├── screens/
 └── components/


Store:

import { create } from 'zustand'

export const useCounterStore = create((set) => ({
  count: 0,

  increment: () =>
    set((state) => ({
      count: state.count + 1
    })),

  decrement: () =>
    set((state) => ({
      count: state.count - 1
    }))
}))

Parte 3 - Consumindo Estado Global (20 min)

Tela:

const count = useCounterStore((state) => state.count)
const increment = useCounterStore((state) => state.increment)


Mostrar:

Reatividade
Atualização automática
Simplicidade
Parte 4 - Mini Projeto Carrinho (40 min)

Criar:

Produtos
↓
Adicionar
↓
Carrinho


Store:

items:[]
addItem()
removeItem()
clearCart()


Funcionalidades:

Adicionar produto
Remover produto
Total de itens
Parte 5 - Desafio (15 min)

Implementar:

✅ Favoritos

toggleFavorite(id)

Hands-on 2: TanStack Query

Duração: 2 horas

Objetivos

Ao final da aula o aluno será capaz de:

Consumir APIs
Trabalhar com cache
Entender loading e error
Refetch automático
Invalidação de cache
Parte 1 - Problema do Fetch Tradicional (20 min)

Mostrar:

useEffect(() => {
 fetchProducts()
}, [])


Problemas:

Repetição
Cache inexistente
Estados manuais
loading
error
data

Parte 2 - Instalação (15 min)
npm install @tanstack/react-query


Configuração:

<QueryClientProvider>

Parte 3 - Primeiro Query (30 min)

API Fake Store:

https://fakestoreapi.com/products


Criar:

useQuery()


Exibir:

Loading
Erro
Dados

Parte 4 - Cache na Prática (20 min)

Demonstrar:

staleTime


Mostrar:

Primeira requisição
Segunda navegação
Dados vindo do cache

Discussão:

Quando vale usar cache?

Parte 5 - Mutações (20 min)

Criar cadastro fictício:

useMutation()


Após criar:

invalidateQueries()


Mostrar atualização automática.

Parte 6 - Desafio (15 min)

Criar tela:

Categorias


Consumindo nova rota da API.

Hands-on 3: Projeto Completo Integrando Zustand + TanStack Query

Duração: 3 horas

Cenário

Aplicativo Marketplace

Login
Produtos
Carrinho
Perfil

Objetivos

Entender claramente:

Zustand

Estado do Cliente

Carrinho
Tema
Usuário logado

Query

Estado do Servidor

Produtos
Categorias
Pedidos
Perfil

Etapa 1 - Estrutura do Projeto (20 min)
src/
 ├── api/
 ├── store/
 ├── hooks/
 ├── screens/
 ├── components/
 └── navigation/


Instalar:

npm install zustand
npm install @tanstack/react-query
npm install @react-navigation/native

Etapa 2 - Listagem de Produtos (40 min)

Query:

useQuery({
  queryKey: ['products']
})


Tela:

Produto
Preço
Imagem

Etapa 3 - Carrinho com Zustand (40 min)

Store:

addProduct()
removeProduct()
clearCart()


Fluxo:

Produto
 ↓
Adicionar
 ↓
Carrinho


Discussão:

Por que isso NÃO deve ficar no React Query?

Resposta:

Porque é estado da interface do usuário.

Etapa 4 - Busca de Produtos (30 min)

Filtro:

searchTerm


Guardar no Zustand.

Explicar:

Estado local
≠
Estado remoto

Etapa 5 - Atualização de Perfil (30 min)

Mutation:

useMutation()


Após salvar:

invalidateQueries()


Atualizar:

Perfil


automaticamente.

Etapa 6 - Arquitetura Final (20 min)

Desenhar com os alunos:

              API
               │
               ▼
       TanStack Query
               │
       Produtos/Perfil
               │
               ▼
             UI

--------------------------

          Zustand
               │
     Carrinho/Temas
               │
               ▼
             UI

Exercício Final (Casa ou Avaliação)

Criar um aplicativo de delivery contendo:

Zustand
Carrinho
Endereço selecionado
Forma de pagamento
TanStack Query
Restaurantes
Cardápio
Pedidos
Perfil
Bônus
AsyncStorage
Persistência do Zustand
npm install @react-native-async-storage/async-storage

Resultado pedagógico

Ao final dos três encontros o aluno terá construído o seguinte raciocínio:

Context API resolve problemas simples
Zustand resolve estado global do cliente
TanStack Query resolve estado vindo do servidor
As duas ferramentas não competem
Aplicações modernas usam ambas juntas, cada uma para um propósito diferente

Esse é exatamente o modelo arquitetural usado na maior parte dos projetos React/React Native modernos hoje.