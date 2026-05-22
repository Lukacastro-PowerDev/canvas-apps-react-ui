# Componente: Input

> Campo de input com label, helper text, erro e ícones. Equivalente a `<TextField />` do Material-UI.

## Interface

```typescript
interface InputProps {
  name: string;
  label: string;
  type?: 'text' | 'email' | 'number' | 'password' | 'date' | 'search';
  placeholder?: string;
  value?: string;
  helperText?: string;
  error?: string;
  disabled?: boolean;
  required?: boolean;
  icon?: string;
  iconRight?: string;
  maxLength?: number;
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varInputHtml,
  "<div class='form-group'>" &
    "<label class='form-group__label' for='" & varInputName & "'>" &
      varInputLabel &
      If(varInputRequired, "<span class='form-group__required'> *</span>", "") &
    "</label>" &
    "<div class='form-group__input-wrapper'>" &
      If(!IsBlank(varInputIcon),
        "<span class='form-group__icon form-group__icon--left'>" & varInputIcon & "</span>", "") &
      "<input " &
        "id='" & varInputName & "' " &
        "name='" & varInputName & "' " &
        "type='" & Coalesce(varInputType, "text") & "' " &
        "class='input " & If(!IsBlank(varInputError), "input--error", "") & "' " &
        "placeholder='" & varInputPlaceholder & "' " &
        "value='" & varInputValue & "' " &
        If(varInputDisabled, "disabled ", "") &
        If(!IsBlank(varInputMaxLength), "maxlength='" & varInputMaxLength & "' ", "") &
        "aria-describedby='" & varInputName & "-helper' " &
        If(!IsBlank(varInputError), "aria-invalid='true' aria-errormessage='" & varInputName & "-error'", "") &
      "/>" &
      If(!IsBlank(varInputIconRight),
        "<span class='form-group__icon form-group__icon--right'>" & varInputIconRight & "</span>", "") &
    "</div>" &
    If(!IsBlank(varInputHelperText) && IsBlank(varInputError),
      "<span id='" & varInputName & "-helper' class='form-group__helper'>" & varInputHelperText & "</span>", "") &
    If(!IsBlank(varInputError),
      "<span id='" & varInputName & "-error' class='form-group__error' role='alert'>" &
        "⚠️ " & varInputError &
      "</span>", "") &
  "</div>"
)
```

> **Nota**: O input HTML em si não é interativo no Power Apps HTML Text. Para capturar dados, use um **Text Input** nativo do Power Apps posicionado sobre o HTML visual, ou use o controle nativo estilizado via CSS.

### CSS

```css
.form-group {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  margin-bottom: var(--space-4);
}

.form-group__label {
  font-size: 14px;
  font-weight: 600;
  color: var(--text);
  display: flex;
  align-items: center;
  gap: 4px;
}

.form-group__required {
  color: var(--danger);
}

.form-group__input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.form-group__icon {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 100%;
  color: var(--text-muted);
  font-size: 16px;
  pointer-events: none;
  z-index: 1;
}

.form-group__icon--left {
  left: 0;
}

.form-group__icon--right {
  right: 0;
}

.form-group__icon--left + .input {
  padding-left: 40px;
}

.form-group__icon--right ~ .input {
  padding-right: 40px;
}

.input {
  width: 100%;
  padding: var(--space-3);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  font-size: 16px; /* Previne zoom no iOS */
  font-family: var(--font-sans);
  color: var(--text);
  background: var(--surface);
  transition: border-color 0.2s, box-shadow 0.2s;
  line-height: 1.5;
}

.input::placeholder {
  color: var(--text-muted);
  opacity: 0.7;
}

.input:focus {
  outline: none;
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.15);
}

.input--error {
  border-color: var(--danger);
  background: var(--danger-light);
}

.input--error:focus {
  box-shadow: 0 0 0 3px rgba(220, 38, 38, 0.15);
}

.input:disabled {
  background: var(--surface-raised);
  color: var(--text-muted);
  cursor: not-allowed;
}

.form-group__helper {
  font-size: 13px;
  color: var(--text-muted);
  display: flex;
  align-items: center;
  gap: 4px;
  line-height: 1.4;
}

.form-group__error {
  font-size: 13px;
  color: var(--danger);
  display: flex;
  align-items: center;
  gap: 4px;
  font-weight: 500;
  line-height: 1.4;
}
```

## Integração com Input Nativo do Power Apps

Para inputs funcionais, posicione um **Text Input** nativo do Power Apps sobre o HTML visual:

```
┌────────────────────────────────────────┐
│  Label do Campo *                    │  ← HTML Text (visual)
│  ┌────────────────────────────────┐  │
│  │  [Text Input nativo]           │  │  ← Text Input (funcional)
│  │  (transparente, sem borda)     │  │
│  └────────────────────────────────┘  │
│  💡 Texto de ajuda                   │  ← HTML Text
└────────────────────────────────────────┘
```

Configure o Text Input nativo:
- **BorderThickness**: 0
- **Fill**: Transparent
- **Color**: var(--text) via fórmula
- **Y**: posição calculada sobre o input HTML
- **OnChange**: `UpdateContext({varInputValue: Self.Text})`

## Acessibilidade

- `<label for="id">` associado ao input.
- `aria-describedby` apontando para helper text.
- `aria-invalid="true"` e `aria-errormessage` quando há erro.
- `role="alert"` na mensagem de erro.
- Tamanho mínimo de 16px para prevenir zoom no iOS.
- Altura mínima de 44px para toque.