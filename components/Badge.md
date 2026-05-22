# Componente: Badge

> Indicador de status com variantes semânticas. Equivalente a `<Badge />` ou `<Tag />`.

## Interface

```typescript
interface BadgeProps {
  children: string;
  variant?: 'success' | 'warning' | 'danger' | 'info' | 'neutral';
  size?: 'sm' | 'md';
  dot?: boolean;
  pulse?: boolean;
}
```

## Implementação

### Variável Power Apps

```powerapps
// Uso simples:
Badge({children: "Concluído", variant: "success"})

// Na prática:
"<span class='badge badge--" & varBadgeVariant & " badge--" & varBadgeSize & "'>" &
  If(varBadgeDot,
    "<span class='badge__dot " & If(varBadgePulse, "badge__dot--pulse", "") & "'></span>", "") &
  varBadgeText &
"</span>"
```

### CSS

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-family: var(--font-sans);
  font-weight: 600;
  line-height: 1;
  white-space: nowrap;
  border-radius: var(--radius-full);
  vertical-align: middle;
}

/* Tamanhos */
.badge--sm {
  padding: 2px 8px;
  font-size: 11px;
}

.badge--md {
  padding: var(--space-1) var(--space-2);
  font-size: 12px;
}

/* Variantes */
.badge--success {
  background: var(--success-light);
  color: var(--success);
}

.badge--warning {
  background: var(--warning-light);
  color: var(--warning);
}

.badge--danger {
  background: var(--danger-light);
  color: var(--danger);
}

.badge--info {
  background: var(--info-light);
  color: var(--info);
}

.badge--neutral {
  background: var(--surface-raised);
  color: var(--text-muted);
}

/* Dot indicator */
.badge__dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: currentColor;
  flex-shrink: 0;
}

.badge__dot--pulse {
  animation: badgePulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes badgePulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
```

## Uso com Map/Concat

```powerapps
Concat(colStatusList,
  "<span class='badge badge--" &
    If(ThisItem.Status = "Aprovado", "success",
      If(ThisItem.Status = "Pendente", "warning",
        If(ThisItem.Status = "Rejeitado", "danger", "neutral"))) &
  "'>" & ThisItem.Status & "</span>"
)
```

## Acessibilidade

- Texto sempre visível dentro do badge.
- Nunca use badge sem texto (cor isolada não é acessível).
- Dot com `aria-hidden` quando decorativo.
- Contraste entre texto e fundo do badge ≥ 4.5:1.