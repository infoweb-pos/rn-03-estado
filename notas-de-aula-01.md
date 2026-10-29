# Notas de aula 01 - React Hook Form + Zod no React Native com Expo

## Aula 1 (60 min + 30 min de desafio)

### Tema
Criando formulários robustos e validados no React Native usando React Hook Form e Zod.

### Público
Estudantes de Desenvolvimento Mobile/Web com conhecimentos básicos de:
- React
- Componentes React Native
- useState
- TypeScript (básico)

### Objetivos da aula
Ao final da aula, o estudante será capaz de:
- Entender os problemas de formulários feitos apenas com `useState`
- Criar formulários com React Hook Form
- Validar dados utilizando Zod
- Exibir mensagens de erro para o usuário
- Integrar React Hook Form e Zod
- Implementar validações mais complexas

---

## 1) Contexto da aula

Formulários são parte central de quase toda aplicação mobile. Quando começamos a construir um cadastro, login, perfil ou checkout, logo percebemos que `useState` sozinho começa a apresentar problemas:

- muito estado para cada campo
- validação espalhada por vários `if`
- dificuldades em lidar com erros por campo
- atualização manual da UI
- código repetitivo e difícil de manter

Exemplo clássico:

```tsx
const [nome, setNome] = useState('');
const [email, setEmail] = useState('');
const [senha, setSenha] = useState('');

const handleSubmit = () => {
  if (!nome.trim()) {
    alert('Nome obrigatório');
    return;
  }

  if (!email.includes('@')) {
    alert('E-mail inválido');
    return;
  }

  if (senha.length < 6) {
    alert('Senha curta');
    return;
  }
};
```

Esse padrão funciona, mas cresce rapidamente e vira um problema quando o formulário fica maior.

A solução: combinar:
- `react-hook-form` → gestão do formulário
- `zod` → schema e validação
- `@hookform/resolvers` → integração entre RHF e Zod

---

## 2) Por que React Hook Form?

React Hook Form ajuda a:
- registrar cada campo automaticamente
- acompanhar valores e erros
- validar com menos código manual
- reduzir re-renderizações desnecessárias
- manter o formulário organizado

Ele abstrai grande parte do boilerplate que normalmente escrevemos no React Native.

### Vantagens principais
- menos `useState` para campos
- melhor organização do formulário
- validação centralizada
- fácil integração com TypeScript
- integração excelente com bibliotecas de validação como Zod

---

## 3) Por que Zod?

Zod é uma biblioteca para validação de dados com schemas. Ela permite descrever a estrutura esperada e validar o valor antes do submit.

Exemplo:

```ts
import { z } from 'zod';

const usuarioSchema = z.object({
  nome: z.string().min(2, 'Nome deve ter pelo menos 2 caracteres'),
  email: z.string().email('E-mail inválido'),
  senha: z.string().min(6, 'Senha deve ter no mínimo 6 caracteres'),
});
```

Com isso, a aplicação tem uma regra clara: "se o objeto não bater com o schema, não avança".

---

## 4) Instalação

No projeto Expo, a instalação costuma ser:

```bash
npx expo install react-hook-form zod @hookform/resolvers
```

Se o projeto já estiver com React Native + Expo e TypeScript, essa é a base mínima.

---

## 5) Cenário prático: Cadastro de Usuário

Vamos construir um formulário com 3 campos obrigatórios:
- Nome
- E-mail
- Senha

### Regras iniciais
- nome: obrigatório, mínimo de 2 caracteres
- email: obrigatório, formato válido
- senha: mínimo de 6 caracteres

### Estrutura do exemplo

```tsx
// App.tsx
import React from 'react';
import { View, Text, TextInput, Button, ScrollView, Alert } from 'react-native';
import { useForm, Controller } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const usuarioSchema = z.object({
  nome: z.string().min(2, 'Nome deve ter pelo menos 2 caracteres'),
  email: z.string().email('Digite um e-mail válido'),
  senha: z.string().min(6, 'A senha deve ter pelo menos 6 caracteres'),
});

type UsuarioFormData = z.infer<typeof usuarioSchema>;

export default function App() {
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm<UsuarioFormData>({
    resolver: zodResolver(usuarioSchema),
    defaultValues: {
      nome: '',
      email: '',
      senha: '',
    },
  });

  const onSubmit = (data: UsuarioFormData) => {
    Alert.alert('Cadastro realizado', JSON.stringify(data, null, 2));
  };

  return (
    <ScrollView contentContainerStyle={{ padding: 24, gap: 16 }}>
      <Text style={{ fontSize: 24, fontWeight: '700' }}>Cadastro</Text>

      <View>
        <Text>Nome</Text>
        <Controller
          control={control}
          name="nome"
          render={({ field: { onChange, onBlur, value } }) => (
            <TextInput
              value={value}
              onChangeText={onChange}
              onBlur={onBlur}
              placeholder="Digite seu nome"
              style={{
                borderWidth: 1,
                borderColor: errors.nome ? 'red' : '#ccc',
                padding: 12,
                borderRadius: 8,
                marginTop: 8,
              }}
            />
          )}
        />
        {errors.nome && (
          <Text style={{ color: 'red', marginTop: 4 }}>{errors.nome.message}</Text>
        )}
      </View>

      <View>
        <Text>E-mail</Text>
        <Controller
          control={control}
          name="email"
          render={({ field: { onChange, onBlur, value } }) => (
            <TextInput
              value={value}
              onChangeText={onChange}
              onBlur={onBlur}
              placeholder="seu@email.com"
              keyboardType="email-address"
              autoCapitalize="none"
              style={{
                borderWidth: 1,
                borderColor: errors.email ? 'red' : '#ccc',
                padding: 12,
                borderRadius: 8,
                marginTop: 8,
              }}
            />
          )}
        />
        {errors.email && (
          <Text style={{ color: 'red', marginTop: 4 }}>{errors.email.message}</Text>
        )}
      </View>

      <View>
        <Text>Senha</Text>
        <Controller
          control={control}
          name="senha"
          render={({ field: { onChange, onBlur, value } }) => (
            <TextInput
              value={value}
              onChangeText={onChange}
              onBlur={onBlur}
              placeholder="Digite sua senha"
              secureTextEntry
              style={{
                borderWidth: 1,
                borderColor: errors.senha ? 'red' : '#ccc',
                padding: 12,
                borderRadius: 8,
                marginTop: 8,
              }}
            />
          )}
        />
        {errors.senha && (
          <Text style={{ color: 'red', marginTop: 4 }}>{errors.senha.message}</Text>
        )}
      </View>

      <Button title="Cadastrar" onPress={handleSubmit(onSubmit)} />
    </ScrollView>
  );
}
```

### Explicação

- `useForm` cria o estado do formulário
- `Controller` conecta cada input ao react-hook-form
- `resolver: zodResolver(usuarioSchema)` integra o schema com o formulário
- `errors.nome`, `errors.email`, `errors.senha` disparam os erros
- `handleSubmit(onSubmit)` só executa se o schema validar

---

## 6) Fluxo de validação

A validação acontece em 3 etapas:

1. O usuário digita
2. O hook registra os valores
3. O schema do Zod valida os dados antes do submit

Se algo estiver inválido:
- o campo recebe erro
- a mensagem aparece
- `handleSubmit` não chama `onSubmit`

Isso melhora bastante a experiência do usuário e reduz lógica manual na aplicação.

---

## 7) Validações mais ricas

A maioria dos formulários reais exige mais do que somente "campo obrigatório".

### Exemplos comuns
- senha com no mínimo 8 caracteres
- pelo menos uma letra maiúscula
- pelo menos um número
- confirmação de senha
- termos aceitos
- e-mail com domínio válido

### Schema mais completo

```ts
const cadastroSchema = z
  .object({
    nome: z.string().min(2, 'Nome deve ter pelo menos 2 caracteres'),
    email: z.string().email('E-mail inválido'),
    senha: z
      .string()
      .min(8, 'Senha deve ter pelo menos 8 caracteres')
      .regex(/[A-Z]/, 'A senha deve ter pelo menos uma letra maiúscula')
      .regex(/[0-9]/, 'A senha deve ter pelo menos um número'),
    confirmarSenha: z.string(),
    termos: z.literal(true, {
      errorMap: () => ({ message: 'Você precisa aceitar os termos' }),
    }),
  })
  .refine((data) => data.senha === data.confirmarSenha, {
    message: 'As senhas não conferem',
    path: ['confirmarSenha'],
  });
```

### O que está acontecendo

- `z.regex` valida padrões específicos
- `z.literal(true)` exige que o checkbox tenha sido marcado
- `.refine` compara dois campos

Esse padrão é muito útil para regras dependentes de mais de um campo.

---

## 8) Como o aluno deve pensar ao aprender

A ideia central é:

- `react-hook-form` cuida do formulário
- `zod` cuida da regra
- o componente apenas renderiza dados e erros

Em vez de misturar validação com UI, o código fica mais legível e escalável.

---

## 9) Dica pedagógica para aula

O professor pode mostrar o problema comparando duas versões do mesmo formulário:

### Versão 1: `useState` manual
- o aluno percebe que cada campo precisa ser tratado individualmente
- a lógica fica dispersa
- a validação e a interface se misturam

### Versão 2: `React Hook Form + Zod`
- o aluno percebe que o formulário fica mais declarativo
- os erros ficam mais fáceis de controlar
- a lógica de validação fica centralizada e reutilizável

Essa comparação costuma gerar grande entendimento do valor da ferramenta.

---

## 10) Projeto exemplo para a aula

### Nome do projeto
`CadastroUsuarioApp`

### Objetivo
Criar um formulário de cadastro com validações reais em React Native com Expo.

### Estrutura sugerida
```bash
app/
  _layout.tsx
  index.tsx
```

ou, se usando estrutura simples:

```bash
App.tsx
```

### Campos
- Nome
- E-mail
- Senha
- Confirmar senha
- Aceitar termos

### Regras adicionais
- senha mínima de 8 caracteres
- senha deve conter letra maiúscula e número
- confirmar senha deve ser igual à senha
- aceitar termos é obrigatório

---

## 11) Sugestão de roteiro de aula (60 min)

### 0-10 min — Introdução
- problemas de formulários com `useState`
- quando a lógica começa a ficar complexa
- motivação para usar formulários mais robustos

### 10-20 min — React Hook Form
- `useForm`
- `Controller`
- `handleSubmit`
- `formState.errors`

### 20-35 min — Zod
- schema
- `z.object`
- `z.string`, `z.email`, `z.min`, `z.regex`
- integração com `zodResolver`

### 35-50 min — Montagem do formulário
- implementar o exemplo de cadastro
- mostrar validações em tempo real e no submit
- exibir mensagens de erro

### 50-60 min — Demo e discussão
- testar cenários válidos e inválidos
- revisão dos conceitos
- reflexões sobre manutenção do código

---

## 12) Desafio (30 min)

### Desafio: Cadastro completo com regras extras

Implemente a versão com os seguintes campos:
- nome
- e-mail
- senha
- confirmarSenha
- termos

### Regras
- nome obrigatório e com mínimo de 2 caracteres
- e-mail válido
- senha com mínimo de 8 caracteres
- senha deve conter ao menos 1 maiúscula
- senha deve conter ao menos 1 número
- confirmarSenha deve ser igual à senha
- termos deve ser aceito

### Exemplo de estrutura final do schema

```ts
const schema = z
  .object({
    nome: z.string().min(2, 'Nome obrigatório'),
    email: z.string().email('E-mail inválido'),
    senha: z
      .string()
      .min(8, 'A senha deve ter pelo menos 8 caracteres')
      .regex(/[A-Z]/, 'A senha deve ter ao menos uma letra maiúscula')
      .regex(/[0-9]/, 'A senha deve ter ao menos um número'),
    confirmarSenha: z.string(),
    termos: z.boolean().refine((value) => value === true, {
      message: 'Você precisa aceitar os termos de uso',
    }),
  })
  .refine((data) => data.senha === data.confirmarSenha, {
    message: 'As senhas não conferem',
    path: ['confirmarSenha'],
  });
```

### Critérios de sucesso
- formulário não envia quando a validação falha
- mensagens aparecem abaixo do campo correto
- confirmação de senha é validada
- termos precisam ser marcados
- dados válidos disparam envio com sucesso

### Dica de implementação
Utilizar `CheckBox` ou `Switch` para o termo de uso e `Controller` para controlar esse campo com React Hook Form.

---

## 13) Observações importantes para o professor

- enfatizar que o `useState` não é errado, mas não é a melhor ferramenta para formulários complexos
- mostrar que `zod` melhora a qualidade do código ao tornar a regra explícita
- discutir a diferença entre validação no cliente e validação no backend
- reforçar que essa validação do front-end é boa para UX, mas não substitui validação no servidor

---

## 14) Conclusão

Nesta aula, os alunos aprenderam:
- a limitação do `useState` em formulários maiores
- como usar React Hook Form para organizar o formulário
- como aplicar regras com Zod
- como mostrar mensagens de erro de forma clara ao usuário
- como construir um cadastro robusto em React Native com Expo

Essa combinação é muito útil em aplicações reais, especialmente em cadastro, autenticação, perfil do usuário e checkout.

---

## 15) Material de apoio para o professor

### Bibliografia recomendada
- https://react-hook-form.com/
- https://zod.dev/
- https://docs.expo.dev/

### Perguntas de reflexão
- Por que `useState` sozinho fica mais difícil em formulários grandes?
- Qual é o papel de cada biblioteca nesta stack?
- O que acontece se a validação só existir no front-end?
- Como a UX melhora quando os erros são mostrados no campo certo?

---

## 16) Tarefa complementar

Peça aos alunos para refatorar um formulário simples de login para usar React Hook Form + Zod, adicionando:
- e-mail obrigatório
- senha obrigatória
- mensagem de erro personalizada
- botão de envio desabilitado enquanto o formulário for inválido

Essa tarefa amadurece o conceito de validação e leitura do schema como fonte de verdade do formulário.

---

## 17) Resumo rápido para apresentação

React Hook Form + Zod = formulário organizado + validação declarativa + melhor UX.

"O formulário deixa de ser um monte de estado manual e passa a ser um schema bem definido."

---

## 18) Emoji de aula (opcional para slides)

- 🧠 Entendendo o problema
- 🧩 Organizando o formulário
- ✅ Validando com Zod
- 🚨 Exibindo erros
- 🛡️ Protegendo o usuário


