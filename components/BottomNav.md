# Componente: BottomNav

> Navegação inferior para apps mobile. Equivalente a `<BottomNavigation />` do Material-UI.

## Interface

```typescript
interface BottomNavItem {
  icon: string;
  label: string;
  active?: boolean;
  badge?: number;
}

interface BottomNavProps {
  items: BottomNavItem[];
  activeIndex: number;
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varBottomNavHtml,
  "<nav class='bottom-nav' role='tablist'>" &
    Concat(colNavItems,
      "<button " &
        "class='bottom-nav__item " & If(ThisItem.Index = varActiveTab, "bottom-nav__item--active", "") & "' " &
        "role='tab' " &
        "aria-selected='" & If(ThisItem.Index = varActiveTab, "true", "false") & "' " &
        "aria-label='" & ThisItem.Label & "'" &
      ">" &
        "<span class='bottom-nav__icon-wrapper'>" &
          ThisItem.Icon &
          If(ThisItem.Badge > 0,
            "<span class='bottom-nav__badge'>" & ThisItem.Badge & "</span>", "") &
        "</span>" &
        "<span class='bottom-nav__label'>" & ThisItem.Label & "</span>" &
      "</button>"
    ) &
  "</nav>"
)
```

### CSS

```css
.bottom-nav {
  display: flex;
  align-items: center;
  justify-content: space-around;
  background: var(--surface);
  border-top: 1px solid var(--border);
  padding: var(--space-1) 0 calc(var(--space-1) + env(safe-area-inset-bottom));
  position: sticky;
  bottom: 0;
  z-index: 50;
  box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
}

.bottom-nav__item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  padding: var(--space-1) var(--space-2);
  background: none;
  border: none;
  color: var(--text-muted);
  font-family: var(--font-sans);
  font-size: 12px;
  cursor: pointer;
  min-width: 64px;
  min-height: 48px;
  flex: 1;
  transition: color 0.2s ease;
  position: relative;
}

.bottom-nav__item:hover {
  color: var(--text-secondary);
}

.bottom-nav__item--active {
  color: var(--primary);
}

.bottom-nav__icon-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
  line-height: 1;
  width: 28px;
  height: 28px;
}

.bottom-nav__label {
  font-size: 11px;
  font-weight: 500;
  line-height: 1.2;
  white-space: nowrap;
}

.bottom-nav__badge {
  position: absolute;
  top: -4px;
  right: -8px;
  min-width: 18px;
  height: 18px;
  padding: 0 5px;
  background: var(--danger);
  color: var(--text-inverse);
  font-size: 11px;
  font-weight: 700;
  border-radius: var(--radius-full);
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 1;
}
```

## Coleção de Itens

```powerapps
// OnStart ou OnVisible:
ClearCollect(colNavItems, [
  {Index: 0, Icon: "🏠", Label: "Início", Badge: 0},
  {Index: 1, Icon: "🔍", Label: "Buscar", Badge: 0},
  {Index: 2, Icon: "➕", Label: "Novo", Badge: 0},
  {Index: 3, Icon: "🔔", Label: "Alertas", Badge: 3},
  {Index: 4, Icon: "👤", Label: "Perfil", Badge: 0}
])
```

## Troca de Aba

```powerapps
// OnSelect do controle invisível sobre cada item:
UpdateContext({varActiveTab: ThisItem.Index})

// Visibilidade das telas:
scrHome.Visible = varActiveTab = 0
scrSearch.Visible = varActiveTab = 1
// ...
```

## Acessibilidade

- `role="tablist"` no nav, `role="tab"` em cada item.
- `aria-selected` indicando o item ativo.
- `aria-label` em cada botão.
- Badge com número legível (não apenas cor).
- Altura mínima de 48px para toque.