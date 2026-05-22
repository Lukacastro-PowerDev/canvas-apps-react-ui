# Mapeamento: React Hooks → Power Apps

> Guia de como traduzir hooks e padrões React para fórmulas e controles do Power Apps.

---

## useState → UpdateContext / Set

### React
```tsx
const [count, setCount] = useState(0);
const [isOpen, setIsOpen] = useState(false);
```

### Power Apps
```powerapps
// Declaração (OnVisible ou OnStart):
UpdateContext({varCount: 0, varIsOpen: false})

// Atualização:
UpdateContext({varCount: varCount + 1})
UpdateContext({varIsOpen: true})

// Para variáveis globais (persistem entre telas):
Set(gblUserName, User().FullName)
```

**Dica**: Use `UpdateContext` para estado local da tela, `Set` para estado global do app.

---

## useEffect (mount) → OnVisible

### React
```tsx
useEffect(() => {
  fetchData();
}, []);
```

### Power Apps
```powerapps
// Propriedade OnVisible da tela:
ClearCollect(colItems, Filter(datasource, Active = true));
Set(varIsLoading, false);
```

---

## useEffect (update) → OnChange / Timer

### React
```tsx
useEffect(() => {
  filterResults(searchTerm);
}, [searchTerm]);
```

### Power Apps
```powerapps
// OnChange do Text Input de busca:
UpdateContext({varSearchTerm: Self.Text});
ClearCollect(colFiltered, Filter(colItems, StartsWith(Title, varSearchTerm)))

// Ou com debounce usando Timer:
// Timer com AutoStart = !IsBlank(varSearchTerm)
// Timer.OnTimerEnd: ClearCollect(colFiltered, Filter(...))
```

---

## useEffect (unmount) → OnHidden

### React
```tsx
useEffect(() => {
  return () => {
    cleanup();
  };
}, []);
```

### Power Apps
```powerapps
// Propriedade OnHidden da tela:
Clear(colItems);
UpdateContext({varIsLoading: true});
```

---

## useReducer → Switch / If encadeado

### React
```tsx
const [state, dispatch] = useReducer(reducer, initialState);
dispatch({type: 'increment'});
```

### Power Apps
```powerapps
// Use uma variável de estado e funções de atualização:
UpdateContext({
  varStatus: If(varAction = "approve", "approved",
              If(varAction = "reject", "rejected",
              If(varAction = "pending", "pending", varStatus)))
})

// Ou use uma coleção como estado complexo:
Patch(colState, LookUp(colState, Key = "count"), {Value: LookUp(colState, Key = "count").Value + 1})
```

---

## useMemo → Variável calculada

### React
```tsx
const filteredItems = useMemo(() => 
  items.filter(item => item.active), 
  [items]
);
```

### Power Apps
```powerapps
// Variável calculada em OnChange ou OnVisible:
Set(varFilteredItems, Filter(colItems, Active = true))

// Ou use diretamente na propriedade Items de uma galeria:
Filter(colItems, Active = true)
```

---

## useCallback → N/A (Power Apps não tem funções inline)

### React
```tsx
const handleClick = useCallback(() => {
  setCount(c => c + 1);
}, []);
```

### Power Apps
```powerapps
// No Power Apps, ações são sempre declarativas.
// Use controles invisíveis (botões) ou OnSelect de controles visuais.

// Exemplo: botão invisível sobre um card HTML:
btnCardAction.OnSelect = UpdateContext({varSelectedId: ThisItem.ID})
```

---

## useRef → Controle com nome

### React
```tsx
const inputRef = useRef<HTMLInputElement>(null);
inputRef.current?.focus();
```

### Power Apps
```powerapps
// Controles nativos têm propriedades acessíveis diretamente:
// txtSearch.SetFocus()  — não existe diretamente

// Alternativa: use a propriedade AutoFocus ou Timer:
Timer com AutoStart = true, Duration = 100, OnTimerEnd = Select(txtSearch)
```

---

## Context API → Variáveis Globais

### React
```tsx
const ThemeContext = createContext('light');
```

### Power Apps
```powerapps
// Variável global no OnStart:
Set(gblTheme, "light")
Set(gblPrimaryColor, "#2563eb")
Set(gblUser, User())

// Acesso em qualquer tela:
// gblTheme, gblPrimaryColor, gblUser.FullName
```

---

## Custom Hook → Componente Reutilizável

### React
```tsx
function useFetch(url) {
  const [data, setData] = useState(null);
  useEffect(() => { fetch(url).then(r => r.json()).then(setData); }, [url]);
  return data;
}
```

### Power Apps
```powerapps
// Crie uma tela ou componente reutilizável no Power Apps.
// Ou use uma fórmula padronizada em uma variável/coleção.

// Exemplo: função de busca padronizada:
// ClearCollect(colData, Filter(datasource, Condition));
// Set(varIsLoading, false);
// Set(varHasError, IsEmpty(colData));
```

---

## Eventos (onClick, onSubmit) → OnSelect

### React
```tsx
<button onClick={() => setOpen(true)}>Abrir</button>
<form onSubmit={handleSubmit}>
```

### Power Apps
```powerapps
// Botão nativo:
// btnOpenModal.OnSelect = UpdateContext({varModalOpen: true})

// Botão invisível sobre HTML Text:
// btnHtmlAction.OnSelect = UpdateContext({varSelectedId: varItemId})

// Submit de formulário:
// frmData.OnSuccess = Navigate(scrSuccess, ScreenTransition.Fade)
// frmData.OnFailure = UpdateContext({varFormError: frmData.Error})
```

---

## Resumo Rápido

| React | Power Apps |
|-------|-----------|
| `useState` | `UpdateContext` / `Set` |
| `useEffect` (mount) | `OnVisible` |
| `useEffect` (update) | `OnChange` / Timer |
| `useEffect` (unmount) | `OnHidden` |
| `useMemo` | Variável calculada |
| `useCallback` | Controle `OnSelect` |
| `useRef` | Nome do controle / Timer |
| `Context` | Variáveis globais (`Set`) |
| `props` | Variáveis de contexto / `ThisItem` |
| `children` | Concatenação de strings |
| `.map()` | `Concat()` / `ForAll()` |
| `.filter()` | `Filter()` |
| `.find()` | `LookUp()` |
| `.reduce()` | `Sum()` / `CountRows()` / loop manual |
| `onClick` | `OnSelect` |