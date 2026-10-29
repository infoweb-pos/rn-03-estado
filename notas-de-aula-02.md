# Notas de aula 02 - NativeWind + React Hook Form + Zod no React Native com Expo

## Aula 2 (60 min + 30 min de desafio)

### Tema
Introduzir ao NativeWind com formulários de cadastro de usuários e de login em React Native com Expo.

### Stack
- Expo
- TypeScript
- React Navigation
- React Hook Form
- Zod
- NativeWind

### Objetivo
Introduzir ao NativeWind com formulários de cadastro de usuários e de login em React Native com Expo.

Ao final desta aula, o estudante será capaz de:
- compreender a proposta do NativeWind para estilização em React Native
- aplicar classes utilitárias para interfaces mais rápidas e consistentes
- criar telas de login e cadastro com layout moderno
- integrar NativeWind com React Hook Form e Zod
- aplicar validações de formulário para UX melhorada
- compreender a organização de telas por rotas em um app Expo

---

## 1) Contexto da aula

Até este ponto, a turma já viu como formularios podem ficar difíceis quando usamos apenas `useState` e como React Hook Form + Zod ajudam a centralizar e validar regras. Agora, precisamos pensar em uma segunda parte importante do desenvolvimento mobile: a aparência e a experiência visual.

Uma interface bem-feita precisa:
- ser legível
- ter espaçamento consistente
- reduzir esforço visual repetitivo
- permitir velocidade de desenvolvimento
- manter a base fácil de evoluir

É aí que entra o NativeWind.

---

## 2) O que é NativeWind?

NativeWind é uma biblioteca que permite escrever estilos em React Native usando classes do Tailwind CSS.

Em vez de escrever algo como:

```tsx
<View style={{ padding: 24, backgroundColor: '#fff', borderRadius: 12 }} />
```

podemos escrever:

```tsx
<View className="p-6 bg-white rounded-xl" />
```

### Vantagens do NativeWind
- produtividade maior
- padronização visual
- classes consistentes e reutilizáveis
- melhor legibilidade
- boa integração com apps React Native

### Atenção
O NativeWind não substitui a lógica de negócio. Ele é uma solução de estilização. A validação continua sendo responsabilidade do React Hook Form + Zod.

---

## 3) Instalação

Em um projeto Expo, a instalação geralmente é feita com:

```bash
npx expo install nativewind
```

E depois, conforme a configuração do projeto, pode ser necessário:

```bash
npm install -D tailwindcss
```

Em projetos mais recentes, o setup pode exigir algumas configurações extras no `tailwind.config.js` e no `babel.config.js`.

### Dica para aula
O professor pode mostrar que NativeWind é uma extensão do Tailwind para React Native, mas não é o Tailwind web completo em CSS.

---

## 4) Estrutura sugerida da aplicação

A seguir, uma organização simples e adequada para ensinar o conceito de telas separadas de autenticação.

```bash
app/
  _layout.tsx
  index.tsx
  /auth/
    login/
      index.tsx
    cadastro/
      index.tsx
```

### Objetivo da estrutura
- separar a tela inicial da aplicação
- criar rotas específicas para login e cadastro
- manter a organização lógica por funcionalidade
- demonstrar a ideia de navegação em aplicação mobile

---

## 5) Fluxo de navegação

A tela inicial pode ter botões para navegar para:
- Login
- Cadastro

```tsx
import { Link } from 'expo-router';

export default function HomeScreen() {
  return (
    <View className="flex-1 items-center justify-center bg-slate-100 p-6">
      <Text className="text-2xl font-bold text-slate-900">Bem-vindo</Text>

      <Link href="/auth/login" asChild>
        <Pressable className="mt-6 w-full rounded-xl bg-blue-600 p-4">
          <Text className="text-center text-white font-semibold">Entrar</Text>
        </Pressable>
      </Link>

      <Link href="/auth/cadastro" asChild>
        <Pressable className="mt-4 w-full rounded-xl border border-blue-600 p-4">
          <Text className="text-center text-blue-600 font-semibold">Criar conta</Text>
        </Pressable>
      </Link>
    </View>
  );
}
```

### Conceito que pode ser enfatizado em aula
A navegação por rotas organiza o app em módulos e facilita a manutenção.

---

## 6) Tela de login

### Campos do login
- E-mail
- Senha
- botão de login

### Regras de validação do login
- email: obrigatório, formato válido
- email: com domínio válido
- senha: mínima de 8 caracteres
- botão de login ativado quando email e senha ok

### Exemplo de schema Zod

```ts
import { z } from 'zod';

export const loginSchema = z.object({
  email: z.string().email('Digite um e-mail válido').refine(
    (value) => value.includes('@') && value.includes('.'),
    'E-mail com domínio inválido'
  ),
  senha: z.string().min(8, 'A senha deve ter pelo menos 8 caracteres'),
});
```

### Observação importante
A regra `email com domínio válido` pode ser implementada como uma validação adicional ou como uma regra mais específica de domínio, dependendo do nível da turma.

### Exemplo de componente

```tsx
import { View, Text, TextInput, Pressable, Alert } from 'react-native';
import { Controller, useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const loginSchema = z.object({
  email: z.string().email('Digite um e-mail válido').refine(
    (value) => value.includes('@') && value.includes('.'),
    'E-mail com domínio inválido'
  ),
  senha: z.string().min(8, 'A senha deve ter pelo menos 8 caracteres'),
});

type LoginFormData = z.infer<typeof loginSchema>;

export default function LoginScreen() {
  const {
    control,
    handleSubmit,
    formState: { errors, isValid },
  } = useForm<LoginFormData>({
    resolver: zodResolver(loginSchema),
    mode: 'onChange',
    defaultValues: {
      email: '',
      senha: '',
    },
  });

  const onSubmit = (data: LoginFormData) => {
    Alert.alert('Login realizado', JSON.stringify(data));
  };

  return (
    <View className="flex-1 justify-center bg-slate-100 px-6">
      <Text className="text-3xl font-bold text-slate-900">Entrar</Text>

      <View className="mt-6 space-y-4">
        <View>
          <Text className="mb-2 text-sm font-medium text-slate-700">E-mail</Text>
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
                className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
              />
            )}
          />
          {errors.email && (
            <Text className="mt-1 text-sm text-red-500">{errors.email.message}</Text>
          )}
        </View>

        <View>
          <Text className="mb-2 text-sm font-medium text-slate-700">Senha</Text>
          <Controller
            control={control}
            name="senha"
            render={({ field: { onChange, onBlur, value } }) => (
              <TextInput
                value={value}
                onChangeText={onChange}
                onBlur={onBlur}
                secureTextEntry
                placeholder="Digite sua senha"
                className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
              />
            )}
          />
          {errors.senha && (
            <Text className="mt-1 text-sm text-red-500">{errors.senha.message}</Text>
          )}
        </View>

        <Pressable
          onPress={handleSubmit(onSubmit)}
          disabled={!isValid}
          className={
            isValid
              ? 'mt-6 rounded-xl bg-blue-600 p-4'
              : 'mt-6 rounded-xl bg-blue-300 p-4'
          }
        >
          <Text className="text-center font-semibold text-white">Entrar</Text>
        </Pressable>
      </View>
    </View>
  );
}
```

### Ponto importante para a aula
O botão de login pode ficar desabilitado enquanto o formulário não estiver válido. Isso melhora a UX e ensina a ideia de validação ativa.

---

## 7) Tela de cadastro do usuário

### Campos do cadastro do usuário
- Nome
- E-mail
- Senha
- Confirmar senha
- Aceitar termos

### Regras de validação do cadastro
- nome: obrigatório, mínimo de 2 caracteres
- email: obrigatório, formato válido
- email: com domínio válido
- senha: mínima de 8 caracteres
- senha: deve conter letra maiúscula, letra minúscula, número e caracteres especiais
- confirmar senha deve ser igual à senha
- aceitar termos é obrigatório

### Schema principal do cadastro

```ts
import { z } from 'zod';

export const cadastroSchema = z
  .object({
    nome: z.string().min(2, 'Nome obrigatório'),
    email: z.string().email('E-mail inválido').refine(
      (value) => value.includes('@') && value.includes('.'),
      'E-mail com domínio inválido'
    ),
    senha: z
      .string()
      .min(8, 'A senha deve ter pelo menos 8 caracteres')
      .regex(/[A-Z]/, 'A senha deve ter ao menos uma letra maiúscula')
      .regex(/[a-z]/, 'A senha deve ter ao menos uma letra minúscula')
      .regex(/[0-9]/, 'A senha deve ter ao menos um número')
      .regex(/[^A-Za-z0-9]/, 'A senha deve ter ao menos um caractere especial'),
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

### Observação didática
Essa validação mostra que Zod pode validar tanto regras simples quanto regras mais complexas que envolvem múltiplos campos.

---

## 8) Exemplo de formulário com NativeWind

```tsx
import { View, Text, TextInput, Pressable, Switch } from 'react-native';
import { Controller, useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { cadastroSchema } from './schema';

export default function CadastroScreen() {
  const {
    control,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(cadastroSchema),
    defaultValues: {
      nome: '',
      email: '',
      senha: '',
      confirmarSenha: '',
      termos: false,
    },
  });

  return (
    <View className="flex-1 bg-slate-50 px-6 py-8">
      <Text className="text-3xl font-bold text-slate-900">Crie sua conta</Text>

      <View className="mt-6 space-y-4">
        <View>
          <Text className="mb-2 text-sm font-medium text-slate-700">Nome</Text>
          <Controller
            control={control}
            name="nome"
            render={({ field: { onChange, onBlur, value } }) => (
              <TextInput
                value={value}
                onChangeText={onChange}
                onBlur={onBlur}
                placeholder="Digite seu nome"
                className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
              />
            )}
          />
          {errors.nome && <Text className="mt-1 text-sm text-red-500">{errors.nome.message}</Text>}
        </View>

        <View>
          <Text className="mb-2 text-sm font-medium text-slate-700">E-mail</Text>
          <Controller
            control={control}
            name="email"
            render={({ field: { onChange, onBlur, value } }) => (
              <TextInput
                value={value}
                onChangeText={onChange}
                onBlur={onBlur}
                keyboardType="email-address"
                autoCapitalize="none"
                placeholder="seu@email.com"
                className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
              />
            )}
          />
          {errors.email && <Text className="mt-1 text-sm text-red-500">{errors.email.message}</Text>}
        </View>

        <View>
          <Text className="mb-2 text-sm font-medium text-slate-700">Senha</Text>
          <Controller
            control={control}
            name="senha"
            render={({ field: { onChange, onBlur, value } }) => (
              <TextInput
                value={value}
                onChangeText={onChange}
                onBlur={onBlur}
                secureTextEntry
                placeholder="Digite sua senha"
                className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
              />
            )}
          />
          {errors.senha && <Text className="mt-1 text-sm text-red-500">{errors.senha.message}</Text>}
        </View>

        <View>
          <Text className="mb-2 text-sm font-medium text-slate-700">Confirmar senha</Text>
          <Controller
            control={control}
            name="confirmarSenha"
            render={({ field: { onChange, onBlur, value } }) => (
              <TextInput
                value={value}
                onChangeText={onChange}
                onBlur={onBlur}
                secureTextEntry
                placeholder="Confirme sua senha"
                className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
              />
            )}
          />
          {errors.confirmarSenha && (
            <Text className="mt-1 text-sm text-red-500">{errors.confirmarSenha.message}</Text>
          )}
        </View>

        <View className="mt-2 flex-row items-center justify-between rounded-xl border border-slate-200 bg-white p-3">
          <Text className="text-sm text-slate-700">Aceito os termos de uso</Text>
          <Controller
            control={control}
            name="termos"
            render={({ field: { value, onChange } }) => (
              <Switch value={value} onValueChange={onChange} />
            )}
          />
        </View>

        {errors.termos && (
          <Text className="text-sm text-red-500">{errors.termos.message}</Text>
        )}

        <Pressable
          onPress={handleSubmit((data) => console.log(data))}
          className="mt-6 rounded-xl bg-sky-600 p-4"
        >
          <Text className="text-center text-base font-semibold text-white">Cadastrar</Text>
        </Pressable>
      </View>
    </View>
  );
}
```

### Comentário didático
Esse exemplo ensina duas coisas importantes ao mesmo tempo:
- NativeWind deixa o layout limpo e legível
- os campos continuam integrados à validação do Zod e ao controle do React Hook Form

---

## 9) Boas práticas de UI para formulários

Ao ensinar NativeWind + formulários, vale enfatizar alguns padrões:

- usar `rounded-xl` para entradas mais modernas
- usar `border-slate-300` para distinguir campos vazios dos campos com erro
- separar grupos com `space-y-4` ou `gap-4`
- manter um padrão visual entre login e cadastro
- usar `text-sm` e `font-medium` para labels
- manter contraste adequado para legibilidade

### Exemplo de padronização visual

```tsx
className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
```

Essa linha pode ser reutilizada em vários campos e reduz repetição.

---

## 10) Sugestão de roteiro da aula (60 min)

### 0-10 min — Introdução ao NativeWind
- o que é NativeWind
- por que usar classes em React Native
- comparação rápida com estilos inline

### 10-25 min — Organização do projeto
- estrutura de pastas
- rotas de login e cadastro
- ideia de navegação em app mobile

### 25-40 min — Login com validação
- schema Zod
- useForm + Controller
- button disabled quando inválido

### 40-55 min — Cadastro completo
- schema mais robusto
- confirmação de senha
- termos de uso
- Switch para checkbox

### 55-60 min — Encerramento
- revisão do que foi aprendido
- discussão sobre UX e manutenção

---

## 11) Desafio da aula (30 min)

### Desafio: cadastro e login completos

Implemente uma mini aplicação com:
- tela inicial com opções de login e cadastro
- tela de login com e-mail e senha
- tela de cadastro com nome, e-mail, senha, confirmar senha e termos
- validações com Zod
- aplicação de classes NativeWind para estilização

### Regras do desafio
- login: e-mail válido + senha mínima de 8 caracteres
- botão desabilitado quando inválido
- cadastro: nome mínimo 2, e-mail válido, senha forte, confirmação igual, termos obrigatórios
- usar uma identidade visual consistente em todo app

### Critérios de sucesso
- telas com design consistente
- boa legibilidade dos formulários
- feedback visual para erros
- regras de validação funcionando corretamente
- app navegando entre login e cadastro

---

## 12) Conclusão

Nesta aula, os alunos aprenderam a conectar dois pilares importantes do desenvolvimento mobile atual:
- NativeWind para criar interfaces com rapidez e consistência
- React Hook Form + Zod para validar e controlar formulários

Em conjunto, essas ferramentas ajudam a construir aplicações mais legíveis, com melhor experiência do usuário e com muito menos retrabalho manual.

A frase central da aula pode ser:

"Um app bonito e funcional não depende só de design; depende também de regras claras e validação bem aplicada."

---

## 13) Material de apoio

- NativeWind: https://www.nativewind.dev/
- Expo Router: https://docs.expo.dev/routing/introduction/
- React Hook Form: https://react-hook-form.com/
- Zod: https://zod.dev/

---

## 14) Perguntas de reflexão

- O que o NativeWind traz de vantagem em relação ao estilo inline?
- Como o React Hook Form facilita a leitura do código do formulário?
- Por que a validação com Zod ajuda a evitar bugs na UI?
- Qual a diferença entre uma tela bonita e uma tela funcional e bem validada?

---

## 15) Resumo rápido para apresentação

Nesta aula, o aluno entende que:
- NativeWind organiza a UI em classes utilitárias
- React Hook Form cuida da gestão do formulário
- Zod valida regras de negócio
- login e cadastro são cenários reais para aplicar essa stack

A junção dessas tecnologias cria uma base sólida para construir interfaces profissionais em React Native com Expo.
