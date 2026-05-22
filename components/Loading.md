# Componente: Loading

> Estados de carregamento com spinner, skeleton e dots. Equivalente a `<Spin />`, `<Skeleton />` do Ant Design.

## Interface

```typescript
interface LoadingProps {
  variant: 'spinner' | 'skeleton' | 'dots';
  count?: number;       // Para skeleton
  text?: string;
  size?: 'sm' | 'md' | 'lg';
  fullScreen?: boolean;
}
```

## Implementação

### Variável Power Apps — Spinner

```powerapps
Set(varSpinnerHtml,
  "<div class='loading loading--" & Coalesce(varLoadingSize, "md") & " " &
    If(varLoadingFullScreen, "loading--fullscreen", "") & "'>" &
    "<div class='spinner spinner--" & varLoadingSize & "'></div>" &
    If(!IsBlank(varLoadingText),
      "<p class='loading__text'>" & varLoadingText & "</p>", "") &
  "</div>"
)
```

### Variável Power Apps — Skeleton

```powerapps
Set(varSkeletonHtml,
  "<div class='skeleton-list'>" &
    Concat(Sequence(Coalesce(varSkeletonCount, 3)),
      "<div class='skeleton-item'>" &
        "<div class='skeleton skeleton--avatar'></div>" &
        "<div class='skeleton-item__content'>" &
          "<div class='skeleton skeleton--title'></div>" &
          "<div class='skeleton skeleton--line'></div>" &
        "</div>" &
      "</div>"
    ) &
  "</div>"
)
```

### Variável Power Apps — Dots

```powerapps
Set(varDotsHtml,
  "<div class='loading loading--" & Coalesce(varLoadingSize, "md") & "'>" &
    "<div class='dots'>" &
      "<span class='dots__dot'></span>" &
      "<span class='dots__dot'></span>" &
      "<span class='dots__dot'></span>" &
    "</div>" &
    If(!IsBlank(varLoadingText),
      "<p class='loading__text'>" & varLoadingText & "</p>", "") &
  "</div>"
)
```

### CSS

```css
/* Loading Container */
.loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-4);
  padding: var(--space-6);
}

.loading--sm { padding: var(--space-4); }
.loading--lg { padding: var(--space-8); }

.loading--fullscreen {
  position: fixed;
  inset: 0;
  background: rgba(255,255,255,0.8);
  z-index: 200;
  backdrop-filter: blur(2px);
}

.loading__text {
  font-size: 14px;
  color: var(--text-muted);
  font-weight: 500;
  margin: 0;
}

/* Spinner */
.spinner {
  border-radius: 50%;
  border: 3px solid var(--border);
  border-top-color: var(--primary);
  animation: spin 0.75s linear infinite;
}

.spinner--sm { width: 20px; height: 20px; border-width: 2px; }
.spinner--md { width: 32px; height: 32px; border-width: 3px; }
.spinner--lg { width: 48px; height: 48px; border-width: 4px; }

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Dots */
.dots {
  display: flex;
  gap: 6px;
  align-items: center;
}

.dots__dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--primary);
  animation: dotPulse 1.4s ease-in-out infinite both;
}

.dots__dot:nth-child(1) { animation-delay: -0.32s; }
.dots__dot:nth-child(2) { animation-delay: -0.16s; }

@keyframes dotPulse {
  0%, 80%, 100% { transform: scale(0.4); opacity: 0.5; }
  40% { transform: scale(1); opacity: 1; }
}

/* Skeleton */
.skeleton-list {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
  width: 100%;
}

.skeleton-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3);
  background: var(--surface);
  border-radius: var(--radius);
  border: 1px solid var(--border);
}

.skeleton-item__content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.skeleton {
  background: linear-gradient(90deg, #f3f4f6 25%, #e5e7eb 50%, #f3f4f6 75%);
  background-size: 200% 100%;
  animation: skeletonShimmer 1.5s ease-in-out infinite;
  border-radius: var(--radius-sm);
}

.skeleton--avatar {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  flex-shrink: 0;
}

.skeleton--title {
  height: 16px;
  width: 60%;
  max-width: 200px;
}

.skeleton--line {
  height: 12px;
  width: 40%;
  max-width: 140px;
}

.skeleton--card {
  height: 120px;
  width: 100%;
  border-radius: var(--radius);
}

@keyframes skeletonShimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

## Uso Condicional

```powerapps
If(varIsLoading,
  Loading({variant: "skeleton", count: 3}),
  If(CountRows(colItems) = 0,
    EmptyState({icon: "📭", title: "Nenhum item"}),
    CardGrid({items: colItems})
  )
)
```

## Acessibilidade

- `aria-live="polite"` no container de loading quando usado em atualizações dinâmicas.
- Texto descritivo sempre visível (não dependa só da animação).
- Skeleton: elementos decorativos, não precisam de labels.
- Reduza movimento: respeite `prefers-reduced-motion`.