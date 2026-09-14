Crie a estrutura inicial de um aplicativo mobile usando **Expo + React Native + TypeScript + Expo Router**, seguindo as boas práticas e a documentação oficial atual do Expo.

## Objetivo

Criar uma arquitetura limpa, escalável e preparada para produção, incluindo testes unitários e de componentes.

## Stack obrigatória

- Expo
- React Native
- TypeScript
- Expo Router
- Jest
- jest-expo
- React Native Testing Library
- `expo-router/testing-library` para testes relacionados à navegação

## Estrutura de pastas

Use `src/` como diretório principal do código.

A pasta `src/app` deve conter **somente arquivos relacionados às rotas do Expo Router**.

Use esta estrutura inicial:

```text
project/
├── src/
│   ├── app/
│   │   ├── _layout.tsx
│   │   ├── index.tsx
│   │   │
│   │   ├── (auth)/
│   │   │   ├── _layout.tsx
│   │   │   ├── login.tsx
│   │   │   └── register.tsx
│   │   │
│   │   ├── (tabs)/
│   │   │   ├── _layout.tsx
│   │   │   ├── index.tsx
│   │   │   ├── explore.tsx
│   │   │   └── profile.tsx
│   │   │
│   │   └── users/
│   │       └── [id].tsx
│   │
│   ├── components/
│   │   ├── ui/
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   └── Card.tsx
│   │   │
│   │   └── common/
│   │       ├── Header.tsx
│   │       └── EmptyState.tsx
│   │
│   ├── hooks/
│   │   ├── useAuth.ts
│   │   └── useTheme.ts
│   │
│   ├── services/
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   └── users.ts
│   │
│   ├── stores/
│   │   └── authStore.ts
│   │
│   ├── utils/
│   │   ├── formatCurrency.ts
│   │   └── validation.ts
│   │
│   ├── types/
│   │   ├── user.ts
│   │   └── auth.ts
│   │
│   └── constants/
│       ├── colors.ts
│       └── config.ts
│
├── __tests__/
│   ├── components/
│   │   ├── Button.test.tsx
│   │   └── Input.test.tsx
│   │
│   ├── hooks/
│   │   └── useAuth.test.ts
│   │
│   ├── services/
│   │   └── auth.test.ts
│   │
│   ├── utils/
│   │   └── validation.test.ts
│   │
│   └── routes/
│       ├── home.test.tsx
│       └── auth.test.tsx
│
├── assets/
│   ├── images/
│   ├── fonts/
│   └── icons/
│
├── app.json
├── package.json
├── tsconfig.json
├── jest.config.js
└── ...
```

## Regras de arquitetura

1. `src/app` deve conter exclusivamente rotas, layouts e arquivos especiais reconhecidos pelo Expo Router.

2. Não coloque:
   - componentes reutilizáveis;
   - hooks;
   - services;
   - stores;
   - utils;
   - testes;
   - tipos;
     dentro de `src/app`.

3. Utilize route groups do Expo Router:
   - `(auth)` para autenticação;
   - `(tabs)` para navegação principal.

4. Utilize rotas dinâmicas quando necessário, como:

   ```text
   users/[id].tsx
   ```

5. Utilize `_layout.tsx` para configurar os layouts e navegadores de cada grupo.

6. Mantenha a lógica de negócio fora das telas sempre que possível.

7. Componentes devem ser pequenos, reutilizáveis e preferencialmente desacoplados da navegação.

8. Utilize TypeScript com `strict` habilitado.

9. Configure aliases de importação, por exemplo:

   ```text
   @/*
   ```

   apontando para `src/*`.

10. Evite imports relativos excessivamente longos como:

```tsx
../../../components/Button
```

Prefira:

```tsx
import { Button } from "@/components/ui/Button";
```

## Testes

Configure Jest usando `jest-expo`.

O projeto deve possuir:

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

Configure também o React Native Testing Library.

Crie testes para:

- componentes;
- hooks;
- utils;
- services;
- telas;
- navegação do Expo Router.

Para testes de navegação, utilize:

```tsx
import { renderRouter, screen } from "expo-router/testing-library";
```

Não coloque os testes dentro de `src/app`.

## Qualidade

Configure:

- TypeScript strict;
- ESLint;
- Prettier;
- Jest;
- React Native Testing Library;
- aliases de importação;
- cobertura de testes.

O código deve ser simples e seguir princípios de:

- SOLID quando aplicável;
- separação de responsabilidades;
- composição;
- reutilização;
- baixo acoplamento;
- alta coesão.

Não crie abstrações desnecessárias.

## Importante

Não invente uma arquitetura proprietária que contrarie o funcionamento do Expo Router.

Priorize as convenções oficiais do Expo.

Quando houver alguma decisão arquitetural que não seja definida pela documentação oficial do Expo, escolha a alternativa mais simples, convencional e escalável e explique brevemente a decisão.

## Entrega

Ao finalizar:

1. Mostre a árvore completa de diretórios.
2. Crie todos os arquivos necessários.
3. Configure o Jest.
4. Configure o TypeScript.
5. Configure os aliases.
6. Configure ESLint e Prettier.
7. Crie exemplos funcionais de componentes.
8. Crie exemplos de testes.
9. Crie pelo menos um teste de navegação usando `expo-router/testing-library`.
10. Garanta que `npm test` funcione.
11. Garanta que o TypeScript compile sem erros.
12. Não deixe arquivos fictícios ou imports quebrados.
13. Explique como executar:
    - o projeto;
    - os testes;
    - os testes em modo watch;
    - a cobertura de testes.

Antes de finalizar, verifique se a estrutura não contém testes ou componentes dentro de `src/app` e se todas as rotas do Expo Router estão corretamente configuradas.
