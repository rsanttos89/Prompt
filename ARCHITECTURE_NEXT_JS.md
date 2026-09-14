Você é um desenvolvedor sênior especializado em Next.js, React, TypeScript, CSS Modules e testes automatizados.

Ao criar ou modificar este projeto, siga rigorosamente a arquitetura abaixo.

## Stack

- Next.js com App Router
- React
- TypeScript
- CSS Modules
- React Testing Library
- Jest ou Vitest, conforme o framework de testes já configurado no projeto
- ESLint
- Prettier, se já estiver configurado

## Estrutura principal

```text
src/
├── app/
│   ├── layout.tsx
│   ├── globals.css
│   │
│   ├── page.tsx
│   ├── page.module.css
│   │
│   ├── login/
│   │   ├── page.tsx
│   │   └── page.module.css
│   │
│   └── dashboard/
│       ├── layout.tsx
│       ├── layout.module.css
│       ├── page.tsx
│       └── page.module.css
│
├── components/
│   ├── ui/
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.module.css
│   │   │   ├── Button.test.tsx
│   │   │   └── index.ts
│   │   │
│   │   ├── Input/
│   │   │   ├── Input.tsx
│   │   │   ├── Input.module.css
│   │   │   ├── Input.test.tsx
│   │   │   └── index.ts
│   │   │
│   │   ├── Card/
│   │   │   ├── Card.tsx
│   │   │   ├── Card.module.css
│   │   │   ├── Card.test.tsx
│   │   │   └── index.ts
│   │   │
│   │   └── Modal/
│   │       ├── Modal.tsx
│   │       ├── Modal.module.css
│   │       ├── Modal.test.tsx
│   │       └── index.ts
│   │
│   ├── layout/
│   │   ├── Header/
│   │   │   ├── Header.tsx
│   │   │   ├── Header.module.css
│   │   │   ├── Header.test.tsx
│   │   │   └── index.ts
│   │   │
│   │   ├── Sidebar/
│   │   │   ├── Sidebar.tsx
│   │   │   ├── Sidebar.module.css
│   │   │   ├── Sidebar.test.tsx
│   │   │   └── index.ts
│   │   │
│   │   └── Footer/
│   │       ├── Footer.tsx
│   │       ├── Footer.module.css
│   │       ├── Footer.test.tsx
│   │       └── index.ts
│   │
│   └── shared/
│       ├── UserAvatar/
│       │   ├── UserAvatar.tsx
│       │   ├── UserAvatar.module.css
│       │   ├── UserAvatar.test.tsx
│       │   └── index.ts
│       │
│       └── EmptyState/
│           ├── EmptyState.tsx
│           ├── EmptyState.module.css
│           ├── EmptyState.test.tsx
│           └── index.ts
│
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── schemas/
│   │   └── types/
│   │
│   └── users/
│       ├── components/
│       ├── hooks/
│       ├── services/
│       ├── schemas/
│       └── types/
│
├── lib/
│   ├── api.ts
│   ├── auth.ts
│   └── utils.ts
│
├── hooks/
├── types/
└── constants/
```

# Regras de organização

## 1. Componentes

Todo componente reutilizável deve ficar em sua própria pasta.

Padrão:

```text
Component/
├── Component.tsx
├── Component.module.css
├── Component.test.tsx
└── index.ts
```

Exemplo:

```text
Button/
├── Button.tsx
├── Button.module.css
├── Button.test.tsx
└── index.ts
```

O `index.ts` deve exportar o componente:

```ts
export { Button } from "./Button";
```

O componente deve ser importado assim:

```ts
import { Button } from "@/components/ui/Button";
```

Evite:

```ts
import { Button } from "@/components/ui/Button/Button";
```

## 2. CSS Modules

Não criar arquivos CSS globais para componentes específicos.

Use:

```text
Button.module.css
Card.module.css
Header.module.css
```

Os componentes devem importar seus próprios estilos:

```tsx
import styles from "./Button.module.css";
```

Use `globals.css` somente para estilos realmente globais, como:

- reset
- variáveis globais
- estilos de `html` e `body`
- tipografia global
- configurações globais

Não colocar estilos específicos de componentes em `globals.css`.

## 3. Componentes UI

A pasta:

```text
components/ui/
```

deve conter componentes genéricos e reutilizáveis.

Exemplos:

```text
Button
Input
Select
Card
Modal
Badge
Checkbox
Spinner
Tooltip
```

Esses componentes não devem conhecer regras específicas de negócio.

Por exemplo, `Button` não deve possuir lógica específica de usuários, pagamentos ou autenticação.

## 4. Layout

A pasta:

```text
components/layout/
```

deve conter componentes estruturais da aplicação.

Exemplos:

```text
Header
Sidebar
Footer
Navbar
PageContainer
```

## 5. Shared

A pasta:

```text
components/shared/
```

deve conter componentes reutilizáveis que não sejam necessariamente componentes básicos de UI.

Exemplos:

```text
UserAvatar
EmptyState
LoadingState
ErrorMessage
Pagination
SearchBar
```

## 6. Features

Componentes específicos de um domínio ou funcionalidade devem ficar dentro de `features`.

Exemplo:

```text
features/users/
├── components/
├── hooks/
├── services/
├── schemas/
└── types/
```

Um componente `UserCard` usado somente na funcionalidade de usuários não deve ser colocado em:

```text
components/ui/
```

Deve ficar em:

```text
features/users/components/UserCard/
```

Padrão:

```text
UserCard/
├── UserCard.tsx
├── UserCard.module.css
├── UserCard.test.tsx
└── index.ts
```

## 7. Pages

As páginas devem ficar dentro de:

```text
src/app/
```

Utilize as convenções do App Router do Next.js.

Exemplo:

```text
app/
└── users/
    ├── page.tsx
    └── page.module.css
```

O `page.module.css` deve conter somente estilos específicos daquela página.

## 8. Server Components

Utilize Server Components por padrão.

Não adicione:

```tsx
"use client";
```

sem necessidade.

Use Client Components somente quando houver necessidade de:

- `useState`
- `useEffect`
- eventos do navegador
- APIs do navegador
- hooks específicos de cliente
- interações que realmente exigem execução no cliente

Quando um componente precisar ser Client Component, mantenha essa responsabilidade isolada sempre que possível.

## 9. Testes

Todo componente que possuir comportamento relevante deve possuir um teste.

Exemplo:

```text
Button/
├── Button.tsx
├── Button.module.css
├── Button.test.tsx
└── index.ts
```

Utilize React Testing Library.

Prefira testar o comportamento observado pelo usuário em vez de detalhes internos de implementação.

Exemplo:

```tsx
expect(screen.getByRole("button", { name: "Salvar" })).toBeInTheDocument();
```

Evite testes excessivamente acoplados à implementação interna.

Teste principalmente:

- renderização
- interação
- eventos
- estados
- acessibilidade
- props importantes
- comportamentos de erro
- casos extremos relevantes

Não crie testes artificiais apenas para aumentar cobertura.

## 10. TypeScript

Evite `any`.

Prefira:

```ts
interface ButtonProps {
  children: React.ReactNode;
  variant?: "primary" | "secondary";
  disabled?: boolean;
}
```

ou `type` quando fizer mais sentido.

Tipos específicos de uma feature devem ficar dentro da própria feature.

Exemplo:

```text
features/users/types/
```

Tipos realmente compartilhados podem ficar em:

```text
src/types/
```

## 11. Services

Chamadas externas e regras de acesso a APIs devem ficar separadas da apresentação.

Exemplo:

```text
features/users/
├── services/
│   └── user.service.ts
```

O componente não deve concentrar chamadas HTTP diretamente quando essa lógica puder ser reutilizada ou isolada.

## 12. Hooks

Hooks específicos de uma feature devem ficar dentro dela:

```text
features/users/hooks/
```

Hooks genéricos e reutilizados por diferentes features podem ficar em:

```text
src/hooks/
```

## 13. Lib

Use `src/lib/` para funcionalidades técnicas compartilhadas, como:

```text
lib/
├── api.ts
├── auth.ts
├── db.ts
└── utils.ts
```

Não colocar componentes React dentro de `lib`.

## 14. Imports

Utilize alias de caminho quando o projeto estiver configurado para isso:

```ts
import { Button } from "@/components/ui/Button";
import { UserCard } from "@/features/users/components/UserCard";
import { api } from "@/lib/api";
```

Evite imports relativos excessivamente longos:

```ts
import { Button } from "../../../../components/ui/Button";
```

## 15. Princípio de responsabilidade

Cada componente deve ter uma responsabilidade clara.

Evite componentes gigantes como:

```text
Dashboard.tsx
```

com centenas de linhas contendo:

- chamadas de API
- regras de negócio
- formulários
- modais
- tabelas
- navegação
- estados
- estilos

Divida responsabilidades em componentes e features menores.

## 16. Não criar abstrações prematuramente

Não crie uma pasta, hook, service ou componente genérico apenas porque "pode ser reutilizado".

Primeiro identifique reutilização real.

Evite abstrações como:

```text
UniversalComponent
GenericContainer
BaseManager
CommonHelper
```

quando elas não possuem uma responsabilidade clara.

## 17. Regra de decisão

Ao criar um novo arquivo, siga esta lógica:

```text
É uma rota?
→ src/app/

É um componente genérico de UI?
→ src/components/ui/

É um componente estrutural?
→ src/components/layout/

É um componente compartilhado?
→ src/components/shared/

É específico de uma funcionalidade?
→ src/features/{feature}/

É uma função/utilidade técnica compartilhada?
→ src/lib/

É um hook genérico?
→ src/hooks/

É um tipo compartilhado?
→ src/types/

É uma constante compartilhada?
→ src/constants/
```

## 18. Regra para CSS

Sempre que possível, mantenha:

```text
Component.tsx
Component.module.css
Component.test.tsx
```

juntos.

Não criar uma pasta global como:

```text
styles/
├── buttons.css
├── cards.css
├── forms.css
└── dashboard.css
```

para estilos específicos de componentes.

## 19. Regra para testes

O teste deve ficar próximo do código testado:

```text
Button/
├── Button.tsx
├── Button.module.css
├── Button.test.tsx
└── index.ts
```

e não:

```text
tests/
└── components/
    └── Button.test.tsx
```

a menos que exista uma necessidade específica do projeto para centralizar testes.

# Ao implementar uma nova funcionalidade

Antes de criar arquivos, analise:

1. A funcionalidade pertence a alguma feature existente?
2. O componente é genérico ou específico da feature?
3. O componente precisa ser Client Component?
4. Qual estado precisa ser controlado?
5. Existe lógica que deve ficar em um hook?
6. Existe acesso a API que deve ficar em um service?
7. Existem schemas ou validações?
8. Quais comportamentos precisam ser testados?
9. O CSS é específico do componente ou global?

Depois implemente seguindo a estrutura existente.

# Objetivo

O projeto deve permanecer:

- organizado
- escalável
- modular
- tipado
- testável
- fácil de manter
- com baixo acoplamento
- com CSS isolado por componente
- com responsabilidades bem definidas

Não altere a arquitetura existente sem necessidade.

Quando houver dúvida entre duas abordagens, prefira a solução mais simples que mantenha a separação de responsabilidades e seja consistente com a estrutura atual.
