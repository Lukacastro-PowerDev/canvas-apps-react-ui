# Componente: EmptyState

> Estado vazio com ícone, título, descrição e CTA. Equivalente a `<Empty />` do Ant Design.

## Interface

```typescript
interface EmptyStateProps {
  icon: string;
  title: string;
  description?: string;
  actionLabel?: string;
  actionIcon?: string;
  secondaryAction?: string;
  size?: 'sm' | 'lg';
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varEmptyStateHtml,
  "<div class='empty-state empty-state--" & Coalesce(varEmptySize, "lg") & "'>" &
    "<div class='empty-state__icon' aria-hidden='true'>" & varEmptyIcon & "</div>" &
    "<h3 class='empty-state__title'>" & varEmptyTitle & "</h3>" &
    If(!IsBlank(varEmptyDescription),
      "<p class='empty-state__description'>" & varEmptyDescription & "</p>", "") &
    "<div class='empty-state__actions'>" &
      If(!IsBlank(varEmptyActionLabel),
        "<button class='btn btn--primary'>" &
          If(!IsBlank(varEmptyActionIcon), "<span class='btn__icon'>" & varEmptyActionIcon & "</span>", "") &
          varEmptyActionLabel &
        "</button>", "") &
      If(!IsBlank(varEmptySecondaryAction),
        "<button class='btn btn--ghost'>" & varEmptySecondaryAction & "</button>", "") &
    "</div>" &
  "</div>"
)
```

### CSS

```css
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: var(--space-8);
  width: 100%;
}

.empty-state--lg {
  min-height: 400px;
}

.empty-state--sm {
  min-height: 200px;
  padding: var(--space-6);
}

.empty-state__icon {
  font-size: 64px;
  line-height: 1;
  margin-bottom: var(--space-4);
  opacity: 0.6;
}

.empty-state--sm .empty-state__icon {
  font-size: 40px;
  margin-bottom: var(--space-3);
}

.empty-state__title {
  font-size: 18px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 var(--space-2);
  line-height: 1.3;
}

.empty-state--sm .empty-state__title {
  font-size: 16px;
}

.empty-state__description {
  font-size: 14px;
  color: var(--text-muted);
  max-width: 320px;
  line-height: 1.5;
  margin: 0 0 var(--space-5);
}

.empty-state--sm .empty-state__description {
  font-size: 13px;
  margin-bottom: var(--space-4);
}

.empty-state__actions {
  display: flex;
  gap: var(--space-3);
  flex-wrap: wrap;
  justify-content: center;
}
```

## Variações por Contexto

| Contexto | Ícone | Título | Descrição | Ação |
|----------|-------|--------|-----------|------|
| Sem resultados | 🔍 | Nenhum resultado | Tente termos diferentes ou limpe os filtros. | Limpar filtros |
| Lista vazia | 📋 | Lista vazia | Nenhum item foi criado ainda. | Criar item |
| Sem notificações | 🔔 | Tudo em ordem | Você não tem notificações pendentes. | — |
| Erro de rede | ⚠️ | Não foi possível carregar | Verifique sua conexão e tente novamente. | Tentar novamente |
| Sem permissão | 🔒 | Acesso restrito | Você não tem permissão para ver este conteúdo. | Voltar |
| Busca vazia | 🔎 | Nenhum resultado | Sua busca não retornou resultados. | Nova busca |

## Acessibilidade

- Ícone com `aria-hidden="true"` (decorativo).
- Título como `<h3>` semântico.
- Descrição em `<p>`.
- Botões com labels claras.
- Contrastes verificados entre texto e fundo.