# Aula 1 — React Hook Form + Zod no React Native com Expo

## Tema
Criando formulários robustos e validados no React Native usando React Hook Form e Zod.

## Público
- Estudantes de Desenvolvimento Mobile/Web
- Conhecimentos básicos em React, React Native, useState e TypeScript

## Objetivo da aula
Ao final da aula, o aluno será capaz de:
- entender por que `useState` sozinho não escala bem em formulários
- criar formulários com React Hook Form
- validar dados com Zod
- mostrar erros de forma clara para o usuário
- integrar React Hook Form + Zod
- implementar regras mais avançadas

---

## 1. Problema: formulário com `useState`

Quando o formulário cresce, o código vira repetitivo e difícil de manter.

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

### Problemas:
- muita lógica manual
- validação espalhada
- difícil manter o código
- cada campo exige atenção individual
- erros ficam acoplados à UI

### Pergunta para a turma
“Se eu tiver 10 campos e 20 regras, como manter isso legível?”

---

## 2. Solução: React Hook Form

`react-hook-form` ajuda a gerenciar os campos do formulário de forma organizada.

### Principais vantagens
- reduz o uso de `useState` para campos
- facilita o acesso aos valores
- lida com submit e validação
- gera menos boilerplate
- funciona muito bem com TypeScript

### Conceitos principais
- `useForm()` → cria o formulário
- `Controller` → conecta o input ao formulário
- `handleSubmit()` → dispara a validação e o envio
- `formState.errors` → guarda os erros

---

## 3. Validação com Zod

`zod` oferece schemas para validar dados.

```ts
import { z } from 'zod';

const usuarioSchema = z.object({
  nome: z.string().min(2, 'Nome deve ter pelo menos 2 caracteres'),
  email: z.string().email('Digite um e-mail válido'),
  senha: z.string().min(6, 'Senha deve ter pelo menos 6 caracteres'),
});
```

### Vantagens
- regras centralizadas em um único lugar
- fácil leitura
- validação tipada
- integração direta com o formulário

---

## 4. Integração: React Hook Form + Zod

A integração é feita com `@hookform/resolvers`.

```bash
npx expo install react-hook-form zod @hookform/resolvers
```

```tsx
const { control, handleSubmit, formState: { errors } } = useForm({
  resolver: zodResolver(usuarioSchema),
});
```

### Fluxo da validação
1. Usuário digita no input
2. React Hook Form registra o valor
3. Zod valida o schema
4. Se houver erro, o formulário não envia
5. A mensagem aparece para o usuário

---

## 5. Exemplo prático: cadastro de usuário

### Campos
- Nome
- E-mail
- Senha

### Regras
- nome obrigatório
- e-mail válido
- senha com no mínimo 6 caracteres

```tsx
import { Controller, useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { View, Text, TextInput, Button, Alert } from 'react-native';

const usuarioSchema = z.object({
  nome: z.string().min(2, 'Nome deve ter pelo menos 2 caracteres'),
  email: z.string().email('E-mail inválido'),
  senha: z.string().min(6, 'Senha deve ter pelo menos 6 caracteres'),
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
    Alert.alert('Sucesso', JSON.stringify(data));
  };

  return (
    <View style={{ padding: 24 }}>
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
          />
        )}
      />
      {errors.nome && <Text>{errors.nome.message}</Text>}

      <Text>E-mail</Text>
      <Controller
        control={control}
        name="email"
        render={({ field: { onChange, onBlur, value } }) => (
          <TextInput
            value={value}
            onChangeText={onChange}
            onBlur={onBlur}
            keyboardType="email-address"
            placeholder="seu@email.com"
          />
        )}
      />
      {errors.email && <Text>{errors.email.message}</Text>}

      <Text>Senha</Text>
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
          />
        )}
      />
      {errors.senha && <Text>{errors.senha.message}</Text>}

      <Button title="Cadastrar" onPress={handleSubmit(onSubmit)} />
    </View>
  );
}
```

---

## 6. O que o aluno deve entender

### React Hook Form
- cuida do estado do formulário
- conecta inputs ao formulário
- facilita submit e validação

### Zod
- define regras de negócio em schema
- valida campos e combinações
- reduz lógica espalhada pelo código

### Juntos
- o formulário fica mais organizado
- a validação fica previsível
- a UX melhora com mensagens de erro mais claras

---

## 7. Validações mais avançadas

Agora vamos ampliar o cenário do cadastro.

### Campos do desafio
- Nome
- E-mail
- Senha
- Confirmar senha
- Termos de uso

### Regras adicionais
- senha mínimo 8 caracteres
- pelo menos uma letra maiúscula
- pelo menos um número
- confirmar senha deve ser igual
- termos devem ser aceitos

```ts
const cadastroSchema = z
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

### Conceito importante
`refine` permite comparar valores entre campos.

---

## 8. Exibindo mensagens de erro

No React Native, a mensagem pode aparecer logo abaixo do campo:

```tsx
{errors.email && <Text style={{ color: 'red' }}>{errors.email.message}</Text>}
```

### Boa prática
- deixar a mensagem clara
- usar a cor vermelha para indicar erro
- mostrar perto do campo correspondente
- evitar alertas excessivos em telas de formulário

---

## 9. Quando usar isso em aplicações reais?

Essas ferramentas são úteis em:
- cadastro de usuário
- login
- recuperação de senha
- edição de perfil
- checkout
- formulário de contato
- qualquer funcionalidade com múltiplos campos

---

## 10. Comparação rápida

### com `useState`
- mais código
- mais chances de erro
- menos escalável

### com `React Hook Form + Zod`
- código mais limpo
- validação centralizada
- melhor manutenção
- melhor experiência do usuário

---

## 11. Desafio da aula (30 min)

Crie um formulário de cadastro com:
- nome
- e-mail
- senha
- confirmar senha
- termos de uso

### Validações do desafio
- nome obrigatório
- e-mail válido
- senha mínima de 8 caracteres
- senha com letra maiúscula
- senha com número
- confirmação igual à senha
- termos obrigatórios

### Critérios de sucesso
- Mostrar erro abaixo do campo correto
- Não enviar formulário se houver erro
- Validar combinação de campos
- Exibir sucesso quando os dados forem válidos

---

## 12. Conclusão

Nesta aula aprendemos que:
- `useState` sozinho não é ideal para formulários mais complexos
- `React Hook Form` organiza a lógica do formulário
- `Zod` centraliza a validação
- a combinação dessas ferramentas produz código mais seguro, legível e profissional

### Frase-chave da aula
“Formulários bem validados são a diferença entre uma experiência simples e uma experiência confiável.”

---

## 13. Dicas para professor

- mostrar primeiro a versão com `useState` e depois a versão com RHF + Zod
- pedir para os alunos observarem a diferença de tamanho e clareza do código
- reforçar que a validação do frontend melhora UX, mas precisa ser complementada com validação do backend
- incentivar o uso de schemas como fonte de verdade da regra de negócio

---

## 14. Material complementar

- React Hook Form: https://react-hook-form.com/
- Zod: https://zod.dev/
- Expo: https://docs.expo.dev/

---

## 15. Slide final para fechar a aula

### Hoje aprendemos
- Problemas de formulários com `useState`
- Como o React Hook Form organiza os campos
- Como o Zod valida regras de negócio
- Como combinar as duas ferramentas
- Como criar formulários robustos em React Native

### Próximo passo
Aplicar esse padrão em login, cadastro e formulários reais de produção.
