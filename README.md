# Grade de Disciplinas

Organize suas disciplinas, enxergue a semana de uma vez e compare opções de grade antes de fechar o semestre. O projeto funciona no navegador e guarda os dados localmente.

## O que dá para fazer

- Criar mais de uma versão da grade e duplicar uma versão para comparar cenários.
- Cadastrar disciplinas com vários encontros, professor, sala e cor.
- Editar e remover disciplinas; buscar pelo nome, código ou docente.
- Visualizar os encontros em uma grade semanal responsiva.
- Identificar encontros sobrepostos. Horários adjacentes, como 9h–10h e 10h–11h, não contam como conflito.
- Exportar os horários para CSV.
- Continuar de onde parou: as versões são persistidas no `localStorage`.

## Executar localmente

Requer Node.js 20.19 ou superior.

```bash
npm install
npm run dev
```

Abra o endereço mostrado pelo Vite (normalmente `http://localhost:5173`). Para gerar a versão de produção: `npm run build`.

## Como estudar o código

1. **Entrada do React:** `src/main.tsx` monta o componente principal e carrega os estilos.
2. **Estado e persistência:** `src/App.tsx` mantém as versões da grade em estado React. Um efeito sincroniza as alterações com `localStorage`; ao abrir o app, o valor salvo é carregado e, na primeira visita, uma grade de demonstração dá contexto à interface.
3. **Modelo:** `Plan` representa uma versão; `Course` tem informações da matéria e uma lista de `Meeting`; cada encontro associa um dia, início, fim e local.
4. **Conflitos:** os pares de disciplinas e encontros são comparados. Há conflito quando o dia coincide e `início A < fim B && início B < fim A`. Essa comparação estrita permite aulas consecutivas sem falso positivo.
5. **Formulário:** `CourseModal` mantém os campos enquanto o formulário está aberto e valida horários antes de chamar `onSave`. O formulário serve tanto para criar como para editar.
6. **Renderização:** a lista de encontros é convertida em blocos posicionados dentro do calendário. A mesma fonte de dados alimenta o resumo e a lista de disciplinas.

## Decisões e próximos passos

Esta primeira versão é client-side, sem conta ou sincronização entre dispositivos. `localStorage` é apropriado para uma ferramenta pessoal de demonstração, mas não para dados que precisem de backup ou compartilhamento. Uma evolução natural seria adicionar testes unitários para a regra de conflitos, importação/exportação JSON e uma API para sincronização.

## Tecnologias

React · TypeScript · Vite · CSS · Lucide · Web Storage API
