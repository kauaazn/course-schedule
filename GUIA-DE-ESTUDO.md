# Guia de estudo — Grade de Disciplinas

Este roteiro acompanha uma funcionalidade de cada vez. Leia o trecho indicado, rode o app e explique o comportamento com suas palavras antes de avançar.

## 1. Entenda o modelo

Abra `src/App.tsx` e procure pelos tipos `Meeting`, `Course` e `Plan`.

- `Meeting` descreve um encontro da disciplina: dia, início, fim e sala.
- `Course` reúne os dados da matéria e um ou mais encontros.
- `Plan` guarda o nome da versão da grade e sua lista de matérias.

Pergunta para responder: por que os horários estão em `Meeting`, em vez de guardar só um horário dentro de `Course`?

## 2. Acompanhe o estado

Na função `App`, encontre `useState` para `plans`, `active`, `modal` e `editing`.

`plans` contém todas as versões. `active` indica qual versão está aberta. `editing` guarda a disciplina que o formulário está alterando; `null` significa que ele vai criar uma nova.

O primeiro carregamento lê `localStorage`. Um `useEffect` grava as alterações. A chave antiga é consultada como fallback para preservar uma grade salva antes da mudança de nome do projeto.

## 3. Siga o fluxo de cadastro

1. O botão **Adicionar disciplina** abre `CourseModal`.
2. O formulário mantém os campos em estados locais.
3. `submit` impede o envio padrão, valida nome e horários e chama `onSave`.
4. `saveCourse` acrescenta a disciplina ou troca a versão que tem o mesmo `id`.
5. React redesenha a lista, o calendário e os indicadores; o efeito persiste os dados.

Leia `CourseModal` e depois procure `saveCourse` dentro de `App`.

## 4. Entenda a regra de conflitos

Em `App`, a variável `conflicts` compara cada par de disciplinas e cada par de encontros. Só existe conflito quando o dia é igual e:

```text
início A < fim B  e  início B < fim A
```

Os sinais são estritos. Assim, uma aula que termina às 10h e outra que começa às 10h não se sobrepõem. A mesma lista `conflicts` alimenta o aviso e o destaque dos blocos.

## 5. Leia a interface

Em `src/style.css`, procure `@media`. Essas regras reorganizam a navegação, o calendário e a tabela para telas menores. Em `src/main.tsx`, veja o ponto de entrada que monta `<App />` na página.

## 6. Faça estes exercícios

1. Adicione duas disciplinas que se cruzem por 20 minutos. Identifique a condição que reconhece o conflito.
2. Mude o segundo encontro para começar exatamente quando o primeiro termina. Confirme por que o aviso desaparece.
3. Recarregue a página. Descubra qual efeito e qual chamada ao `localStorage` preservam os dados.
4. Duplique uma grade, edite a cópia e explique por que a grade original continua igual.
5. Adicione um novo campo a `Course` e acompanhe a mudança no formulário, na lista e na exportação CSV.

## 7. Pratique uma explicação curta

Sem ler o código, explique: como uma disciplina chega do formulário ao calendário? O que é salvo no navegador? Como o programa distingue uma sobreposição de duas aulas consecutivas?

Depois confira as respostas seguindo o fluxo nos arquivos. Se não conseguir responder uma parte, volte apenas à função correspondente e trace os dados com o depurador do navegador.
