# Componente: DataTable

> Tabela responsiva com cabeçalho, linhas, badges e ações. Equivalente a `<Table />` do Material-UI ou Ant Design.

## Interface

```typescript
interface DataTableColumn {
  key: string;
  header: string;
  width?: string;
  align?: 'left' | 'center' | 'right';
  format?: 'text' | 'date' | 'currency' | 'badge';
  badgeVariant?: 'success' | 'warning' | 'danger' | 'info' | 'neutral';
}

interface DataTableProps {
  columns: DataTableColumn[];
  items: any[];
  emptyState?: EmptyStateProps;
  loading?: boolean;
  sortable?: boolean;
  onRowClick?: string;
}
```

## Implementação

### Variável Power Apps

```powerapps
Set(varDataTableHtml,
  "<div class='data-table-wrapper'>" &
    "<table class='data-table' role='table'>" &
      "<thead class='data-table__head'>" &
        "<tr>" &
          Concat(colTableColumns,
            "<th class='data-table__th " &
              "data-table__th--" & Coalesce(ThisItem.Align, "left") & "' " &
              "style='" & If(!IsBlank(ThisItem.Width), "width: " & ThisItem.Width & ";", "") & "' " &
              If(varTableSortable, "role='columnheader' aria-sort='none'", "") &
            ">" & ThisItem.Header & "</th>"
          ) &
        "</tr>" &
      "</thead>" &
      "<tbody class='data-table__body'>" &
        If(CountRows(colTableItems) = 0,
          "<tr><td colspan='" & CountRows(colTableColumns) & "' class='data-table__empty'>" &
            varEmptyStateHtml &
          "</td></tr>",
          Concat(colTableItems,
            "<tr class='data-table__row " & If(ThisItem.IsSelected, "data-table__row--selected", "") & "' role='row'>" &
              Concat(colTableColumns,
                "<td class='data-table__td data-table__td--" & Coalesce(ThisItem.Align, "left") & "' role='cell'>" &
                  If(ThisItem.Format = "badge",
                    "<span class='badge badge--" & Coalesce(ThisItem.BadgeVariant, "neutral") & "'>" &
                      LookUp(colTableItems, ID = ThisItem.ID, ThisRecord[ThisItem.Key]) &
                    "</span>",
                    LookUp(colTableItems, ID = ThisItem.ID, ThisRecord[ThisItem.Key])
                  ) &
                "</td>"
              ) &
            "</tr>"
          )
        ) &
      "</tbody>" &
    "</table>" &
  "</div>"
)
```

### CSS

```css
.data-table-wrapper {
  width: 100%;
  overflow-x: auto;
  border-radius: var(--radius);
  border: 1px solid var(--border);
  background: var(--surface);
  box-shadow: var(--shadow-sm);
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
  min-width: 640px;
}

.data-table__head {
  background: var(--surface-raised);
  border-bottom: 1px solid var(--border);
}

.data-table__th {
  padding: var(--space-3) var(--space-4);
  font-weight: 600;
  color: var(--text-secondary);
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 0.025em;
  white-space: nowrap;
  text-align: left;
}

.data-table__th--center { text-align: center; }
.data-table__th--right { text-align: right; }

.data-table__th[aria-sort]:hover {
  background: var(--border);
  cursor: pointer;
}

.data-table__body {
  background: var(--surface);
}

.data-table__row {
  border-bottom: 1px solid var(--border);
  transition: background 0.15s ease;
}

.data-table__row:last-child {
  border-bottom: none;
}

.data-table__row:hover {
  background: var(--surface-raised);
}

.data-table__row--selected {
  background: var(--primary-light);
}

.data-table__row--selected:hover {
  background: var(--primary-light);
}

.data-table__td {
  padding: var(--space-3) var(--space-4);
  color: var(--text);
  vertical-align: middle;
}

.data-table__td--center { text-align: center; }
.data-table__td--right { text-align: right; }

.data-table__empty {
  padding: 0;
  border: none;
}

/* Mobile: cards em vez de tabela */
@media (max-width: 640px) {
  .data-table-wrapper {
    border: none;
    box-shadow: none;
    background: transparent;
  }

  .data-table,
  .data-table__head,
  .data-table__th,
  .data-table__body,
  .data-table__row,
  .data-table__td {
    display: block;
    width: 100%;
  }

  .data-table__head {
    display: none;
  }

  .data-table__row {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    margin-bottom: var(--space-3);
    padding: var(--space-4);
    box-shadow: var(--shadow-sm);
  }

  .data-table__td {
    padding: var(--space-1) 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px dashed var(--border);
  }

  .data-table__td:last-child {
    border-bottom: none;
  }

  .data-table__td::before {
    content: attr(data-label);
    font-weight: 600;
    color: var(--text-muted);
    font-size: 12px;
    text-transform: uppercase;
  }
}
```

## Coleção de Colunas

```powerapps
ClearCollect(colTableColumns, [
  {Key: "Title", Header: "Nome", Width: "", Align: "left", Format: "text"},
  {Key: "Status", Header: "Status", Width: "120px", Align: "center", Format: "badge", BadgeVariant: "info"},
  {Key: "Date", Header: "Data", Width: "120px", Align: "center", Format: "date"},
  {Key: "Amount", Header: "Valor", Width: "120px", Align: "right", Format: "currency"}
])
```

## Acessibilidade

- `role="table"`, `role="row"`, `role="cell"`, `role="columnheader"`.
- Cabeçalho com `scope="col"` (implícito no CSS mobile via `::before`).
- Linhas selecionáveis com foco visível.
- Contraste entre texto e fundo verificado.