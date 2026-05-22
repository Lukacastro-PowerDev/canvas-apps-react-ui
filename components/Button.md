# Componente: Button

> Botão com variantes, tamanhos e estados. Equivalente a `<Button />` do Material-UI ou Ant Design.

## Interface

```typescript
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'danger' | 'ghost' | 'icon';
  size?: 'sm' | 'md' | 'lg';
  label: string;
  icon?: string;
  iconRight?: string;
  disabled?: boolean;
  loading?: boolean;
  fullWidth?: boolean;
}
```

## Implementação

### Variável Power Apps

```powerapps
// Função helper para gerar botão HTML:
Button({variant: "primary", size: "md", label: "Salvar", icon: "💾"})

// Na prática (variável):
Set(varBtnSave,
  "<button class='btn btn--" & varBtnVariant & " btn--" & varBtnSize &
  If(varBtnFullWidth, " btn--full", "") &
  If(varBtnDisabled, " btn--disabled", "") & "' " &
  If(varBtnDisabled, "disabled", "") & " type='button'>" &
    If(varBtnLoading,
      "<span class='btn__spinner'></span><span class='btn__label'>" & varBtnLabel & "</span>",
      If(!IsBlank(varBtnIcon), "<span class='btn__icon'>" & varBtnIcon & "</span>", "") &
      "<span class='btn__label'>" & varBtnLabel & "</span>" &
      If(!IsBlank(varBtnIconRight), "<span class='btn__icon-right'>" & varBtnIconRight & "</span>", "")
    ) &
  "</button>"
)
```

### CSS

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-2);
  font-family: var(--font-sans);
  font-weight: 600;
  line-height: 1.5;
  border: 1px solid transparent;
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all 0.2s ease;
  text-decoration: none;
  white-space: nowrap;
  user-select: none;
}

/* Tamanhos */
.btn--sm {
  padding: var(--space-1) var(--space-3);
  font-size: 13px;
  min-height: 32px;
}

.btn--md {
  padding: var(--space-2) var(--space-4);
  font-size: 14px;
  min-height: 40px;
}

.btn--lg {
  padding: var(--space-3) var(--space-6);
  font-size: 16px;
  min-height: 48px;
}

/* Variantes */
.btn--primary {
  background: var(--primary);
  color: var(--text-inverse);
}

.btn--primary:hover:not(:disabled) {
  background: var(--primary-hover);
  transform: translateY(-1px);
  box-shadow: var(--shadow);
}

.btn--secondary {
  background: var(--surface);
  color: var(--text-secondary);
  border-color: var(--border);
}

.btn--secondary:hover:not(:disabled) {
  background: var(--surface-raised);
  border-color: var(--border-strong);
}

.btn--danger {
  background: var(--danger);
  color: var(--text-inverse);
}

.btn--danger:hover:not(:disabled) {
  filter: brightness(0.9);
}

.btn--ghost {
  background: transparent;
  color: var(--text-secondary);
}

.btn--ghost:hover:not(:disabled) {
  background: var(--surface-raised);
}

.btn--icon {
  background: transparent;
  color: var(--text-muted);
  padding: var(--space-2);
  min-width: 40px;
  min-height: 40px;
  border-radius: var(--radius-sm);
}

.btn--icon:hover:not(:disabled) {
  background: var(--surface-raised);
  color: var(--text);
}

/* Estados */
.btn--full {
  width: 100%;
}

.btn:disabled,
.btn--disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none !important;
  box-shadow: none !important;
}

.btn__spinner {
  width: 16px;
  height: 16px;
  border: 2px solid currentColor;
  border-right-color: transparent;
  border-radius: 50%;
  animation: spin 0.75s linear infinite;
  display: inline-block;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.btn__icon,
.btn__icon-right {
  font-size: 16px;
  display: inline-flex;
  align-items: center;
}

/* Foco visível */
.btn:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.3);
}

.btn--danger:focus-visible {
  box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.3);
}
```

## Uso em Grupos

```css
.btn-group {
  display: flex;
  gap: var(--space-2);
  flex-wrap: wrap;
}

.btn-group--right {
  justify-content: flex-end;
}

.btn-group--center {
  justify-content: center;
}

.btn-group--full .btn {
  flex: 1;
}
```

## Acessibilidade

- `type="button"` em todos os botões (evita submit acidental).
- `disabled` atributo HTML quando desabilitado.
- Foco visível com anel de 3px.
- Altura mínima de 32px (sm), 40px (md), 48px (lg).
- Spinner com `aria-hidden` implícito (elemento decorativo).