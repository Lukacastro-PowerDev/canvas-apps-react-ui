# Componente: FormGroup

> Wrapper de formulário que combina label, input visual, helper text e erro. Equivalente a `<Form.Item />` do Ant Design.

## Interface

```typescript
interface FormGroupProps {
  label: string;
  required?: boolean;
  helperText?: string;
  error?: string;
  children: ReactNode;
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varFormGroupHtml,
  "<div class='form-group " & If(!IsBlank(varFormError), "form-group--error", "") & "'>" &
    "<label class='form-group__label' for='" & varFormFieldId & "'>" &
      varFormLabel &
      If(varFormRequired, "<span class='form-group__required' aria-hidden='true'> *</span>", "") &
    "</label>" &
    "<div class='form-group__field'>" &
      varFormFieldHtml &  // Input HTML visual ou nativo
    "</div>" &
    If(!IsBlank(varFormHelperText) && IsBlank(varFormError),
      "<span id='" & varFormFieldId & "-helper' class='form-group__helper'>💡 " & varFormHelperText & "</span>", "") &
    If(!IsBlank(varFormError),
      "<span id='" & varFormFieldId & "-error' class='form-group__error' role='alert'>⚠️ " & varFormError & "</span>", "") &
  "</div>"
)
```

### CSS

```css
.form-group {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  margin-bottom: var(--space-4);
}

.form-group--error {
  animation: shake 0.4s ease-in-out;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-4px); }
  75% { transform: translateX(4px); }
}

.form-group__label {
  font-size: 14px;
  font-weight: 600;
  color: var(--text);
  display: flex;
  align-items: center;
  gap: 4px;
  line-height: 1.4;
}

.form-group__required {
  color: var(--danger);
}

.form-group__field {
  position: relative;
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

/* Seção de formulário */
.form-section {
  margin-bottom: var(--space-6);
}

.form-section__title {
  font-size: 16px;
  font-weight: 700;
  color: var(--text);
  margin-bottom: var(--space-4);
  padding-bottom: var(--space-2);
  border-bottom: 1px solid var(--border);
}

.form-section__grid {
  display: grid;
  gap: var(--space-4);
  grid-template-columns: 1fr;
}

@media (min-width: 640px) {
  .form-section__grid--2 {
    grid-template-columns: repeat(2, 1fr);
  }
}
```

## Uso em Conjunto

```powerapps
// Seção de Dados Pessoais
Set(varSectionPersonal,
  "<div class='form-section'>" &
    "<h3 class='form-section__title'>Dados Pessoais</h3>" &
    "<div class='form-section__grid form-section__grid--2'>" &
      FormGroup({label: "Nome", required: true, error: varNameError, children: Input({name: "name"})}) &
      FormGroup({label: "Email", required: true, error: varEmailError, children: Input({name: "email", type: "email"})}) &
    "</div>" &
  "</div>"
)
```

## Acessibilidade

- Label com `for` apontando para o input.
- Helper text com `id` único e `aria-describedby` no input.
- Erro com `role="alert"` e `aria-errormessage`.
- Animação de shake sutil para erro (respeita `prefers-reduced-motion`).