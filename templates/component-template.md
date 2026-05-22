# Template de Novo Componente

> Use este template para criar um novo componente reutilizável.

---

## 1. Identificação

| Campo | Valor |
|-------|-------|
| Nome do componente | `cmp[Nome]` |
| Inspiração React | `<[Componente] />` do [Biblioteca] |
| Uso | [Onde será usado?] |
| Reutilizável em | [Quais telas?] |

---

## 2. Interface (Props)

```typescript
interface [Nome]Props {
  // Props obrigatórias
  prop1: string;
  prop2: number;

  // Props opcionais
  prop3?: boolean;
  prop4?: 'option1' | 'option2' | 'option3';

  // Eventos
  onAction?: () => void;
}
```

---

## 3. Variáveis Power Apps

| Variável | Tipo | Entrada/Saída | Descrição |
|----------|------|---------------|-----------|
| `var[Nome]Prop1` | Text | Entrada | Descrição |
| `var[Nome]Prop2` | Number | Entrada | Descrição |
| `var[Nome]Html` | Text | Saída | HTML gerado |

---

## 4. Implementação HTML

```powerapps
Set(var[Nome]Html,
  "<div class='[component-class]'>" &
    // Conteúdo do componente
  "</div>"
)
```

---

## 5. CSS do Componente

```css
.[component-class] {
  /* Estilos base */
}

.[component-class]--variant {
  /* Variante */
}

.[component-class]:hover {
  /* Hover state */
}

.[component-class]:focus-visible {
  /* Foco */
}

.[component-class]:disabled,
.[component-class]--disabled {
  /* Disabled state */
}
```

---

## 6. Estados

| Estado | Classe CSS | Condição |
|--------|-----------|----------|
| Default | `[component-class]` | Estado normal |
| Hover | `[component-class]:hover` | Mouse sobre |
| Active | `[component-class]--active` | `varIsActive = true` |
| Disabled | `[component-class]--disabled` | `varIsDisabled = true` |
| Loading | `[component-class]--loading` | `varIsLoading = true` |
| Error | `[component-class]--error` | `varHasError = true` |

---

## 7. Acessibilidade

- [ ] Elemento semântico correto (`<button>`, `<article>`, etc.)
- [ ] `aria-label` quando necessário
- [ ] `aria-disabled` quando desabilitado
- [ ] `role` apropriado
- [ ] Contraste verificado
- [ ] Foco visível

---

## 8. Exemplo de Uso

```powerapps
// Em uma tela:
Set(varHeaderHtml, Header({
  title: "Dashboard",
  showBack: false,
  primaryAction: "Novo"
}))

// No HTML Text:
"<style>" & varGlobalStyles & "</style>" &
varHeaderHtml &
varContentHtml
```

---

**Gerado em**: [DATA]
**Por**: [AUTOR]