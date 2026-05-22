# Interfaces TypeScript-like

> Documentação de props para cada componente. No Power Apps, estas props se tornam variáveis de contexto, coleções ou propriedades de controles.

---

## Interface Base

```typescript
interface BaseProps {
  className?: string;       // Classes CSS adicionais
  style?: CSSProperties;    // Estilos inline adicionais
}
```

---

## AppShell

```typescript
interface AppShellProps extends BaseProps {
  children: ReactNode;      // Conteúdo da tela (HTML concatenado)
  theme?: 'light' | 'dark'; // Tema do app
  maxWidth?: number;          // Largura máxima em px (default: 100%)
}
```

**Mapeamento Power Apps:**
- `children` → concatenação de strings no HtmlText
- `theme` → variável `varTheme` ("light" | "dark")
- `maxWidth` → variável `varMaxWidth`

---

## Header

```typescript
interface HeaderProps extends BaseProps {
  title: string;                  // Título principal (obrigatório)
  subtitle?: string;              // Subtítulo descritivo
  showBack?: boolean;             // Exibe botão voltar (default: false)
  backLabel?: string;             // Label do botão voltar (default: "Voltar")
  primaryAction?: string;         // Label do botão primário
  primaryActionIcon?: string;       // Ícone do botão primário (emoji ou SVG)
  secondaryActions?: Array<{        // Ações secundárias
    icon: string;
    label: string;
    onClick: string;              // Nome da ação Power Apps
  }>;
  userAvatar?: string;            // URL da imagem do avatar
  backgroundColor?: string;         // Cor de fundo (default: --primary)
  textColor?: string;             // Cor do texto (default: --text-inverse)
  elevated?: boolean;             // Sombra ao rolar (default: true)
}
```

**Mapeamento Power Apps:**
- `title` → `varHeaderTitle`
- `subtitle` → `varHeaderSubtitle`
- `showBack` → `varShowBack`
- `primaryAction` → `varPrimaryActionLabel`
- `userAvatar` → `User().Image`

---

## Card

```typescript
type CardVariant = 'metric' | 'list' | 'summary' | 'media';

interface CardProps extends BaseProps {
  variant: CardVariant;
  title: string;
  subtitle?: string;
  description?: string;
  value?: number | string;          // Para variant='metric'
  delta?: number;                 // Variação percentual
  deltaDirection?: 'up' | 'down' | 'neutral';
  badge?: string;                 // Texto do badge
  badgeVariant?: 'success' | 'warning' | 'danger' | 'info' | 'neutral';
  icon?: string;                  // Emoji ou SVG
  image?: string;                 // URL da imagem (variant='media')
  actionIcon?: string;            // Ícone de ação (variant='list')
  actionLabel?: string;           // Label da ação
  onClick?: string;               // Nome da ação Power Apps
  disabled?: boolean;
  loading?: boolean;
}
```

**Mapeamento Power Apps:**
- `variant` → `varCardVariant`
- `title` → `ThisItem.Title`
- `badgeVariant` → `If(ThisItem.Status = "Urgente", "danger", "info")`
- `onClick` → controle invisível `btnCardAction` com `OnSelect`

---

## Button

```typescript
type ButtonVariant = 'primary' | 'secondary' | 'danger' | 'ghost' | 'icon';
type ButtonSize = 'sm' | 'md' | 'lg';

interface ButtonProps extends BaseProps {
  variant: ButtonVariant;
  size?: ButtonSize;              // Default: 'md'
  label: string;
  icon?: string;                  // Emoji ou SVG à esquerda
  iconRight?: string;             // Emoji ou SVG à direita
  disabled?: boolean;
  loading?: boolean;
  fullWidth?: boolean;            // Ocupa largura total
  onClick?: string;               // Nome da ação Power Apps
}
```

**Mapeamento Power Apps:**
- `variant` → classe CSS (`btn--primary`, `btn--secondary`, etc.)
- `label` → texto do botão
- `disabled` → `If(varIsSubmitting, "disabled", "")`
- `loading` → `If(varIsLoading, "<span class='spinner'></span>", "")`

---

## Input

```typescript
type InputType = 'text' | 'email' | 'number' | 'password' | 'date' | 'search';

interface InputProps extends BaseProps {
  name: string;                   // Nome do campo (para ID)
  label: string;                  // Label visível
  type?: InputType;               // Default: 'text'
  placeholder?: string;
  value?: string;                 // Valor atual
  helperText?: string;            // Texto de ajuda
  error?: string;                 // Mensagem de erro
  disabled?: boolean;
  required?: boolean;             // Exibe asterisco
  icon?: string;                  // Ícone dentro do input
  iconRight?: string;             // Ícone à direita
  autoFocus?: boolean;
  maxLength?: number;
}
```

**Mapeamento Power Apps:**
- `name` → `varInputName`
- `value` → `varInputValue`
- `error` → `varInputError`
- `onChange` → não existe em HTML Text; usar controle Text Input nativo sobreposto

---

## Badge

```typescript
type BadgeVariant = 'success' | 'warning' | 'danger' | 'info' | 'neutral';
type BadgeSize = 'sm' | 'md';

interface BadgeProps extends BaseProps {
  children: string;              // Texto do badge
  variant?: BadgeVariant;         // Default: 'neutral'
  size?: BadgeSize;               // Default: 'md'
  dot?: boolean;                  // Ponto indicador à esquerda
  pulse?: boolean;                // Animação de pulso
}
```

**Mapeamento Power Apps:**
- `children` → texto do status
- `variant` → classe CSS (`badge--success`, `badge--danger`, etc.)
- `dot` + `pulse` → `<span class='badge__dot badge__dot--pulse'></span>`

---

## Modal

```typescript
interface ModalProps extends BaseProps {
  open: boolean;                  // Controla visibilidade
  title: string;
  description?: string;
  size?: 'sm' | 'md' | 'lg' | 'full';  // Default: 'md'
  closeButton?: boolean;          // Default: true
  footer?: ReactNode;               // HTML do footer (botões)
  children: ReactNode;             // Conteúdo do modal
  onClose?: string;               // Ação ao fechar
  backdrop?: boolean;               // Fundo escuro (default: true)
}
```

**Mapeamento Power Apps:**
- `open` → `varModalOpen` (boolean)
- `onClose` → `UpdateContext({varModalOpen: false})`
- `footer` → concatenação de botões HTML
- `children` → HTML do conteúdo

---

## EmptyState

```typescript
interface EmptyStateProps extends BaseProps {
  icon: string;                   // Emoji ou SVG (64px)
  title: string;
  description?: string;
  actionLabel?: string;           // Label do CTA
  actionIcon?: string;            // Ícone do CTA
  secondaryAction?: string;       // Label de ação secundária
  size?: 'sm' | 'lg';             // Default: 'lg'
}
```

**Mapeamento Power Apps:**
- `icon` → emoji ou SVG inline
- `actionLabel` → texto do botão primário
- `size` → classe CSS (`empty-state--sm` | `empty-state--lg`)

---

## Loading

```typescript
type LoadingVariant = 'spinner' | 'skeleton' | 'dots';

interface LoadingProps extends BaseProps {
  variant: LoadingVariant;
  count?: number;                 // Para skeleton: quantos itens (default: 3)
  text?: string;                  // Texto explicativo
  size?: 'sm' | 'md' | 'lg';      // Para spinner
  fullScreen?: boolean;           // Overlay em tela cheia
}
```

**Mapeamento Power Apps:**
- `variant` → classe CSS
- `count` → `ForAll([1,2,3], ...)` ou `Concat(Sequence(3), ...)
- `fullScreen` → `position: fixed; inset: 0;`

---

## BottomNav

```typescript
interface BottomNavItem {
  icon: string;                   // Emoji ou SVG
  label: string;
  active?: boolean;
  badge?: number;                 // Contador de notificações
  onClick?: string;               // Ação Power Apps
}

interface BottomNavProps extends BaseProps {
  items: BottomNavItem[];
  activeIndex?: number;           // Índice do item ativo
}
```

**Mapeamento Power Apps:**
- `items` → coleção `colNavItems`
- `activeIndex` → `varActiveTab`

---

## DataTable

```typescript
interface DataTableColumn {
  key: string;                    // Nome da propriedade do item
  header: string;                 // Texto do cabeçalho
  width?: string;                 // Largura da coluna
  align?: 'left' | 'center' | 'right';
  format?: 'text' | 'date' | 'currency' | 'badge';
  badgeVariant?: BadgeVariant;     // Para format='badge'
}

interface DataTableProps extends BaseProps {
  columns: DataTableColumn[];
  items: any[];
  emptyState?: EmptyStateProps;
  loading?: boolean;
  sortable?: boolean;
  onRowClick?: string;
}
```

**Mapeamento Power Apps:**
- `columns` → coleção `colTableColumns`
- `items` → coleção `colTableItems`
- `onRowClick` → controle invisível sobre a linha

---

## FormGroup

```typescript
interface FormGroupProps extends BaseProps {
  label: string;
  required?: boolean;
  helperText?: string;
  error?: string;
  children: ReactNode;             // Input HTML
}
```

**Mapeamento Power Apps:**
- `children` → HTML do input (native ou HTML Text)
- `error` → `varFieldError`

---

## Tipos Utilitários

```typescript
// Para cores semânticas
type SemanticColor = 'primary' | 'success' | 'warning' | 'danger' | 'info' | 'neutral';

// Para tamanhos
type Size = 'xs' | 'sm' | 'md' | 'lg' | 'xl';

// Para espaçamento (escala 4px)
type Spacing = 1 | 2 | 3 | 4 | 5 | 6 | 8 | 10 | 12 | 16 | 20 | 24;
```