# Componente: Header

> App bar superior fixa com título, subtítulo e ações. Equivalente a um `<AppBar />` do Material-UI.

## Interface

```typescript
interface HeaderProps {
  title: string;
  subtitle?: string;
  showBack?: boolean;
  backLabel?: string;
  primaryAction?: string;
  primaryActionIcon?: string;
  secondaryActions?: Array<{ icon: string; label: string }>;
  userAvatar?: string;
  backgroundColor?: string;
  textColor?: string;
  elevated?: boolean;
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varHeaderHtml,
  "<header class='header " & If(varHeaderElevated, "header--elevated", "") & "' " &
  "style='background: " & Coalesce(varHeaderBg, "var(--primary)") & "; color: " & Coalesce(varHeaderTextColor, "var(--text-inverse)") & ";'>" &
    "<div class='header__left'>" &
      If(varShowBack,
        "<button class='btn-icon header__back' aria-label='" & varBackLabel & "'>" &
          "<span>←</span>" &
          If(!IsBlank(varBackLabel), "<span class='header__back-label'>" & varBackLabel & "</span>", "") &
        "</button>", "") &
      "<div class='header__title-group'>" &
        "<h1 class='header__title'>" & varHeaderTitle & "</h1>" &
        If(!IsBlank(varHeaderSubtitle), "<span class='header__subtitle'>" & varHeaderSubtitle & "</span>", "") &
      "</div>" &
    "</div>" &
    "<div class='header__right'>" &
      If(!IsBlank(varPrimaryAction),
        "<button class='btn btn--sm btn--primary-inverted'>" &
          If(!IsBlank(varPrimaryActionIcon), "<span class='btn__icon'>" & varPrimaryActionIcon & "</span>", "") &
          "<span>" & varPrimaryAction & "</span>" &
        "</button>", "") &
      If(!IsBlank(varUserAvatar),
        "<img class='header__avatar' src='" & varUserAvatar & "' alt='Avatar do usuário' />", "") &
    "</div>" &
  "</header>"
)
```

### CSS

```css
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-3) var(--space-4);
  min-height: 56px;
  position: sticky;
  top: 0;
  z-index: 50;
  transition: box-shadow 0.2s ease;
}

.header--elevated {
  box-shadow: var(--shadow-md);
}

.header__left {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  flex: 1;
  min-width: 0;
}

.header__back {
  display: flex;
  align-items: center;
  gap: var(--space-1);
  background: none;
  border: none;
  color: inherit;
  font-size: 20px;
  cursor: pointer;
  padding: var(--space-2);
  border-radius: var(--radius-sm);
  min-width: 40px;
  min-height: 40px;
  justify-content: center;
}

.header__back:hover {
  background: rgba(255,255,255,0.15);
}

.header__back-label {
  font-size: 14px;
  font-weight: 500;
}

.header__title-group {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.header__title {
  font-size: 18px;
  font-weight: 700;
  line-height: 1.2;
  margin: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header__subtitle {
  font-size: 13px;
  opacity: 0.85;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header__right {
  display: flex;
  align-items: center;
  gap: var(--space-2);
  flex-shrink: 0;
}

.header__avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid rgba(255,255,255,0.3);
}

.btn--primary-inverted {
  background: rgba(255,255,255,0.15);
  color: inherit;
  border: 1px solid rgba(255,255,255,0.3);
}

.btn--primary-inverted:hover {
  background: rgba(255,255,255,0.25);
}
```

## Props Mapeadas

| Prop | Variável Power Apps | Tipo |
|------|---------------------|------|
| title | `varHeaderTitle` | Text |
| subtitle | `varHeaderSubtitle` | Text |
| showBack | `varShowBack` | Boolean |
| backLabel | `varBackLabel` | Text |
| primaryAction | `varPrimaryAction` | Text |
| primaryActionIcon | `varPrimaryActionIcon` | Text |
| userAvatar | `varUserAvatar` | Text (URL) |
| backgroundColor | `varHeaderBg` | Text |
| textColor | `varHeaderTextColor` | Text |
| elevated | `varHeaderElevated` | Boolean |

## Acessibilidade

- `aria-label` no botão voltar.
- `<h1>` semântico para o título.
- Avatar com `alt` descritivo.
- Altura mínima de 56px para toque.