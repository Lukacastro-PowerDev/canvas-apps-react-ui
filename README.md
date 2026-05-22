# canvas-apps-react-ui

> Skill de UI/UX para Power Apps Canvas Apps que imita **React + TypeScript** usando controles **HTML Text** puros. Componentes declarativos, props tipadas, hooks mapeados e design system completo — tudo sem JavaScript, apenas HTML + CSS + fórmulas Power Apps.

## 🎯 Filosofia

Power Apps Canvas não executa JavaScript, mas seu controle **HTML Text** aceita HTML + CSS inline/embedded. Esta skill ensina a pensar em **componentes React-like**:

- Cada componente é uma **função HTML** que recebe **props** (variáveis Power Apps)
- Props são documentadas com **interfaces TypeScript-like** para clareza
- Estado é gerenciado por **variáveis de contexto** e **coleções** do Power Apps
- Efeitos colaterais usam **OnVisible**, **OnChange** e **timers** do Power Apps
- O JSX é simulado por **concatenação de strings HTML** no controle HTML Text
- O CSS vive em **variáveis CSS** dentro de uma tag `<style>` no HTML Text

> **Regra de ouro**: Se você consegue descrever em React, consegue implementar em Power Apps HTML Text.

## 📁 Estrutura

```
canvas-apps-react-ui/
├── SKILL.md                          # Documento principal da skill
├── README.md                         # Este arquivo
├── types/
│   └── interfaces.md                 # Interfaces TypeScript-like de props
├── components/
│   ├── AppShell.md                   # Container raiz (layout, CSS vars)
│   ├── Header.md                     # App bar com props
│   ├── Card.md                       # Card genérico (métrica, lista, resumo)
│   ├── Button.md                     # Botões com variantes
│   ├── Input.md                      # Campo de input com label e erro
│   ├── Badge.md                      # Badge de status
│   ├── Modal.md                      # Modal/drawer
│   ├── EmptyState.md                 # Estado vazio
│   ├── Loading.md                    # Skeleton e spinners
│   ├── BottomNav.md                  # Navegação inferior mobile
│   ├── DataTable.md                  # Tabela responsiva
│   └── FormGroup.md                  # Grupo de formulário
├── hooks/
│   └── power-apps-hooks.md           # Mapeamento React hooks → Power Apps
├── utils/
│   ├── design-system.css             # CSS base completo com variáveis
│   └── helpers.md                    # Funções helper (Concat, If, etc.)
├── screens/
│   ├── Dashboard.md                  # Tela Dashboard completa
│   ├── ListDetail.md                 # Tela Lista + Detalhes
│   └── FormScreen.md                 # Tela de Formulário
├── examples/
│   └── order-management.md           # Exemplo end-to-end
└── templates/
    ├── screen-template.md            # Template para nova tela
    └── component-template.md         # Template para novo componente
```

## 🚀 Como Usar

### 1. Copie o CSS Global
Abra `utils/design-system.css` e copie o conteúdo para a variável `varGlobalStyles` no `OnStart` do seu app.

### 2. Crie Variáveis de Contexto
Use as interfaces em `types/interfaces.md` para definir as variáveis de cada tela.

### 3. Monte o HTML
Use os componentes em `components/` como referência para montar seu HTML Text.

### 4. Mapeie Hooks
Consulte `hooks/power-apps-hooks.md` para traduzir lógica React em fórmulas Power Apps.

### 5. Use os Templates
`templates/screen-template.md` e `templates/component-template.md` aceleram a criação.

## 🧩 Componentes Disponíveis

| Componente | React Equivalent | Descrição |
|-----------|-----------------|-----------|
| AppShell | `<App />` | Container raiz com CSS vars |
| Header | `<AppBar />` | Barra superior com título e ações |
| Card | `<Card />` | Container versátil (métrica, lista, resumo) |
| Button | `<Button />` | Botões com variantes e estados |
| Input | `<TextField />` | Campo de input com label e validação |
| Badge | `<Badge />` / `<Tag />` | Indicador de status |
| Modal | `<Dialog />` / `<Modal />` | Overlay com conteúdo |
| EmptyState | `<Empty />` | Estado vazio com CTA |
| Loading | `<Spin />` / `<Skeleton />` | Estados de carregamento |
| BottomNav | `<BottomNavigation />` | Navegação inferior mobile |
| DataTable | `<Table />` | Tabela responsiva |
| FormGroup | `<Form.Item />` | Grupo de formulário |

## 🎨 Design System

O design system usa variáveis CSS para:
- **Cores**: primária, semânticas (success, warning, danger, info)
- **Superfícies**: surface, surface-raised, surface-overlay
- **Texto**: text, text-secondary, text-muted, text-inverse
- **Espaçamento**: escala 4px (4, 8, 12, 16, 20, 24, 32, 40)
- **Radius**: sm (6px), base (12px), lg (16px), full (9999px)
- **Sombras**: sm, base, md, lg
- **Tipografia**: font-sans (system), font-mono

## 🔗 Mapeamento React → Power Apps

| React/TypeScript | Power Apps |
|-----------------|------------|
| `useState` | `UpdateContext` / `Set` |
| `useEffect` (mount) | `OnVisible` |
| `useEffect` (update) | `OnChange` / Timer |
| `useMemo` | Variável calculada |
| `useCallback` | `OnSelect` de controles |
| `props` | Variáveis de contexto |
| `children` | Concatenação de strings |
| `.map()` | `Concat()` / `ForAll()` |
| `.filter()` | `Filter()` |
| `.find()` | `LookUp()` |
| `onClick` | `OnSelect` |
| `onChange` | `OnChange` |

## 📱 Responsividade

- **Mobile** (< 640px): 1 coluna, bottom nav, modal em tela cheia
- **Tablet** (640-1024px): 2 colunas, sidebar colapsada
- **Desktop** (> 1024px): 3-4 colunas, sidebar expandida, painel de detalhes

## ♿ Acessibilidade

- Títulos semânticos (`<h1>`, `<h2>`, `<h3>`)
- Labels visíveis em todos os inputs
- Contraste ≥ 4.5:1
- Áreas tocáveis ≥ 44px
- Estados de erro com texto + ícone + cor
- Foco visível em todos os elementos interativos
- `aria-label`, `aria-describedby`, `aria-invalid`

## 📚 Recursos

- [Power Apps HTML Text Control](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/controls/control-text-box)
- [Power Apps Concat Function](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/functions/function-concatenate)
- [Power Apps UpdateContext](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/functions/function-updatecontext)
- [Material-UI](https://mui.com/) — Referência visual de componentes
- [Ant Design](https://ant.design/) — Referência visual de componentes

## 📝 Licença

MIT