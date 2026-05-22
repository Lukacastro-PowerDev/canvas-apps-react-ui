# Screen: Formulário

> Tela de formulário com seções, validação visual e ações. Equivalente a um `<Form />` do React Hook Form + Material-UI.

## Interface

```typescript
interface FormScreenProps {
  mode: 'create' | 'edit' | 'view';
  title: string;
  subtitle?: string;
  sections: Array<{
    title: string;
    fields: Array<{
      name: string;
      label: string;
      type: 'text' | 'email' | 'number' | 'date' | 'dropdown' | 'textarea';
      required?: boolean;
      placeholder?: string;
      helperText?: string;
      options?: Array<{value: string; label: string}>;
      validation?: {
        minLength?: number;
        maxLength?: number;
        pattern?: string;
        message: string;
      };
    }>;
  }>;
  isSubmitting: boolean;
  errors: Record<string, string>;
}
```

## Implementação

### OnVisible

```powerapps
UpdateContext({
  varFormMode: "create",  // ou "edit" ou "view"
  varFormTitle: "Nova Ordem de Serviço",
  varFormSubtitle: "Preencha os dados abaixo",
  varIsSubmitting: false,
  varFormErrors: {},
  varShowConfirmModal: false
});

// Inicializar valores (modo edit)
If(varFormMode = "edit",
  UpdateContext({
    varFieldTitle: LookUp(datasource, ID = varEditId).Title,
    varFieldDescription: LookUp(datasource, ID = varEditId).Description,
    varFieldPriority: LookUp(datasource, ID = varEditId).Priority,
    varFieldDueDate: LookUp(datasource, ID = varEditId).DueDate,
    varFieldAssignee: LookUp(datasource, ID = varEditId).Assignee
  })
)
```

### Validação

```powerapps
// Função de validação (em um botão invisível ou OnSelect):
Set(varIsValid, true);
ClearCollect(colFormErrors, []);

// Validar título
If(IsBlank(varFieldTitle),
  Collect(colFormErrors, {Field: "title", Message: "Título é obrigatório"});
  Set(varIsValid, false)
);

If(Len(varFieldTitle) < 3,
  Collect(colFormErrors, {Field: "title", Message: "Título deve ter pelo menos 3 caracteres"});
  Set(varIsValid, false)
);

// Validar descrição
If(IsBlank(varFieldDescription),
  Collect(colFormErrors, {Field: "description", Message: "Descrição é obrigatória"});
  Set(varIsValid, false)
);

// Validar prioridade
If(IsBlank(varFieldPriority),
  Collect(colFormErrors, {Field: "priority", Message: "Selecione uma prioridade"});
  Set(varIsValid, false)
);

// Validar data
If(varFieldDueDate < Today(),
  Collect(colFormErrors, {Field: "dueDate", Message: "Data deve ser futura"});
  Set(varIsValid, false)
);

// Se válido, mostrar confirmação
If(varIsValid,
  UpdateContext({varShowConfirmModal: true})
)
```

### HTML Text — Formulário Completo

```powerapps
"<style>" & varGlobalStyles & "</style>" &
"<div class='app'>" &
  // Header
  "<header class='header' style='background: var(--primary); color: white;'>" &
    "<div class='header__left'>" &
      "<button class='btn-icon header__back' aria-label='Voltar'>←</button>" &
      "<div class='header__title-group'>" &
        "<h1 class='header__title'>" & varFormTitle & "</h1>" &
        "<span class='header__subtitle'>" & varFormSubtitle & "</span>" &
      "</div>" &
    "</div>" &
    "<div class='header__right'>" &
      If(varFormMode <> "view",
        "<button class='btn btn--sm' style='background: rgba(255,255,255,0.15); color: white;'>" &
          If(varIsSubmitting, "<span class='btn__spinner'></span>", "💾 Salvar") &
        "</button>",
        ""
      ) &
    "</div>" &
  "</header>" &

  // Main Form
  "<main class='app__main'>" &
    "<form class='form' onsubmit='return false;'>" &

      // Seção: Informações Básicas
      "<div class='form-section'>" &
        "<h2 class='form-section__title'>Informações Básicas</h2>" &
        "<div class='form-section__grid form-section__grid--2'>" &

          // Campo: Título
          "<div class='form-group " & If(!IsBlank(LookUp(colFormErrors, Field = "title").Message), "form-group--error", "") & "'>" &
            "<label class='form-group__label' for='field-title'>Título <span class='form-group__required'>*</span></label>" &
            "<div class='form-group__input-wrapper'>" &
              "<input id='field-title' name='title' type='text' class='input " &
                If(!IsBlank(LookUp(colFormErrors, Field = "title").Message), "input--error", "") &
              "' placeholder='Ex: Manutenção preventiva' value='" & varFieldTitle & "' />" &
            "</div>" &
            If(!IsBlank(LookUp(colFormErrors, Field = "title").Message),
              "<span class='form-group__error' role='alert'>⚠️ " & LookUp(colFormErrors, Field = "title").Message & "</span>",
              "<span class='form-group__helper'>💡 Nomeie a ordem de forma clara</span>"
            ) &
          "</div>" &

          // Campo: Prioridade
          "<div class='form-group " & If(!IsBlank(LookUp(colFormErrors, Field = "priority").Message), "form-group--error", "") & "'>" &
            "<label class='form-group__label' for='field-priority'>Prioridade <span class='form-group__required'>*</span></label>" &
            "<div class='form-group__input-wrapper'>" &
              "<select id='field-priority' name='priority' class='input " &
                If(!IsBlank(LookUp(colFormErrors, Field = "priority").Message), "input--error", "") &
              "'>" &
                "<option value='' " & If(IsBlank(varFieldPriority), "selected", "") & ">Selecione...</option>" &
                "<option value='Baixa' " & If(varFieldPriority = "Baixa", "selected", "") & ">🟢 Baixa</option>" &
                "<option value='Média' " & If(varFieldPriority = "Média", "selected", "") & ">🟡 Média</option>" &
                "<option value='Alta' " & If(varFieldPriority = "Alta", "selected", "") & ">🔴 Alta</option>" &
                "<option value='Crítica' " & If(varFieldPriority = "Crítica", "selected", "") & ">🔴 Crítica</option>" &
              "</select>" &
            "</div>" &
            If(!IsBlank(LookUp(colFormErrors, Field = "priority").Message),
              "<span class='form-group__error' role='alert'>⚠️ " & LookUp(colFormErrors, Field = "priority").Message & "</span>",
              ""
            ) &
          "</div>" &

        "</div>" &
      "</div>" &

      // Seção: Detalhes
      "<div class='form-section'>" &
        "<h2 class='form-section__title'>Detalhes</h2>" &
        "<div class='form-section__grid'>" &

          // Campo: Descrição
          "<div class='form-group " & If(!IsBlank(LookUp(colFormErrors, Field = "description").Message), "form-group--error", "") & "'>" &
            "<label class='form-group__label' for='field-description'>Descrição <span class='form-group__required'>*</span></label>" &
            "<textarea id='field-description' name='description' rows='4' class='input " &
              If(!IsBlank(LookUp(colFormErrors, Field = "description").Message), "input--error", "") &
            "' placeholder='Descreva o problema ou tarefa...'>" & varFieldDescription & "</textarea>" &
            If(!IsBlank(LookUp(colFormErrors, Field = "description").Message),
              "<span class='form-group__error' role='alert'>⚠️ " & LookUp(colFormErrors, Field = "description").Message & "</span>",
              "<span class='form-group__helper'>💡 Seja específico para facilitar o atendimento</span>"
            ) &
          "</div>" &

        "</div>" &
        "<div class='form-section__grid form-section__grid--2'>" &

          // Campo: Data de Entrega
          "<div class='form-group " & If(!IsBlank(LookUp(colFormErrors, Field = "dueDate").Message), "form-group--error", "") & "'>" &
            "<label class='form-group__label' for='field-dueDate'>Data de Entrega <span class='form-group__required'>*</span></label>" &
            "<div class='form-group__input-wrapper'>" &
              "<span class='form-group__icon form-group__icon--left'>📅</span>" &
              "<input id='field-dueDate' name='dueDate' type='date' class='input " &
                If(!IsBlank(LookUp(colFormErrors, Field = "dueDate").Message), "input--error", "") &
              "' value='" & Text(varFieldDueDate, "yyyy-mm-dd") & "' style='padding-left: 40px;' />" &
            "</div>" &
            If(!IsBlank(LookUp(colFormErrors, Field = "dueDate").Message),
              "<span class='form-group__error' role='alert'>⚠️ " & LookUp(colFormErrors, Field = "dueDate").Message & "</span>",
              ""
            ) &
          "</div>" &

          // Campo: Responsável
          "<div class='form-group'>" &
            "<label class='form-group__label' for='field-assignee'>Responsável</label>" &
            "<div class='form-group__input-wrapper'>" &
              "<span class='form-group__icon form-group__icon--left'>👤</span>" &
              "<input id='field-assignee' name='assignee' type='text' class='input' placeholder='Nome do responsável' value='" & varFieldAssignee & "' style='padding-left: 40px;' />" &
            "</div>" &
            "<span class='form-group__helper'>💡 Deixe em branco para atribuir automaticamente</span>" &
          "</div>" &

        "</div>" &
      "</div>" &

      // Ações do Formulário
      If(varFormMode <> "view",
        "<div class='form-actions'>" &
          "<button type='button' class='btn btn--ghost'>Cancelar</button>" &
          "<button type='submit' class='btn btn--primary btn--lg' " & If(varIsSubmitting, "disabled", "") & ">" &
            If(varIsSubmitting,
              "<span class='btn__spinner'></span><span>Salvando...</span>",
              "💾 Salvar Ordem"
            ) &
          "</button>" &
        "</div>",
        ""
      ) &

    "</form>" &
  "</main>" &
"</div>"
```

### CSS Adicional do Formulário

```css
/* Formulário */
.form {
  max-width: 720px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
}

/* Seções */
.form-section {
  background: var(--surface);
  border-radius: var(--radius);
  border: 1px solid var(--border);
  padding: var(--space-5);
  box-shadow: var(--shadow-sm);
}

.form-section__title {
  font-size: 16px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 var(--space-5);
  padding-bottom: var(--space-3);
  border-bottom: 1px solid var(--border);
}
```

## Acessibilidade
- `<form>` com `onsubmit="return false;"`.
- `aria-invalid` e `role="alert"` nos erros.
- Labels explícitos com `for`.
- Inputs funcionais devem ser nativos do Power Apps sobrepostos.