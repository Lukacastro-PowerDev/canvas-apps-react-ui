# Componente: Card

> Container versátil inspirado em `<Card />` do Material-UI. Suporta variantes: métrica, lista, resumo e mídia.

## Interface

```typescript
interface CardProps {
  variant: 'metric' | 'list' | 'summary' | 'media';
  title: string;
  subtitle?: string;
  description?: string;
  value?: string;
  delta?: number;
  deltaDirection?: 'up' | 'down' | 'neutral';
  badge?: string;
  badgeVariant?: 'success' | 'warning' | 'danger' | 'info' | 'neutral';
  icon?: string;
  image?: string;
  actionIcon?: string;
  actionLabel?: string;
  disabled?: boolean;
  loading?: boolean;
}
```

## Implementação

### Variável Power Apps (Card de Lista — mais comum)

```powerapps
// Use dentro de um Concat() para renderizar uma lista:
Concat(colItems,
  "<article class='card card--list " & If(ThisItem.IsSelected, "card--selected", "") & "'>" &
    "<div class='card__avatar'>" &
      If(!IsBlank(ThisItem.Image),
        "<img src='" & ThisItem.Image & "' alt='' class='card__avatar-img' />",
        "<span class='card__avatar-fallback'>" & Left(ThisItem.Title, 1) & "</span>") &
    "</div>" &
    "<div class='card__content'>" &
      "<div class='card__header'>" &
        "<h3 class='card__title truncate'>" & ThisItem.Title & "</h3>" &
        If(!IsBlank(ThisItem.Badge),
          "<span class='badge badge--" & ThisItem.BadgeVariant & "'>" & ThisItem.Badge & "</span>", "") &
      "</div>" &
      If(!IsBlank(ThisItem.Subtitle),
        "<p class='card__subtitle truncate'>" & ThisItem.Subtitle & "</p>", "") &
    "</div>" &
    If(!IsBlank(ThisItem.ActionIcon),
      "<div class='card__action' aria-label='Abrir detalhes'>" & ThisItem.ActionIcon & "</div>", "") &
  "</article>"
)
```

### Variável Power Apps (Card de Métrica)

```powerapps
"<article class='card card--metric'>" &
  If(!IsBlank(varMetricIcon),
    "<div class='card__metric-icon'>" & varMetricIcon & "</div>", "") &
  "<div class='card__metric-value'>" & varMetricValue & "</div>" &
  "<div class='card__metric-label'>" & varMetricTitle & "</div>" &
  If(!IsBlank(varMetricDelta),
    "<div class='card__metric-delta card__metric-delta--" & varMetricDeltaDirection & "'>" &
      If(varMetricDeltaDirection = "up", "▲", If(varMetricDeltaDirection = "down", "▼", "−")) &
      " " & Text(varMetricDelta, "0.0%") &
    "</div>", "") &
"</article>"
```

### CSS

```css
/* Card Base */
.card {
  background: var(--surface);
  border-radius: var(--radius);
  border: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  transition: box-shadow 0.2s ease, transform 0.2s ease, border-color 0.2s ease;
  overflow: hidden;
}

.card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--border-strong);
}

.card--selected {
  border-color: var(--primary);
  box-shadow: 0 0 0 3px var(--primary-light);
}

/* Card de Lista */
.card--list {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  cursor: pointer;
  min-height: 64px;
}

.card__avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--primary-light);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  overflow: hidden;
}

.card__avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card__avatar-fallback {
  font-size: 16px;
  font-weight: 700;
  color: var(--primary);
  text-transform: uppercase;
}

.card__content {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.card__header {
  display: flex;
  align-items: center;
  gap: var(--space-2);
}

.card__title {
  font-size: 15px;
  font-weight: 600;
  color: var(--text);
  margin: 0;
  line-height: 1.3;
}

.card__subtitle {
  font-size: 13px;
  color: var(--text-muted);
  margin: 0;
  line-height: 1.3;
}

.card__action {
  color: var(--text-muted);
  font-size: 18px;
  flex-shrink: 0;
  padding: var(--space-2);
}

/* Card de Métrica */
.card--metric {
  padding: var(--space-6);
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-2);
}

.card__metric-icon {
  font-size: 28px;
  margin-bottom: var(--space-1);
}

.card__metric-value {
  font-size: 32px;
  font-weight: 800;
  color: var(--text);
  line-height: 1.1;
}

.card__metric-label {
  font-size: 14px;
  color: var(--text-muted);
  font-weight: 500;
}

.card__metric-delta {
  font-size: 13px;
  font-weight: 600;
  display: inline-flex;
  align-items: center;
  gap: 2px;
  padding: 2px 8px;
  border-radius: var(--radius-full);
}

.card__metric-delta--up {
  color: var(--success);
  background: var(--success-light);
}

.card__metric-delta--down {
  color: var(--danger);
  background: var(--danger-light);
}

.card__metric-delta--neutral {
  color: var(--text-muted);
  background: var(--surface-raised);
}

/* Card de Resumo */
.card--summary {
  padding: var(--space-4);
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}

.card--summary .card__title {
  font-size: 16px;
}

.card--summary .card__description {
  font-size: 14px;
  color: var(--text-secondary);
  line-height: 1.5;
}

/* Card de Mídia */
.card--media {
  padding: 0;
}

.card__media-img {
  width: 100%;
  height: 160px;
  object-fit: cover;
  border-radius: var(--radius) var(--radius) 0 0;
}

.card__media-body {
  padding: var(--space-4);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}
```

## Grid de Cards

```css
.card-grid {
  display: grid;
