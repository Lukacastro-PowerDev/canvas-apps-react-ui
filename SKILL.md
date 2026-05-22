# canvas-apps-react-ui

> Skill de UI/UX para Power Apps Canvas Apps que imita **React + TypeScript** usando controles **HTML Text** puros. Componentes declarativos, props tipadas, hooks mapeados e design system completo — tudo sem JavaScript, apenas HTML + CSS + fórmulas Power Apps.

---

## Filosofia

Power Apps Canvas não executa JavaScript, mas seu controle **HTML Text** aceita HTML + CSS inline/embedded. Esta skill ensina a pensar em **componentes React-like**:

- Cada componente é uma **função HTML** que recebe **props** (variáveis Power Apps)
- Props são documentadas com **interfaces TypeScript-like** para clareza
- Estado é gerenciado por **variáveis de contexto** e **coleções** do Power Apps
- Efeitos colaterais usam **OnVisible**, **OnChange** e **timers** do Power Apps
- O JSX é simulado por **concatenação de strings HTML** no controle HTML Text
- O CSS vive em **variáveis CSS** dentro de uma tag `<style>` no HTML Text

> **Regra de ouro**: Se você consegue descrever em React, consegue implementar em Power Apps HTML Text.

---

## Como Funciona no Power Apps

### O Controle HTML Text

No Canvas Apps, adicione um controle **HTML Text** e defina sua propriedade `HtmlText` como uma string concatenada:

```powerapps
"<style>" &
App.Styles &  // Variável global com CSS base
"</style>" &
"<div class='app'>" &
  Header({
    title: "Dashboard",
    subtitle: "Visão geral",
    showBack: false,
    primaryAction: "Novo",
    userAvatar: User().Image
  }) &
  "<main class='main'>" &
    CardGrid({
      items: colKPIs,
      columns: If(Screen.Size = ScreenSize.Small, 1, If(Screen.Size = ScreenSize.Medium, 2, 3))
    }) &
  "</main>" &
"</div>"
```

### Props = Variáveis Power Apps

Cada "componente" é documentado com uma **interface TypeScript-like**. No Power Apps, você passa os valores via variáveis de contexto, coleções ou propriedades de controles:

| React/TS | Power Apps |
|----------|-----------|
| `props.title` | `varTitle` ou `ThisItem.Title` |
| `props.items.map(...)` | `Concat(colItems, ...)` |
| `useState(true)` | `UpdateContext({varIsOpen: true})` |
| `useEffect(() => {...}, [])` | `OnVisible` da tela |
| `onClick={() => setOpen(true)}` | `OnSelect` de um botão invisível + `UpdateContext` |
| `children` | Concatenação de strings dentro do componente |

---

## Estrutura da Skill

```
canvas-apps-react-ui/
├── SKILL.md                    ← Você está aqui
├── types/
│   └── interfaces.md           ← Interfaces TypeScript-like de props
├── components/
│   ├── AppShell.md             ← Container raiz (layout, CSS vars)
│   ├── Header.md               ← App bar com props
│   ├── Card.md                 ← Card genérico (métrica, lista, resumo)
│   ├── Button.md               ← Botões com variantes
│   ├── Input.md                ← Campo de input com label e erro
│   ├── Badge.md                ← Badge de status
│   ├── Modal.md                ← Modal/drawer
│   ├── EmptyState.md           ← Estado vazio
│   ├── Loading.md              ← Skeleton e spinners
│   ├── BottomNav.md            ← Navegação inferior
│   ├── DataTable.md            ← Tabela responsiva
│   └── FormGroup.md            ← Grupo de formulário
├── hooks/
│   └── power-apps-hooks.md     ← Mapeamento React hooks → Power Apps
├── utils/
│   ├── design-system.css       ← CSS base com variáveis
│   └── helpers.md              ← Funções helper (Concat, If, etc.)
├── screens/
│   ├── Dashboard.md            ← Tela Dashboard completa
│   ├── ListDetail.md           ← Tela Lista + Detalhes
│   └── FormScreen.md           ← Tela de Formulário
├── examples/
│   └── order-management.md     ← Exemplo end-to-end
└── templates/
    ├── screen-template.md      ← Template para nova tela
    └── component-template.md   ← Template para novo componente
```

---

## Design System (CSS Variables)

Todo o design system vive em variáveis CSS injetadas no `<style>` do HTML Text:

```css
:root {
  /* Cores */
  --primary: #2563eb;
  --primary-hover: #1d4ed8;
  --primary-light: #dbeafe;
  --success: #16a34a;
  --success-light: #d1fae5;
  --warning: #d97706;
  --warning-light: #fef3c7;
  --danger: #dc2626;
  --danger-light: #fee2e2;
  --info: #0891b2;
  --info-light: #cffafe;

  /* Superfícies */
  --surface: #ffffff;
  --surface-raised: #f9fafb;
  --surface-overlay: rgba(0,0,0,0.5);

  /* Texto */
  --text: #111827;
  --text-secondary: #374151;
  --text-muted: #6b7280;
  --text-inverse: #ffffff;

  /* Bordas */
  --border: #e5e7eb;
  --border-strong: #d1d5db;

  /* Espaçamento (escala 4px) */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;

  /* Radius */
  --radius-sm: 6px;
  --radius: 12px;
  --radius-lg: 16px;
  --radius-full: 9999px;

  /* Sombras */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow: 0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.07), 0 2px 4px rgba(0,0,0,0.05);
  --shadow-lg: 0 10px 15px rgba(0,0,0,0.1), 0 4px 6px rgba(0,0,0,0.05);

  /* Tipografia */
  --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, monospace;
}
```

---

## Padrões de Implementação

### 1. Componente como Função String

```powerapps
// No controle HTML Text, você "chama" componentes como concatenação
Header({title: varTitle, subtitle: varSubtitle})

// Na prática, é uma variável ou fórmula que retorna HTML:
Set(varHeaderHtml,
  "<header class='header'>" &
    "<div class='header__back'>" & If(varShowBack, "<button class='btn-icon'>←</button>", "") & "</div>" &
    "<div class='header__title-group'>" &
      "<h1 class='header__title'>" & varTitle & "</h1>" &
      If(!IsBlank(varSubtitle), "<span class='header__subtitle'>" & varSubtitle & "</span>", "") &
    "</div>" &
    "<div class='header__actions'>" &
      If(!IsBlank(varPrimaryAction),
        "<button class='btn btn--primary'>" & varPrimaryAction & "</button>", "") &
    "</div>" &
  "</header>"
)
```

### 2. Renderização Condicional (React: `{condition && <Component />}`)

```powerapps
// Power Apps: use If() como operador ternário
If(varIsLoading,
  Loading({count: 3}),
  If(CountRows(colItems) = 0,
    EmptyState({icon: "📭", title: "Nenhum item", description: "Crie seu primeiro registro."}),
    CardGrid({items: colItems})
  )
)
```

### 3. Mapeamento de Lista (React: `items.map(item => <Card {...item} />)`)

```powerapps
// Power Apps: use Concat() como .map()
Concat(colItems,
  Card({
    title: ThisItem.Title,
    subtitle: ThisItem.Description,
    badge: ThisItem.Status,
    badgeColor: If(ThisItem.Status = "Urgente", "danger", "info")
  })
)
```

### 4. Estado e Eventos

No Power Apps, estado vive em **variáveis de contexto** e eventos em **controles invisíveis**:

```powerapps
// OnSelect de um botão invisível sobre o HTML Text:
UpdateContext({varSelectedId: ThisItem.ID})

// OnVisible da tela (equivalente a useEffect com []):
ClearCollect(colItems, Filter(datasource, Active = true))

// Timer (equivalente a setInterval / useEffect com delay):
// Configure um Timer control com AutoStart e Repeat
```

---

## Regras de Ouro

1. **Todo componente tem uma interface** documentada em `types/interfaces.md`.
2. **Todo componente é puro**: não depende de estado global, recebe tudo via props.
3. **CSS é global via variáveis**: injetado uma vez no `<style>` do HTML Text.
4. **Mobile-first**: componentes usam `flex-direction: column` por padrão, `media queries` para desktop.
5. **Acessibilidade é requisito**: labels, contrastes, foco visível, áreas tocáveis ≥ 44px.
6. **Estados são obrigatórios**: loading, empty, error, success devem existir.

---

## Recursos

- [Power Apps HTML Text Control](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/controls/control-text-box)
- [Power Apps Concat Function](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/functions/function-concatenate)
- [Power Apps UpdateContext](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/functions/function-updatecontext)