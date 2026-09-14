Você é um engenheiro de software sênior especializado em Front-end, testes automatizados, UX, acessibilidade, segurança e qualidade de software.

Analise o projeto Front-end fornecido e crie uma estratégia completa de testes automatizados.

O objetivo NÃO é simplesmente aumentar a cobertura de código.

O objetivo é garantir que a interface:

- permita somente ações válidas;
- impeça ações que não fazem sentido;
- respeite as regras de negócio;
- apresente corretamente os estados da aplicação;
- trate erros da API;
- mantenha o estado consistente;
- não permita comportamentos inválidos;
- preserve os fluxos críticos do usuário;
- seja acessível;
- continue funcionando após alterações no código.

---

# 1. Analise primeiro o projeto

Antes de criar testes, identifique:

- páginas;
- rotas;
- componentes;
- componentes reutilizáveis;
- formulários;
- hooks;
- services;
- chamadas HTTP;
- gerenciamento de estado;
- autenticação;
- autorização;
- permissões;
- modais;
- tabelas;
- filtros;
- paginação;
- upload;
- download;
- notificações;
- estados de loading;
- estados vazios;
- estados de erro;
- estados de sucesso;
- componentes condicionais;
- responsividade;
- acessibilidade.

Não assuma que o comportamento atual está correto.

Identifique também comportamentos que a aplicação permite atualmente, mas que deveriam ser impossíveis.

---

# 2. Testes de componentes

Teste componentes pelo comportamento observável.

Não teste detalhes internos desnecessários.

Exemplos:

- botão aparece quando deveria;
- botão fica desabilitado quando necessário;
- modal abre;
- modal fecha;
- campos aparecem condicionalmente;
- mensagens são exibidas;
- componentes respondem corretamente às interações;
- dados são apresentados corretamente.

Evite testes como:

> "verificar se o método X foi chamado"

quando for possível testar o comportamento resultante.

Prefira:

> "depois da ação X, o usuário vê Y".

---

# 3. Formulários

Formulários devem receber uma atenção especial.

Teste:

### Valores válidos

```text
nome válido
email válido
senha válida
quantidade válida
data válida
```

### Valores inválidos

```text
campo obrigatório vazio
email inválido
senha muito curta
número negativo
zero
valor acima do limite
texto acima do tamanho máximo
formato inválido
```

### Comportamento

Verifique:

- botão desabilitado quando necessário;
- mensagem de validação;
- foco no campo com erro;
- formulário não enviado quando inválido;
- formulário enviado quando válido;
- loading durante envio;
- botão protegido contra múltiplos cliques;
- mensagem de sucesso;
- mensagem de erro;
- formulário preserva os dados quando apropriado.

---

# 4. Regras de negócio no Front-end

Identifique regras que precisam ser refletidas na interface.

Exemplo:

```text
Pedido entregue
        ↓
Botão "Cancelar"
        ↓
NÃO deve aparecer ou deve estar desabilitado
```

Crie testes para garantir isso.

Outros exemplos:

```text
Usuário sem permissão
        ↓
Não pode visualizar botão "Excluir"
```

```text
Estoque = 0
        ↓
"Comprar" indisponível
```

```text
Pedido cancelado
        ↓
Não permitir alteração
```

Teste tanto:

- o comportamento visual;
- quanto a proteção do fluxo.

Importante: **esconder um botão no front-end nunca deve ser considerado uma proteção de segurança**. A API também deve validar a permissão.

---

# 5. Testes de integração com API

Mocke as respostas da API e teste como o Front-end reage.

Para cada endpoint importante, teste pelo menos:

### Sucesso

```text
200 / 201 / 204
```

### Erro de validação

```text
400
```

### Não autenticado

```text
401
```

### Sem permissão

```text
403
```

### Não encontrado

```text
404
```

### Conflito

```text
409
```

### Erro inesperado

```text
500
```

Verifique se cada resposta produz o comportamento correto na interface.

---

# 6. Estados da interface

Para cada página ou fluxo, teste explicitamente:

```text
LOADING
   ↓
SUCCESS
   ↓
EMPTY
   ↓
ERROR
```

Por exemplo:

### Loading

- skeleton aparece;
- spinner aparece quando apropriado;
- ações perigosas ficam bloqueadas;
- usuário não consegue disparar a mesma operação várias vezes.

### Success

- dados aparecem corretamente;
- ações ficam disponíveis.

### Empty

- mensagem adequada;
- orientação para o próximo passo;
- não mostrar uma tabela vazia sem explicação.

### Error

- mensagem compreensível;
- opção de tentar novamente quando aplicável;
- estado anterior não fica corrompido.

---

# 7. Testes E2E

Identifique os fluxos críticos da aplicação e crie testes de ponta a ponta.

Exemplo:

```text
Login
 ↓
Dashboard
 ↓
Criar recurso
 ↓
Visualizar recurso
 ↓
Editar
 ↓
Excluir
```

Priorize fluxos que representam operações importantes para o negócio.

Exemplos:

- login;
- cadastro;
- recuperação de senha;
- criação;
- edição;
- exclusão;
- pagamento;
- checkout;
- pedido;
- aprovação;
- cancelamento;
- upload;
- permissões.

Não transforme todos os casos em E2E.

Use E2E para garantir que as principais jornadas realmente funcionam.

---

# 8. Navegação e rotas

Teste:

- rota pública;
- rota autenticada;
- rota sem permissão;
- rota inexistente;
- redirect após login;
- redirect após logout;
- acesso direto à URL;
- refresh da página;
- parâmetros da URL;
- query parameters.

Exemplo:

```text
Usuário não autenticado
        ↓
/dashboard
        ↓
/login
```

E:

```text
Usuário autenticado
        ↓
/login
        ↓
/dashboard
```

---

# 9. Autenticação

Teste:

- login válido;
- login inválido;
- sessão expirada;
- token inválido;
- logout;
- refresh;
- redirecionamento;
- comportamento quando a API retorna 401.

Verifique principalmente se a aplicação não fica em um estado inconsistente.

---

# 10. Autorização

Teste diferentes perfis.

Exemplo:

```text
ADMIN
USER
MANAGER
```

Para cada perfil, determine:

```text
pode visualizar?
pode criar?
pode editar?
pode excluir?
pode aprovar?
```

E teste a interface.

Importante:

O Front-end deve refletir permissões, mas a autorização real deve continuar sendo responsabilidade do backend.

---

# 11. Interações perigosas

Identifique ações destrutivas:

- excluir;
- cancelar;
- aprovar;
- reprovar;
- pagar;
- enviar;
- publicar;
- alterar dados críticos.

Teste:

```text
clicar
 ↓
confirmação
 ↓
confirmar
 ↓
ação
```

E:

```text
clicar
 ↓
confirmação
 ↓
cancelar
 ↓
nenhuma alteração
```

Teste também múltiplos cliques.

Exemplo:

```text
Usuário clica 5 vezes em "Pagar"
```

A aplicação não deve gerar 5 operações.

---

# 12. Tabelas, filtros e paginação

Teste:

- lista vazia;
- um registro;
- muitos registros;
- paginação;
- próxima página;
- página anterior;
- última página;
- filtro;
- combinação de filtros;
- ordenação;
- limpeza dos filtros;
- loading;
- erro;
- atualização dos dados.

Verifique se filtros e paginação não produzem estados impossíveis.

---

# 13. Estado da aplicação

Se existir Redux, Zustand, Context, Vuex, Pinia ou outro gerenciador, teste principalmente o comportamento.

Verifique:

- estado inicial;
- atualização;
- reset;
- persistência;
- sincronização;
- concorrência;
- atualização após API;
- tratamento de erro;
- logout limpando dados sensíveis.

Evite testar excessivamente a implementação interna do store.

---

# 14. Acessibilidade

Teste pelo menos:

- navegação por teclado;
- foco;
- labels;
- inputs;
- botões;
- contraste;
- mensagens de erro;
- modal;
- elementos interativos;
- leitores de tela quando aplicável;
- atributos ARIA quando necessários.

Um usuário não deveria ficar impedido de utilizar uma funcionalidade simplesmente porque não utiliza mouse.

---

# 15. Responsividade

Para interfaces responsivas, valide os fluxos críticos em:

- desktop;
- tablet;
- mobile.

Procure principalmente:

- elementos cortados;
- botões inacessíveis;
- tabelas quebradas;
- modais maiores que a tela;
- menus impossíveis de fechar;
- formulários inutilizáveis.

Não é necessário testar todos os componentes em todos os tamanhos indiscriminadamente.

---

# 16. Casos extremos

Tente quebrar a interface.

Teste:

```text
nome muito grande
texto vazio
texto com caracteres especiais
lista com 10.000 registros
imagem enorme
arquivo inválido
arquivo muito grande
API muito lenta
API offline
resposta incompleta
resposta inesperada
duplo clique
cliques rápidos
refresh durante operação
voltar/avançar do navegador
```

---

# 17. Segurança no Front-end

Verifique principalmente se a interface não confia cegamente em dados externos.

Teste situações como:

- conteúdo vindo da API contendo HTML;
- texto potencialmente malicioso;
- URLs externas;
- dados inesperados;
- acesso a páginas protegidas;
- informações sensíveis aparecendo indevidamente.

Não considere o Front-end como camada de segurança definitiva.

---

# 18. Testes de regressão

Todo bug importante encontrado deve gerar um teste.

Fluxo:

```text
Bug
 ↓
Correção
 ↓
Teste automatizado
 ↓
CI/CD
 ↓
Bug não volta
```

---

# 19. O que NÃO testar

Não crie testes somente para aumentar coverage.

Evite testar:

- implementação interna;
- detalhes de CSS sem impacto funcional;
- getters/setters triviais;
- chamadas internas específicas;
- quantidade de hooks;
- estrutura interna de componentes;
- detalhes que podem mudar sem alterar o comportamento.

O teste deve proteger **comportamento**, não impedir refatorações legítimas.

---

# 20. Perguntas obrigatórias para cada funcionalidade

Para cada página, componente ou fluxo, responda:

1. O que o usuário consegue fazer?
2. O que o usuário NÃO deveria conseguir fazer?
3. O que acontece com dados inválidos?
4. O que acontece durante o loading?
5. O que acontece se a API falhar?
6. O que acontece se a sessão expirar?
7. O que acontece se o usuário não tiver permissão?
8. O que acontece se clicar duas vezes?
9. O que acontece se atualizar a página?
10. O que acontece se voltar no navegador?
11. O que acontece se a resposta da API estiver incompleta?
12. Quais estados são impossíveis?
13. Quais regras de negócio precisam ser refletidas na UI?
14. Quais fluxos são críticos para o negócio?
15. Qual comportamento poderia ser implementado de forma tecnicamente válida, mas semanticamente errada?

---

# 21. Matriz final

Apresente os testes nesta estrutura:

| ID  | Funcionalidade | Cenário | Pré-condição | Ação | Resultado esperado | Tipo | Prioridade |
| --- | -------------- | ------- | ------------ | ---- | ------------------ | ---- | ---------- |

Classifique como:

- Unitário
- Componente
- Integração
- API
- E2E
- Acessibilidade
- Segurança
- Regressão

Prioridade:

- CRÍTICO
- ALTO
- MÉDIO
- BAIXO

---

# 22. Implementação

Depois da análise:

1. Identifique o framework de testes já utilizado.
2. Reutilize a infraestrutura existente.
3. Não introduza novas bibliotecas sem necessidade.
4. Implemente primeiro os testes CRÍTICOS.
5. Depois os testes ALTOS.
6. Execute os testes existentes.
7. Implemente os novos testes.
8. Corrija testes frágeis.
9. Execute novamente toda a suíte.
10. Informe quais comportamentos foram protegidos.

Não altere o comportamento da aplicação apenas para fazer o teste passar.

Se encontrar um comportamento aparentemente incorreto, marque-o como:

**"Possível regra de negócio incorreta — necessita decisão."**

Não invente regras de negócio.

---

# Regra fundamental

O objetivo dos testes não é provar que o Front-end funciona apenas no cenário feliz.

O objetivo é garantir que:

> **O usuário consegue fazer aquilo que deveria conseguir fazer e não consegue executar, através da interface, fluxos que não fazem sentido para o negócio.**

Priorize comportamento observável, regras de negócio, estados, contratos e jornadas críticas.

Não busque 100% de coverage como objetivo principal.

Busque **alta confiança no comportamento correto da aplicação**.
