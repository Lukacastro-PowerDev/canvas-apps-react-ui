# Screen: Dashboard

> Tela de dashboard com KPIs, gráfico e lista recente. Implementação completa em HTML Text com estilo React-like.

## Interface

```typescript
interface DashboardScreenProps {
  kpis: Array<{
    title: string;
    value: string;
    delta: number;
    deltaDirection: 'up' | 'down' | 'neutral';
    icon: string;
  }>;
  recentItems: Array<{
    id: string;
    title: string;
    subtitle: string;
    status: string;
    date: string;
  }>;
  isLoading: boolean;
  user: { name: string; avatar: string };
}
```

## Implementação Completa

### OnVisible da Tela

```powerapps
UpdateContext({
  varIsLoading: true,
  varHeaderTitle: "Dashboard",
  varHeaderSubtitle: "Visão geral do sistema",
  varShowBack: false,
  varPrimaryAction: "Novo",
  varPrimaryActionIcon: "➕",
  varUserAvatar: User().Image,
  varHeaderElevated: false
});

// Carregar KPIs
ClearCollect(colKPIs, [
  {Title: "Ordens", Value: "1.247", Delta: 12.5, Direction: "up", Icon: "📦"},
  {Title: "Receita", Value: "R$ 45.2K", Delta: -3.2, Direction: "down", Icon: "💰"},
  {Title: "Clientes", Value: "892", Delta: 0, Direction: "neutral", Icon: "👥"},
  {Title: "Pendentes", Value: "23", Delta: 5.1, Direction: "up", Icon: "⏳"}
]);

// Carregar itens recentes
ClearCollect(colRecentItems, 
  Filter(datasource, Created >= DateAdd(Today(), -7, Days))
);

UpdateContext({varIsLoading: false})
```

### HTML Text — Estrutura Completa

```powerapps
"<style>" & varGlobalStyles & "</style>" &
"<div class='app'>" &
  // Header
  "<header class='header' style='background: var(--primary); color: white;'>" &
    "<div class='header__left'>" &
      "<div class='header__title-group'>" &
        "<h1 class='header__title'>" & varHeaderTitle & "</h1>" &
        "<span class='header__subtitle'>" & varHeaderSubtitle & "</span>" &
      "</div>" &
    "</div>" &
    "<div class='header__right'>" &
      "<button class='btn btn--sm' style='background: rgba(255,255,255,0.15); color: white; border: 1px solid rgba(255,255,255,0.3);'>" &
        varPrimaryActionIcon & " " & varPrimaryAction &
      "</button>" &
      "<img class='header__avatar' src='" & varUserAvatar & "' alt='' />" &
    "</div>" &
  "</header>" &

  // Main Content
  "<main class='app__main'>" &

    // KPI Grid
    If(varIsLoading,
      // Loading Skeleton
      "<div class='grid grid-cols-2 sm:grid-cols-2 md:grid-cols-4 gap-4 mb-6'>" &
        Concat(Sequence(4),
          "<div class='card card--metric'>" &
            "<div class='skeleton' style='width: 40px; height: 40px; border-radius: 50%; margin-bottom: 12px;'></div>" &
            "<div class='skeleton' style='width: 80px; height: 32px; margin-bottom: 8px;'></div>" &
            "<div class='skeleton' style='width: 60px; height: 14px;'></div>" &
          "</div>"
        ) &
      "</div>",

      // KPI Cards
      "<div class='grid grid-cols-2 sm:grid-cols-2 md:grid-cols-4 gap-4 mb-6'>" &
        Concat(colKPIs,
          "<div class='card card--metric animate-fade-in-up' style='animation-delay: " & ThisRecord.Index * 0.1 & "s'>" &
            "<div class='card__metric-icon'>" & ThisRecord.Icon & "</div>" &
            "<div class='card__metric-value'>" & ThisRecord.Value & "</div>" &
            "<div class='card__metric-label'>" & ThisRecord.Title & "</div>" &
            If(ThisRecord.Delta <> 0,
              "<div class='card__metric-delta card__metric-delta--" & ThisRecord.Direction & "'>" &
                If(ThisRecord.Direction = "up", "▲", If(ThisRecord.Direction = "down", "▼", "−")) &
                " " & Text(Abs(ThisRecord.Delta), "0.0") & "%" &
              "</div>",
              ""
            ) &
          "</div>"
        ) &
      "</div>"
    ) &

    // Seção: Atividades Recentes
    "<div class='mb-4'>" &
      "<div class='flex items-center justify-between mb-4'>" &
        "<h2 class='text-lg font-bold'>Atividades Recentes</h2>" &
        "<button class='btn btn--ghost btn--sm text-primary'>Ver todas →</button>" &
      "</div>" &
    "</div>" &

    If(varIsLoading,
      // Loading Skeleton List
      "<div class='flex flex-col gap-3'>" &
        Concat(Sequence(5),
          "<div class='card card--list'>" &
            "<div class='skeleton skeleton--avatar'></div>" &
            "<div class='flex-1'>" &
              "<div class='skeleton' style='width: 60%; height: 16px; margin-bottom: 8px;'></div>" &
              "<div class='skeleton' style='width: 40%; height: 12px;'></div>" &
            "</div>" &
          "</div>"
        ) &
      "</div>",

      If(CountRows(colRecentItems) = 0,
        // Empty State
        "<div class='empty-state empty-state--sm'>" &
          "<div class='empty-state__icon'>📭</div>" &
          "<h3 class='empty-state__title'>Nenhuma atividade recente</h3>" &
          "<p class='empty-state__description'>As atividades dos últimos 7 dias aparecerão aqui.</p>" &
        "</div>",

        // List Cards
        "<div class='flex flex-col gap-3'>" &
          Concat(colRecentItems,
            "<div class='card card--list'>" &
              "<div class='card__avatar'>" &
                "<span class='card__avatar-fallback'>" & Left(ThisRecord.Title, 1) & "</span>" &
              "</div>" &
              "<div class='card__content'>" &
                "<div class='card__header'>" &
                  "<h3 class='card__title truncate'>" & ThisRecord.Title & "</h3>" &
                  "<span class='badge badge--" &
                    If(ThisRecord.Status = "Concluído", "success",
                      If(ThisRecord.Status = "Pendente", "warning",
                        If(ThisRecord.Status = "Cancelado", "danger", "neutral"))) &
                  "'>" & ThisRecord.Status & "</span>" &
                "</div>" &
                "<p class='card__subtitle truncate'>" & ThisRecord.Subtitle & " • " & Text(ThisRecord.Date, "dd/mm/yyyy") & "</p>" &
              "</div>" &
              "<div class='card__action'>→</div>" &
            "</div>"
          ) &
        "</div>"
      )
    ) &
  "</main>" &
"</div>"
```

## CSS Adicional do Dashboard

```css
/* KPI Card específico */
.card--metric {
  text-align: center;
  padding: var(--space-6);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-2);
}
```

## Acessibilidade

- Título da tela como `<h1>`.
- Seções com `<h2>`.
- Cards de lista como `<article>`.