# Exemplo Completo: Gestão de Ordens de Serviço

> Aplicativo end-to-end usando canvas-apps-react-ui. Dashboard → Lista → Detalhes → Formulário.

## Estrutura do App

```
App (OnStart)
├── scrDashboard    → Dashboard com KPIs e atividades recentes
├── scrList         → Lista de ordens com filtros e busca
├── scrDetail       → Detalhes da ordem (modal/painel)
├── scrFormCreate   → Formulário de criação
├── scrFormEdit     → Formulário de edição
└── Components
    ├── cmpHeader     → Componente reutilizável de Header
    ├── cmpCard       → Componente reutilizável de Card
    └── cmpEmptyState → Componente reutilizável de EmptyState
```

## OnStart do App

```powerapps
// Design System CSS Global
Set(varGlobalStyles,
  "<style>" &
  ":root {" &
    "--primary: #2563eb; --primary-hover: #1d4ed8; --primary-light: #dbeafe;" &
    "--success: #16a34a; --success-light: #d1fae5;" &
    "--warning: #d97706; --warning-light: #fef3c7;" &
    "--danger: #dc2626; --danger-light: #fee2e2;" &
    "--info: #0891b2; --info-light: #cffafe;" &
    "--surface: #ffffff; --surface-raised: #f9fafb; --surface-overlay: rgba(0,0,0,0.5);" &
    "--text: #111827; --text-secondary: #374151; --text-muted: #6b7280; --text-inverse: #ffffff;" &
    "--border: #e5e7eb; --border-strong: #d1d5db;" &
    "--space-1: 4px; --space-2: 8px; --space-3: 12px; --space-4: 16px; --space-5: 20px; --space-6: 24px; --space-8: 32px;" &
    "--radius-sm: 6px; --radius: 12px; --radius-lg: 16px; --radius-full: 9999px;" &
    "--shadow-sm: 0 1px 2px rgba(0,0,0,0.05); --shadow: 0 1px 3px rgba(0,0,0,0.1);" &
    "--shadow-md: 0 4px 6px rgba(0,0,0,0.07); --shadow-lg: 0 10px 15px rgba(0,0,0,0.1);" &
    "--font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;" &
  "}" &
  "* { box-sizing: border-box; margin: 0; padding: 0; }" &
  "body { font-family: var(--font-sans); color: var(--text); background: var(--surface-raised); line-height: 1.5; -webkit-font-smoothing: antialiased; }" &
  ".app { display: flex; flex-direction: column; min-height: 100vh; }" &
  ".app__main { flex: 1; padding: var(--space-4); overflow-y: auto; }" &
  ".flex { display: flex; } .flex-col { flex-direction: column; } .items-center { align-items: center; }" &
  ".justify-between { justify-content: space-between; } .justify-center { justify-content: center; }" &
  ".gap-1 { gap: var(--space-1); } .gap-2 { gap: var(--space-2); } .gap-3 { gap: var(--space-3); } .gap-4 { gap: var(--space-4); }" &
  ".w-full { width: 100%; } .h-full { height: 100%; } .text-center { text-align: center; }" &
  ".truncate { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }" &
  "</style>"
);

// Navegação
Set(varActiveScreen, "dashboard")
```

## Fluxo de Navegação

```
scrDashboard ──[Nova Ordem]──→ scrFormCreate
     │                              │
     │                              │ [Salvar]
     │                              ▼
     │                         scrDashboard (refresh)
     │
     ├──[Ver todas]──→ scrList
     │                      │
     │                      ├──[Item]──→ scrDetail (modal/painel)
     │                      │              │
     │                      │              ├──[Editar]──→ scrFormEdit
     │                      │              └──[Concluir]──→ refresh
     │                      │
     │                      └──[Nova]──→ scrFormCreate
     │
     └──[Item recente]──→ scrDetail
```

## Acessibilidade no App

- Todas as telas têm `<h1>` no header.
- Cards de lista usam `<article>`.
- Badges sempre com texto.
- Estados de loading, empty e error em todas as telas.
- Contraste verificado em todos os textos.
- Inputs nativos sobrepostos para funcionalidade.