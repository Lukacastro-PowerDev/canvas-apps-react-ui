# Componente: AppShell

> Container raiz da aplicação. Injeta o CSS base, define o layout flex e gerencia o tema.

## Interface

```typescript
interface AppShellProps {
  children: ReactNode;
  theme?: 'light' | 'dark';
  maxWidth?: number;
  padding?: boolean;
}
```

## Implementação no Power Apps

### Variável Global de CSS

Crie uma variável global `App.Styles` (ou `varGlobalStyles`) no `OnStart` do app:

```powerapps
Set(varGlobalStyles,
  "<style>" &
  ":root {" &
    "--primary: #2563eb;" &
    "--primary-hover: #1d4ed8;" &
    "--primary-light: #dbeafe;" &
    "--success: #16a34a;" &
    "--success-light: #d1fae5;" &
    "--warning: #d97706;" &
    "--warning-light: #fef3c7;" &
    "--danger: #dc2626;" &
    "--danger-light: #fee2e2;" &
    "--info: #0891b2;" &
    "--info-light: #cffafe;" &
    "--surface: #ffffff;" &
    "--surface-raised: #f9fafb;" &
    "--surface-overlay: rgba(0,0,0,0.5);" &
    "--text: #111827;" &
    "--text-secondary: #374151;" &
    "--text-muted: #6b7280;" &
    "--text-inverse: #ffffff;" &
    "--border: #e5e7eb;" &
    "--border-strong: #d1d5db;" &
    "--space-1: 4px;" &
    "--space-2: 8px;" &
    "--space-3: 12px;" &
    "--space-4: 16px;" &
    "--space-5: 20px;" &
    "--space-6: 24px;" &
    "--space-8: 32px;" &
    "--space-10: 40px;" &
    "--radius-sm: 6px;" &
    "--radius: 12px;" &
    "--radius-lg: 16px;" &
    "--radius-full: 9999px;" &
    "--shadow-sm: 0 1px 2px rgba(0,0,0,0.05);" &
    "--shadow: 0 1px 3px rgba(0,0,0,0.1), 0 1px 2px rgba(0,0,0,0.06);" &
    "--shadow-md: 0 4px 6px rgba(0,0,0,0.07), 0 2px 4px rgba(0,0,0,0.05);" &
    "--shadow-lg: 0 10px 15px rgba(0,0,0,0.1), 0 4px 6px rgba(0,0,0,0.05);" &
    "--font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;" &
    "--font-mono: ui-monospace, SFMono-Regular, monospace;" &
  "}" &
  "* { box-sizing: border-box; margin: 0; padding: 0; }" &
  "body { font-family: var(--font-sans); color: var(--text); background: var(--surface-raised); line-height: 1.5; -webkit-font-smoothing: antialiased; }" &
  ".app { display: flex; flex-direction: column; min-height: 100vh; background: var(--surface-raised); }" &
  ".app__main { flex: 1; padding: var(--space-4); overflow-y: auto; }" &
  ".app--dark { --surface: #1f2937; --surface-raised: #111827; --text: #f9fafb; --text-secondary: #e5e7eb; --text-muted: #9ca3af; --border: #374151; --border-strong: #4b5563; }" &
  "</style>"
)
```

### Uso no HTML Text

```powerapps
// Propriedade HtmlText do controle HTML Text:
"<style>" & varGlobalStyles & "</style>" &
"<div class='app " & If(varTheme = "dark", "app--dark", "") & "'>" &
  varHeaderHtml &
  "<main class='app__main'>" &
    varContentHtml &
  "</main>" &
  If(varShowBottomNav, varBottomNavHtml, "") &
"</div>"
```

## CSS Adicional do AppShell

```css
.app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  background: var(--surface-raised);
  font-family: var(--font-sans);
  color: var(--text);
}

.app__main {
  flex: 1;
  padding: var(--space-4);
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.app--dark {
  --surface: #1f2937;
  --surface-raised: #111827;
  --text: #f9fafb;
  --text-secondary: #e5e7eb;
  --text-muted: #9ca3af;
  --border: #374151;
  --border-strong: #4b5563;
}

/* Utilitários */
.flex { display: flex; }
.flex-col { flex-direction: column; }
.items-center { align-items: center; }
.justify-between { justify-content: space-between; }
.justify-center { justify-content: center; }
.gap-1 { gap: var(--space-1); }
.gap-2 { gap: var(--space-2); }
.gap-3 { gap: var(--space-3); }
.gap-4 { gap: var(--space-4); }
.w-full { width: 100%; }
.h-full { height: 100%; }
.text-center { text-align: center; }
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

## Dicas

- Defina `varGlobalStyles` no `OnStart` do app para carregar uma vez só.
- Use `varTheme` como variável de contexto para alternar entre light/dark.
- O `AppShell` não é um componente visual isolado — é o **wrapper da tela inteira**.