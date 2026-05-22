# Helpers e Funções Utilitárias

> Funções helper para uso em fórmulas Power Apps, inspiradas em utilitários JavaScript/TypeScript.

---

## String Helpers

### Truncate (truncar texto)

```powerapps
// React: text.length > max ? text.slice(0, max) + '...' : text
// Power Apps:
If(Len(varText) > varMaxLength,
  Left(varText, varMaxLength) & "…",
  varText
)
```

### Capitalize (primeira letra maiúscula)

```powerapps
// React: text.charAt(0).toUpperCase() + text.slice(1)
// Power Apps:
Upper(Left(varText, 1)) & Mid(varText, 2, Len(varText) - 1)
```

### Format Currency (moeda)

```powerapps
// React: new Intl.NumberFormat('pt-BR', {style: 'currency', currency: 'BRL'}).format(value)
// Power Apps:
Text(varValue, "[$-pt-BR]R$ #,##0.00")
```

### Format Date (data)

```powerapps
// React: new Date().toLocaleDateString('pt-BR')
// Power Apps:
Text(varDate, "dd/mm/yyyy")
Text(varDateTime, "dd/mm/yyyy hh:mm")
Text(varDateTime, "dddd, dd 'de' mmmm 'de' yyyy", "pt-BR")
```

### Slugify (criar ID amigável)

```powerapps
// React: text.toLowerCase().replace(/\s+/g, '-').replace(/[^a-z0-9-]/g, '')
// Power Apps (simplificado):
Substitute(
  Substitute(
    Lower(varText),
    " ", "-"
  ),
  "ã", "a", "á", "a", "â", "a", "à", "a",
  "é", "e", "ê", "e", "è", "e",
  "í", "i", "î", "i", "ì", "i",
  "õ", "o", "ó", "o", "ô", "o", "ò", "o",
  "ú", "u", "û", "u", "ù", "u",
  "ç", "c",
  "ñ", "n"
)
```

---

## Array/Collection Helpers

### Map (transformar coleção)

```powerapps
// React: items.map(item => ({...item, formattedDate: format(item.date)}))
// Power Apps: use ForAll + Collect ou Patch
ClearCollect(colFormatted,
  ForAll(colItems,
    {
      ID: ThisRecord.ID,
      Title: ThisRecord.Title,
      FormattedDate: Text(ThisRecord.Created, "dd/mm/yyyy"),
      IsUrgent: ThisRecord.Priority = "Alta"
    }
  )
)
```

### Filter (filtrar coleção)

```powerapps
// React: items.filter(item => item.active && item.priority === 'high')
// Power Apps:
Filter(colItems, Active = true && Priority = "Alta")
```

### Find/Lookup (buscar item)

```powerapps
// React: items.find(item => item.id === searchId)
// Power Apps:
LookUp(colItems, ID = varSearchId)
```

### Sort (ordenar coleção)

```powerapps
// React: items.sort((a, b) => a.date - b.date)
// Power Apps:
Sort(colItems, Created, SortOrder.Descending)
SortByColumns(colItems, "Title", SortOrder.Ascending)
```

### Group By (agrupar)

```powerapps
// React: Object.groupBy(items, item => item.status)
// Power Apps:
GroupBy(colItems, "Status", "GroupedItems")
```

### Unique (valores únicos)

```powerapps
// React: [...new Set(items.map(i => i.category))]
// Power Apps:
Distinct(colItems, Category)
```

### Count / Sum / Average

```powerapps
// React: items.reduce((sum, item) => sum + item.value, 0)
// Power Apps:
CountRows(colItems)
Sum(colItems, Value)
Average(colItems, Value)
Max(colItems, Value)
Min(colItems, Value)
```

### First / Last

```powerapps
// React: items[0], items[items.length - 1]
// Power Apps:
First(colItems)
Last(colItems)
FirstN(colItems, 5)  // Primeiros 5
```

---

## Conditional Helpers

### Coalesce (primeiro valor não vazio)

```powerapps
// React: value1 ?? value2 ?? value3 ?? 'default'
// Power Apps:
Coalesce(varValue1, varValue2, varValue3, "default")
```

### Ternary (condicional inline)

```powerapps
// React: condition ? valueIfTrue : valueIfFalse
// Power Apps:
If(varIsActive, "Ativo", "Inativo")

// Encadeado (equivalente a switch):
If(varStatus = "approved", "Aprovado",
  If(varStatus = "rejected", "Rejeitado",
    If(varStatus = "pending", "Pendente", "Desconhecido")
  )
)

// Ou use Switch:
Switch(varStatus,
  "approved", "Aprovado",
  "rejected", "Rejeitado",
  "pending", "Pendente",
  "Desconhecido"
)
```

### Boolean (converter para booleano)

```powerapps
// React: !!value
// Power Apps:
!IsBlank(varValue) && varValue <> false && varValue <> 0
```

---

## Date/Time Helpers

### Add Days

```powerapps
// React: new Date(date.getTime() + days * 24 * 60 * 60 * 1000)
// Power Apps:
DateAdd(varDate, 7, Days)
```

### Date Difference

```powerapps
// React: Math.floor((date2 - date1) / (1000 * 60 * 60 * 24))
// Power Apps:
DateDiff(varDate1, varDate2, Days)
```

### Now / Today

```powerapps
// React: new Date(), new Date().setHours(0,0,0,0)
// Power Apps:
Now()      // Data e hora atual
Today()    // Data atual (sem hora)
```

### Is Today / Is Past / Is Future

```powerapps
// React: date.toDateString() === new Date().toDateString()
// Power Apps:
DateValue(varDate) = Today()           // É hoje
DateValue(varDate) < Today()           // É passado
DateValue(varDate) > Today()           // É futuro
```

---

## Color Helpers

### Hex to RGBA (para overlay)

```powerapps
// React: hex + opacity como rgba
// Power Apps (simplificado — use CSS no HTML Text):
// No CSS: background: rgba(37, 99, 235, 0.1);
// Ou defina cores com alpha no design system
```

### Contrast Color (texto sobre fundo)

```powerapps
// React: luminance(bg) > 0.5 ? '#000' : '#fff'
// Power Apps (simplificado):
If(varIsLightBackground, "#111827", "#ffffff")
```

---

## Validation Helpers

### Is Email

```powerapps
// React: /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
// Power Apps:
IsMatch(varEmail, Email)
// Ou regex personalizado:
IsMatch(varEmail, "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$")
```

### Is Phone

```powerapps
// React: regex para telefone
// Power Apps:
IsMatch(varPhone, "^\(?[0-9]{2}\)?[\s-]?[0-9]{4,5}[\s-]?[0-9]{4}$")
```

### Is Empty / Is Blank

```powerapps
// React: value === null || value === undefined || value === ''
// Power Apps:
IsBlank(varValue)        // String vazia ou null
IsEmpty(colCollection)   // Coleção vazia
CountRows(colCollection) = 0
```

### Required Field

```powerapps
// React: if (!value) return 'Campo obrigatório'
// Power Apps:
If(IsBlank(varFieldValue), "Campo obrigatório", "")
```

### Min / Max Length

```powerapps
// React: value.length < min ? `Mínimo ${min} caracteres` : ''
// Power Apps:
If(Len(varFieldValue) < 3, "Mínimo 3 caracteres", "")
If(Len(varFieldValue) > 100, "Máximo 100 caracteres", "")
```

---

## Navigation Helpers

### Navigate with Params

```powerapps
// React: navigate('/detail', { state: { id: item.id } })
// Power Apps:
Navigate(scrDetail, ScreenTransition.Fade, {varDetailId: ThisItem.ID})
```

### Back

```powerapps
// React: navigate(-1)
// Power Apps:
Back()
```

### Reset Screen

```powerapps
// React: reset state
// Power Apps:
Reset(txtSearch);
ClearCollect(colItems, Filter(datasource, true));
UpdateContext({varSearchTerm: ""})
```

---

## Performance Helpers

### Debounce (simulado com Timer)

```powerapps
// React: useDebounce(value, 500)
// Power Apps:
// 1. Timer control com AutoStart = false, Duration = 500
// 2. OnChange do input: Reset(tmrDebounce); StartTimer(tmrDebounce)
// 3. OnTimerEnd do Timer: executar ação (busca, filtro, etc.)
```

### Throttle (simulado com Timer)

```powerapps
// React: useThrottle(callback, 1000)
// Power Apps:
// Timer com AutoStart = true, Repeat = true, Duration = 1000
// OnTimerEnd: executar ação se varHasPendingAction = true
```

---

## Random Helpers

### Random Number

```powerapps
// React: Math.floor(Math.random() * max)
// Power Apps:
Rand() * 100           // 0 a 100
Round(Rand() * 100, 0)  // Inteiro 0 a 100
```

### Random Item from Collection

```powerapps
// React: items[Math.floor(Math.random() * items.length)]
// Power Apps:
First(Shuffle(colItems))
```

---

## User Info Helpers

### Current User

```powerapps
// React: useAuth().user
// Power Apps:
User().FullName
User().Email
User().Image
```

### User Roles / Permissions

```powerapps
// React: user.roles.includes('admin')
// Power Apps:
"admin" in varUserRoles
// Ou:
If(LookUp(colUserRoles, Role = "admin", true), true, false)
```

---

## Error Handling Helpers

### Try / Catch (simulado)

```powerapps
// React: try { ... } catch (e) { ... }
// Power Apps: use IfError (se disponível) ou verificação manual
If(IsBlank(LookUp(datasource, ID = varId)),
  UpdateContext({varError: "Registro não encontrado"}),
  UpdateContext({varError: ""})
)
```

### Loading State

```powerapps
// React: const [isLoading, setIsLoading] = useState(false)
// Power Apps:
UpdateContext({varIsLoading: true});
// ... após operação ...
UpdateContext({varIsLoading: false})
```

---

## Resumo de Equivalências

| JavaScript/TypeScript | Power Apps |
|----------------------|-----------|
| `array.map()` | `ForAll()` / `Concat()` |
| `array.filter()` | `Filter()` |
| `array.find()` | `LookUp()` |
| `array.reduce()` | Loop manual / `Sum()` / `Concatenate()` |
| `array.sort()` | `Sort()` / `SortByColumns()` |
| `array.length` | `CountRows()` |
| `array.includes()` | `in` operator |
| `string.length` | `Len()` |
| `string.slice()` | `Left()` / `Mid()` / `Right()` |
| `string.replace()` | `Substitute()` |
| `string.split()` | `Split()` |
| `string.trim()` | `Trim()` |
| `Math.max()` | `Max()` |
| `Math.min()` | `Min()` |
| `Math.round()` | `Round()` |
| `Math.random()` | `Rand()` |
| `Date.now()` | `Now()` |
| `new Date()` | `Today()` |
| `setTimeout()` | Timer control |
| `JSON.stringify()` | `JSON()` |
| `JSON.parse()` | `ParseJSON()` |
| `typeof` | `Type()` |
| `===` | `=` |
| `!==` | `<>` |
| `&&` | `And` / `&&` |
| `\|\|` | `Or` / `\|\|` |
| `!` | `Not` / `!` |
| `??` | `Coalesce()` |
| `?.` | `Coalesce(Lookup(...).Property, "default")` |