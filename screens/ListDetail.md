# Screen: List + Detail

> Tela com lista de itens e painel de detalhes (desktop) ou modal (mobile). Padrão master-detail.

## Interface

```typescript
interface ListDetailScreenProps {
  items: Array<{
    id: string;
    title: string;
    subtitle: string;
    status: string;
    priority: string;
    date: string;
    image?: string;
  }>;
  selectedItem?: {
    id: string;
    title: string;
    description: string;
    status: string;
    priority: string;
    createdBy: string;
    createdDate: string;
    history: Array<{date: string; action: string; user: string}>;
  };
  filters: Array<{key: string; label: string; active: boolean}>;
  searchTerm: string;
  isLoading: boolean;
  isDetailOpen: boolean;
}
```

## Implementação

### OnVisible

```powerapps
UpdateContext({
  varIsLoading: true,
  varHeaderTitle: "Ordens de Serviço",
  varHeaderSubtitle: CountRows(datasource) & " registros",
  varShowBack: false,
  varPrimaryAction: "Nova Ordem",
  varPrimaryActionIcon: "➕",
  varSearchTerm: "",
  varSelectedFilter: "Todas",
  varIsDetailOpen: false,
  varSelectedId: Blank()
});

ClearCollect(colFilters, [
  {Key: "Todas", Label: "Todas", Count: CountRows(datasource)},
  {Key: "Aberta", Label: "Abertas", Count: CountRows(Filter(datasource, Status = "Aberta"))},
  {Key: "Andamento", Label: "Em Andamento", Count: CountRows(Filter(datasource, Status = "Em Andamento"))},
  {Key: "Urgente", Label: "Urgentes", Count: CountRows(Filter(datasource, Priority = "Alta"))},
  {Key: "Concluída", Label: "Concluídas", Count: CountRows(Filter(datasource, Status = "Concluída"))}
]);

ClearCollect(colItems, datasource);
UpdateContext({varIsLoading: false})
```

### Filtros e Busca

```powerapps
// OnChange do Text Input de busca (nativo, sobreposto):
UpdateContext({varSearchTerm: Self.Text});
ClearCollect(colFilteredItems,
  Filter(colItems,
    (IsBlank(varSearchTerm) || StartsWith(Title, varSearchTerm) || StartsWith(Description, varSearchTerm)) &&
    (varSelectedFilter = "Todas" ||
      (varSelectedFilter = "Aberta" && Status = "Aberta") ||
      (varSelectedFilter = "Andamento" && Status = "Em Andamento") ||
      (varSelectedFilter = "Urgente" && Priority = "Alta") ||
      (varSelectedFilter = "Concluída" && Status = "Concluída")
    )
  )
)
```

### HTML Text — Layout

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
      "<button class='btn btn--sm' style='background: rgba(255,255,255,0.15); color: white;'>" &
        varPrimaryActionIcon & " " & varPrimaryAction &
      "</button>" &
    "</div>" &
  "</header>" &

  // Main: Split Layout
  "<main class='app__main' style='padding: 0; display: flex; flex-direction: row;'>" &

    // Painel Esquerdo: Lista
    "<div class='list-panel' style='flex: 1; min-width: 0; display: flex; flex-direction: column; border-right: 1px solid var(--border);'>" &

      // Barra de Filtros
      "<div class='filter-bar' style='padding: var(--space-3) var(--space-4); border-bottom: 1px solid var(--border); overflow-x: auto; white-space: nowrap;'>" &
        Concat(colFilters,
          "<button class='filter-chip " & If(ThisRecord.Key = varSelectedFilter, "filter-chip--active", "") & "'>" &
            ThisRecord.Label &
            "<span class='filter-chip__count'>" & ThisRecord.Count & "</span>" &
          "</button>"
        ) &
      "</div>" &

      // Barra de Busca (visual — input nativo fica sobreposto)
      "<div style='padding: var(--space-3) var(--space-4); border-bottom: 1px solid var(--border);'>" &
        "<div class='form-group__input-wrapper'>" &
          "<span class='form-group__icon form-group__icon--left'>🔍</span>" &
          "<input type='search' class='input' placeholder='Buscar ordens...' value='" & varSearchTerm & "' style='padding-left: 40px;' />" &
        "</div>" &
      "</div>" &

      // Lista de Itens
      If(varIsLoading,
        "<div class='flex flex-col gap-2' style='padding: var(--space-3);'>" &
          Concat(Sequence(6),
            "<div class='card card--list'>" &
              "<div class='skeleton skeleton--avatar'></div>" &
              "<div class='flex-1'>" &
                "<div class='skeleton' style='width: 60%; height: 16px; margin-bottom: 8px;'></div>" &
                "<div class='skeleton' style='width: 40%; height: 12px;'></div>" &
              "</div>" &
            "</div>"
          ) &
        "</div>",

        If(CountRows(colFilteredItems) = 0,
          "<div style='padding: var(--space-8);'>" &
            "<div class='empty-state empty-state--sm'>" &
              "<div class='empty-state__icon'>🔍</div>" &
              "<h3 class='empty-state__title'>Nenhum resultado</h3>" &
              "<p class='empty-state__description'>Tente ajustar os filtros ou limpar a busca.</p>" &
            "</div>" &
          "</div>",

          "<div class='flex flex-col' style='overflow-y: auto;'>" &
            Concat(colFilteredItems,
              "<div class='list-item " & If(ThisRecord.ID = varSelectedId, "list-item--selected", "") & "'>" &
                "<div class='list-item__indicator " &
                  If(ThisRecord.Priority = "Alta", "list-item__indicator--urgent",
                    If(ThisRecord.Priority = "Média", "list-item__indicator--warning", "list-item__indicator--normal")) &
                "'></div>" &
                "<div class='list-item__content'>" &
                  "<div class='list-item__header'>" &
                    "<span class='list-item__title truncate'>" & ThisRecord.Title & "</span>" &
                    "<span class='badge badge--" &
                      If(ThisRecord.Status = "Concluída", "success",
                        If(ThisRecord.Status = "Em Andamento", "warning",
                          If(ThisRecord.Status = "Aberta", "info", "neutral"))) &
                    " badge--sm'>" & ThisRecord.Status & "</span>" &
                  "</div>" &
                  "<div class='list-item__meta'>" &
                    "<span>" & ThisRecord.Subtitle & "</span>" &
                    "<span>•</span>" &
                    "<span>" & Text(ThisRecord.Date, "dd/mm/yyyy") & "</span>" &
                  "</div>" &
                "</div>" &
              "</div>"
            ) &
          "</div>"
        )
      ) &
    "</div>" &

    // Painel Direito: Detalhes (Desktop) ou oculto (Mobile)
    If(!varIsDetailOpen && Screen.Size = ScreenSize.Small,
      "",
      "<div class='detail-panel' style='flex: 1; min-width: 0; display: flex; flex-direction: column; background: var(--surface);'>" &
        If(IsBlank(varSelectedId),
          "<div class='empty-state'>" &
            "<div class='empty-state__icon'>📋</div>" &
            "<h3 class='empty-state__title'>Selecione um item</h3>" &
            "<p class='empty-state__description'>Clique em um item da lista para ver os detalhes.</p>" &
          "</div>",

          // Detalhes do item selecionado
          "<div style='display: flex; flex-direction: column; height: 100%;'>" &
            // Header do Detail
            "<div style='padding: var(--space-5) var(--space-6); border-bottom: 1px solid var(--border);'>" &
              "<div class='flex items-center gap-3 mb-3'>" &
                "<span class='badge badge--" &
                  If(varSelectedStatus = "Concluída", "success",
                    If(varSelectedStatus = "Em Andamento", "warning",
                      If(varSelectedStatus = "Aberta", "info", "neutral"))) &
                "'>" & varSelectedStatus & "</span>" &
                "<span class='text-sm text-muted'>" & Text(varSelectedDate, "dd/mm/yyyy hh:mm") & "</span>" &
              "</div>" &
              "<h2 style='font-size: 22px; font-weight: 700; margin: 0 0 var(--space-2);'>" & varSelectedTitle & "</h2>" &
              "<p style='color: var(--text-secondary); line-height: 1.6; margin: 0;'>" & varSelectedDescription & "</p>" &
            "</div>" &

            // Info Grid
            "<div style='padding: var(--space-4) var(--space-6); display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--space-4); border-bottom: 1px solid var(--border);'>" &
              "<div>" &
                "<div class='text-sm text-muted mb-1'>Criado por</div>" &
                "<div class='font-semibold'>" & varSelectedCreatedBy & "</div>" &
              "</div>" &
              "<div>" &
                "<div class='text-sm text-muted mb-1'>Prioridade</div>" &
                "<div class='font-semibold'>" & varSelectedPriority & "</div>" &
              "</div>" &
            "</div>" &

            // Histórico (Timeline)
            "<div style='padding: var(--space-4) var(--space-6); flex: 1; overflow-y: auto;'>" &
              "<h3 style='font-size: 14px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.05em; color: var(--text-muted); margin: 0 0 var(--space-4);'>Histórico</h3>" &
              "<div class='timeline'>" &
                Concat(colSelectedHistory,
                  "<div class='timeline__item'>" &
                    "<div class='timeline__dot'></div>" &
                    "<div class='timeline__content'>" &
                      "<div class='timeline__action'>" & ThisRecord.Action & "</div>" &
                      "<div class='timeline__meta'>" & ThisRecord.User & " • " & Text(ThisRecord.Date, "dd/mm/yyyy hh:mm") & "</div>" &
                    "</div>" &
                  "</div>"
                ) &
              "</div>" &
            "</div>" &

            // Ações
            "<div style='padding: var(--space-4) var(--space-6); border-top: 1px solid var(--border); display: flex; gap: var(--space-3);'>" &
              "<button class='btn btn--secondary' style='flex: 1;'>Editar</button>" &
              If(varSelectedStatus <> "Concluída",
                "<button class='btn btn--primary' style='flex: 1;'>Concluir</button>",
                ""
              ) &
            "</div>" &
          "</div>"
        ) &
      "</div>"
    ) &
  "</main>" &
"</div>"
```

### CSS Adicional

```css
/* List Panel */
.list-panel {
  background: var(--surface);
}

/* Filter Chips */
.filter-bar {
  display: flex;
  gap: var(--space-2);
  padding: var(--space-3) var(--space-4);
  border-bottom: 1px solid var(--border);
  overflow-x: auto;
  scrollbar-width: none;
}
```

## Acessibilidade
- Lista com `role="list"`, itens com `role="listitem"`.
- Item selecionado com `aria-selected="true"`.
- Painel de detalhes com `role="complementary"`.
- Timeline com estrutura semântica.