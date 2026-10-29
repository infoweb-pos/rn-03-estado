# Roteiro de apresentação oral — Aula 2

## Tema
NativeWind + React Hook Form + Zod em React Native com Expo.

## Objetivo da aula
Apresentar NativeWind e mostrar como combinar essa ferramenta com React Hook Form e Zod para criar telas de login e cadastro bonitas, organizadas e validadas.

## Estrutura da apresentação

### 1. Abertura (5 min)
- apresentar o tema da aula
- lembrar a turma do que foi visto na aula anterior: formulários, React Hook Form e Zod
- explicar que agora vai entrar a parte visual e de UX

### 2. O problema de UI em React Native sem padronização (5 min)
- mostrar que, sem organização, o código de estilo se torna repetitivo
- explicar que a interface precisa ser consistente e fácil de manter
- introduzir o NativeWind como solução para estilos rápidos e padronizados

### 3. O que é NativeWind (10 min)
- explicar que NativeWind usa classes do Tailwind para React Native
- mostrar o exemplo com `className="bg-slate-100 p-6 rounded-xl"`
- comentar que ele melhora produtividade e legibilidade
- reforçar que NativeWind não substitui a lógica de negócio

### 4. Estrutura do projeto (10 min)
- mostrar a organização em `app/auth/login` e `app/auth/cadastro`
- explicar a ideia de navegação por rotas
- mostrar a tela inicial com botões para entrar ou cadastrar

### 5. Tela de login (15 min)
- apresentar os campos: e-mail, senha e botão de login
- mostrar validação com Zod:
  - e-mail obrigatório
  - e-mail com domínio válido
  - senha mínima de 8 caracteres
- mostrar que o botão só fica habilitado quando o formulário está válido
- explicar que React Hook Form controla os campos e os valores

### 6. Tela de cadastro (15 min)
- apresentar os campos: nome, e-mail, senha, confirmar senha e termos
- explicar as regras mais rígidas
- mostrar que a senha precisa ter maiúscula, minúscula, número e caractere especial
- mostrar que a confirmação precisa coincidir com a senha
- explicar que o `termos` deve ser true para enviar o formulário

### 7. Integração com Zod e RHF (5 min)
- mostrar a ideia central:
  - React Hook Form cuida do formulário
  - Zod define as regras
  - NativeWind cuida da aparência
- reforçar que isso deixa o código mais organizado e escalável

### 8. Boas práticas de design de formulários (5 min)
- bordas consistentes
- espaçamento uniforme
- label clara
- mensagens de erro próximas ao campo
- visual padronizado em todas as telas

### 9. Desafio da aula (10 min)
- pedir que os alunos implementem uma mini aplicação com login e cadastro
- dizer que o objetivo é combinar navegação, validação e styling
- deixar claro os critérios:
  - formularios com validação
  - navegação entre telas
  - design coerente
  - feedback visual para erros

### 10. Encerramento (5 min)
- resumir a aula em 3 pontos:
  - NativeWind organiza visualmente o app
  - React Hook Form gerencia os campos
  - Zod garante regras e validação
- reforçar que isso é uma base muito útil para apps reais

## Frase de fechamento
“Um app bonito e funcional depende tanto da interface quanto da lógica de validação e organização do formulário.”

## Material de apoio
- NativeWind
- React Hook Form
- Zod
- Expo Router
