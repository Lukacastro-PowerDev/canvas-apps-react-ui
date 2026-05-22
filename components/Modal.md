# Componente: Modal

> Dialog/Modal overlay com header, conteúdo e footer. Equivalente a `<Dialog />` ou `<Modal />` do Material-UI.

## Interface

```typescript
interface ModalProps {
  open: boolean;
  title: string;
  description?: string;
  size?: 'sm' | 'md' | 'lg' | 'full';
  closeButton?: boolean;
  footer?: ReactNode;
  children: ReactNode;
  backdrop?: boolean;
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varModalHtml,
  If(varModalOpen,
    "<div class='modal-overlay " & If(varModalBackdrop, "modal-overlay--visible", "") & "' onclick='closeModal()'>" &
      "<div class='modal modal--" & Coalesce(varModalSize, "md") & "' role='dialog' aria-modal='true' aria-labelledby='modal-title'>" &
        "<div class='modal__header'>" &
          "<div class='modal__title-group'>" &
            "<h2 id='modal-title' class='modal__title'>" & varModalTitle & "</h2>" &
            If(!IsBlank(varModalDescription),
              "<p class='modal__description'>" & varModalDescription & "</p>", "") &
          "</div>" &
          If(varModalCloseButton,
            "<button class='btn-icon modal__close' aria-label='Fechar modal'>✕</button>", "") &
        "</div>" &
        "<div class='modal__body'>" &
          varModalContent &
        "</div>" &
        If(!IsBlank(varModalFooter),
          "<div class='modal__footer'>" & varModalFooter & "</div>", "") &
      "</div>" &
    "</div>",
    ""
  )
)
```

### CSS

```css
.modal-overlay {
  position: fixed;
  inset: 0;
  background: var(--surface-overlay);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--space-4);
  z-index: 100;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease, visibility 0.3s ease;
}

.modal-overlay--visible {
  opacity: 1;
  visibility: visible;
}

.modal {
  background: var(--surface);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-lg);
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  animation: modalSlideUp 0.3s ease-out;
}

@keyframes modalSlideUp {
  from {
    opacity: 0;
    transform: translateY(20px) scale(0.96);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

/* Tamanhos */
.modal--sm { max-width: 400px; }
.modal--md { max-width: 560px; }
.modal--lg { max-width: 720px; }
.modal--full {
  max-width: 100%;
  max-height: 100vh;
  border-radius: 0;
}

.modal__header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: var(--space-4);
  padding: var(--space-5) var(--space-5) var(--space-3);
  border-bottom: 1px solid var(--border);
}

.modal__title-group {
  flex: 1;
  min-width: 0;
}

.modal__title {
  font-size: 18px;
  font-weight: 700;
  color: var(--text);
  margin: 0;
  line-height: 1.3;
}

.modal__description {
  font-size: 14px;
  color: var(--text-muted);
  margin: var(--space-1) 0 0;
  line-height: 1.4;
}

.modal__close {
  width: 36px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--radius-sm);
  background: none;
  border: none;
  color: var(--text-muted);
  font-size: 18px;
  cursor: pointer;
  flex-shrink: 0;
  margin-top: -4px;
  margin-right: -4px;
}

.modal__close:hover {
  background: var(--surface-raised);
  color: var(--text);
}

.modal__body {
  padding: var(--space-4) var(--space-5);
  flex: 1;
  overflow-y: auto;
}

.modal__footer {
  display: flex;
  gap: var(--space-3);
  justify-content: flex-end;
  padding: var(--space-3) var(--space-5) var(--space-5);
  border-top: 1px solid var(--border);
}

/* Mobile: modal em tela cheia */
@media (max-width: 640px) {
  .modal {
    max-width: 100%;
    max-height: 100vh;
    border-radius: var(--radius-lg) var(--radius-lg) 0 0;
    position: fixed;
    bottom: 0;
    animation: modalSlideUpMobile 0.3s ease-out;
  }

  @keyframes modalSlideUpMobile {
    from { transform: translateY(100%); }
    to { transform: translateY(0); }
  }

  .modal--full {
    border-radius: 0;
  }
}
```

## Footer com Botões

```powerapps
Set(varModalFooter,
  "<button class='btn btn--ghost'>Cancelar</button>" &
  "<button class='btn btn--primary'>Confirmar</button>"
)
```

## Controle de Estado

```powerapps
// Abrir modal:
UpdateContext({varModalOpen: true, varModalTitle: "Confirmar exclusão"})

// Fechar modal:
UpdateContext({varModalOpen: false})

// OnVisible da tela (reset):
UpdateContext({varModalOpen: false})
```

## Acessibilidade

- `role="dialog"` e `aria-modal="true"`.
- `aria-labelledby` apontando para o título.
- Botão de fechar com `aria-label="Fechar modal"`.
- Foco deve ser gerenciado (Power Apps: use controles nativos sobrepostos).
- Fundo escuro com `onclick` para fechar (implementado via controle invisível no Power Apps).