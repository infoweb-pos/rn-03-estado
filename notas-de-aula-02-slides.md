# Aula 2 — NativeWind + formulários em React Native

## Tema
NativeWind com formulários de login e cadastro em React Native com Expo.

## Objetivo
Apresentar NativeWind e mostrar como ele pode ser usado junto com React Hook Form e Zod para criar formulários bonitos e validados.

## Stack
- Expo
- TypeScript
- React Navigation
- React Hook Form
- Zod
- NativeWind

---

## 1. Por que usar NativeWind?

O NativeWind permite aplicar classes do Tailwind diretamente em componentes React Native.

```tsx
<View className="flex-1 bg-slate-100 p-6" />
<Text className="text-xl font-bold text-slate-900">Bem-vindo</Text>
```

### Vantagens
- mais produtivo
- código mais legível
- visual consistente
- fácil manutenção

---

## 2. Estrutura sugerida

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

### Objetivo
Separar telas de autenticação e manter a navegação organizada.

---

## 3. Login: campos e regras

### Campos
- E-mail
- Senha
- Botão de login

### Regras
- e-mail obrigatório e válido
- domínio válido
- senha mínima de 8 caracteres
- botão habilitado somente quando o formulário estiver válido

```ts
const loginSchema = z.object({
  email: z.string().email('E-mail inválido').refine(
    (value) => value.includes('@') && value.includes('.'),
    'E-mail com domínio inválido'
  ),
  senha: z.string().min(8, 'A senha deve ter pelo menos 8 caracteres'),
});
```

---

## 4. Cadastro: campos e regras

### Campos
- Nome
- E-mail
- Senha
- Confirmar senha
- Termos de uso

### Regras
- nome obrigatório, mínimo 2 caracteres
- e-mail válido
- senha com 8+ caracteres
- senha com maiúscula, minúscula, número e caractere especial
- confirmar senha igual à senha
- termos obrigatórios

```ts
const cadastroSchema = z
  .object({
    nome: z.string().min(2, 'Nome obrigatório'),
    email: z.string().email('E-mail inválido'),
    senha: z
      .string()
      .min(8, 'A senha deve ter pelo menos 8 caracteres')
      .regex(/[A-Z]/, 'Precisa ter letra maiúscula')
      .regex(/[a-z]/, 'Precisa ter letra minúscula')
      .regex(/[0-9]/, 'Precisa ter um número')
      .regex(/[^A-Za-z0-9]/, 'Precisa ter um caractere especial'),
    confirmarSenha: z.string(),
    termos: z.boolean().refine((v) => v === true, {
      message: 'Você precisa aceitar os termos',
    }),
  })
  .refine((data) => data.senha === data.confirmarSenha, {
    message: 'As senhas não conferem',
    path: ['confirmarSenha'],
  });
```

---

## 5. Integração com React Hook Form + Zod

```tsx
const { control, handleSubmit, formState: { errors } } = useForm({
  resolver: zodResolver(cadastroSchema),
});
```

### Fluxo
1. usuário preenche formulário
2. RHF monitora os campos
3. Zod valida as regras
4. erros são exibidos abaixo do campo
5. submit só acontece quando tudo estiver válido

---

## 6. Estilo dos formulários

```tsx
className="rounded-xl border border-slate-300 bg-white px-4 py-3 text-base"
```

### Boas práticas
- campos com borda consistente
- espaçamento uniforme
- labels claros
- mensagens de erro próximas ao campo
- visual padronizado entre login e cadastro

---

## 7. Desafio da aula

Crie uma mini aplicação com:
- home para acessar login e cadastro
- tela de login com e-mail e senha
- tela de cadastro com nome, e-mail, senha, confirmação e termos
- validações com Zod
- estilos com NativeWind

### Critérios de sucesso
- telas consistentes
- validações funcionando
- botões e campos com feedback visual
- navegação entre telas

---

## 8. Conclusão

Nesta aula, aprendemos que:
- NativeWind agiliza a estilização em React Native
- React Hook Form organiza o formulário
- Zod centraliza a validação
- juntos, eles ajudam a construir telas mais bonitas e mais confiáveis

"Um app bonito e funcional depende tanto da UI quanto da validação correta."
