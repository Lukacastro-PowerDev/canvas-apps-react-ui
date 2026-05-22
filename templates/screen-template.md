# Template de Nova Tela

> Use este template para descrever uma nova tela ao Copilot. Preencha cada seção e o Copilot gerará a implementação completa.

---

## 1. Identificação

| Campo | Valor |
|-------|-------|
| Nome da tela | `scr[Nome]` |
| Objetivo principal | [O que o usuário faz nesta tela?] |
| Público-alvo | [Desktop / Mobile / Ambos] |
| Tela anterior | [De onde o usuário vem?] |
| Tela seguinte | [Para onde vai após concluir?] |

---

## 2. Estrutura de Componentes React-like

Liste os componentes que esta tela usará:

```
<AppShell>
  <Header
    title="..."
    subtitle="..."
    showBack={true/false}
    primaryAction="..."
    userAvatar={...}
  />
  <main>
    [Conteúdo principal]
  </main>
  [BottomNav?]
</AppShell>
```

---

## 3. Props / Variáveis de Contexto

| Variável | Tipo | Inicial | Descrição |
|----------|------|---------|-----------|
| `varHeaderTitle` | Text | `"..."` | Título do header |
| `varIsLoading` | Boolean | `true` | Estado de loading |
| `varItems` | Collection | `[]` | Dados da lista |
| ... | ... | ... | ... |

---

## 4. Estados de Interface

| Estado | Condição | Componente a renderizar |
|--------|----------|--------------------------|
| Loading | `varIsLoading = true` | `<Loading variant="skeleton" count={3} />` |
| Empty | `CountRows(colItems) = 0` | `<EmptyState icon="📭" title="..." />` |
| Error | `varHasError = true` | `<EmptyState icon="⚠️" title="Erro" ... />` |
| Data | Default | `<CardGrid items={colItems} />` |

---

## 5. Layout Responsivo

| Breakpoint | Colunas | Observações |
|------------|---------|-------------|
| Mobile (< 640px) | 1 | Bottom nav, FAB, scroll vertical |
| Tablet (640-1024px) | 2 | Sidebar colapsada |
| Desktop (> 1024px) | 3+ | Sidebar expandida, painel de detalhes |

---

## 6. Interações

| Elemento | Ação | Resultado |
|----------|------|-----------|
| Card item | OnSelect | Navigate(scrDetail, Fade, {id}) |
| Botão "Novo" | OnSelect | Navigate(scrForm, Fade) |
| Filtro | OnChange | Recarrega colFiltered |
| ... | ... | ... |

---

## 7. Acessibilidade Checklist

- [ ] Título da tela como `<h1>`
- [ ] Contraste ≥ 4.5:1 em todos os textos
- [ ] Áreas tocáveis ≥ 44px
- [ ] Labels em todos os inputs
- [ ] Estados de erro com texto + ícone + cor
- [ ] Foco visível em elementos interativos
- [ ] Ordem de navegação lógica

---

## 8. CSS Adicional Necessário

```css
/* Adicione aqui estilos específicos desta tela */
```

---

**Gerado em**: [DATA]
**Por**: [AUTOR]