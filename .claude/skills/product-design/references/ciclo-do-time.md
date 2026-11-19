# Ciclo do time

Um time pequeno e sênior, com um objetivo só. Seis papéis, que podem ser seis agentes ou uma pessoa trocando de chapéu. Todos leem o mesmo brief e o mesmo backlog, e tudo fica em `projeto/`.

## Papéis e entregas

| Papel | Pergunta que responde | Entrega |
| --- | --- | --- |
| Estratégia | Qual é o problema certo e como saberemos que resolvemos? | `projeto/brief.md` |
| Produto (PM) | O que entra primeiro e como testar que ficou pronto? | `projeto/backlog.md`, `projeto/decisoes.md` |
| Design de produto | Qual o caminho mais simples para a tarefa? | `projeto/design/fluxos.md`, `exploracoes.md`, `spec-telas.md` |
| Design visual e sistema | Como isso parece, e com quais peças se repete? | `projeto/design/design-system.md`, `tokens.css` |
| Engenharia de protótipo | Dá para fazer? Como? Funciona de verdade? | `projeto/arquitetura.md`, `projeto/app/` com testes |
| Pesquisa e verificação | As pessoas conseguem? Está bom o bastante para entregar? | `projeto/pesquisa/`, `projeto/verificacao/` |

## As rodadas

Não é cascata. Engenharia e pesquisa entram no primeiro dia.

1. **Kickoff.** Rascunho do brief, refinado pela estratégia.
2. **Em paralelo:** backlog com critérios de aceite testáveis, e viabilidade com arquitetura. A engenharia diz cedo o que os dados e os sistemas permitem, antes de o conceito endurecer.
3. **Explorar.** Pelo menos cinco alternativas de verdade para cada tela P0 (ver [alternativas.md](alternativas.md)). Escolha uma ou duas por tela, com o motivo ligado à métrica do brief. Registre as descartadas e por quê.
4. **Dar forma.** Direções visuais, grid, tipografia, tokens. Microcopy já nasce limpa (ver [texto-e-superficie.md](texto-e-superficie.md)).
5. **Prototipar.** As histórias P0 em código, com testes, usando os tokens.
6. **Testar e verificar.** Rodada com usuários e parecer de qualidade.
7. **Iterar** de 3 a 6 até as métricas do brief aparecerem. No fim de cada rodada, transforme as correções em regras (ver [aprender-o-gosto.md](aprender-o-gosto.md)).

## Modelo de brief

```markdown
# Brief: <projeto>

## Problema em uma frase
Para <público>, que precisa <necessidade>, vamos <solução> para que <resultado>.

## Público e contexto de uso
## Objetivo e métricas
- Objetivo:
- Métrica principal:
- Métrica de apoio:
## Restrições
(técnicas, legais, prazo, marca, sistemas existentes)
## Hipóteses, da mais arriscada para a menos
1. <hipótese>. Como testar:
## Escopo do primeiro protótipo
## Perguntas em aberto
```

Uma hipótese sem "como testar" não está pronta. A mais arriscada é a primeira a ir para o protótipo.

## Backlog

Cada história tem prioridade (P0, P1, P2), a hipótese que ajuda a testar e critérios de aceite que um teste consegue verificar ("ao digitar um pedido que não existe, a tela diz que não encontrou e oferece buscar pelo e-mail"), nunca adjetivos ("experiência fluida").

## Spec de tela

Para cada tela escolhida: hierarquia de conteúdo, componentes, todos os estados (carregando, vazio, erro, sem conexão, sem permissão, um item, muitos itens), microcopy real, comportamento em celular e desktop, e onde o movimento ajuda ou deve ficar de fora.

## Pesquisa

- **Plano da rodada:** objetivo, perfil, cinco participantes, tarefas e o que conta como sucesso.
- **Roteiro:** aquecimento, tarefas sem induzir a resposta, perguntas de acompanhamento, encerramento.
- **Síntese:** o que funcionou, onde travaram (frequência e gravidade), falas marcantes e a decisão que cada achado pede. Achado sem decisão é curiosidade.

## Parecer de verificação

Arquivo `projeto/verificacao/parecer-AAAA-MM-DD.md`, com uma das três conclusões: aprovado, aprovado com ressalvas ou reprovado. Seções:

1. Critérios de aceite P0, cada um com a evidência (teste, tela, passo a passo).
2. Acessibilidade: contraste, teclado, foco, textos alternativos, leitor de tela.
3. Estados esquecidos.
4. Consistência com o design system.
5. Texto e superfície: contagem por regra antes e depois, textos reescritos, tarefas.
6. Movimento: tabela Antes, Depois, Por quê, e o veredito. Um bloqueio de movimento impede o "aprovado".
7. Pior caso: o que quebrou com dados extremos e a correção de cada item.
8. O resultado ainda serve à métrica principal?

Seja cético com o que parece pronto, principalmente com código gerado por IA.

## Apresentar o trabalho

Uma tela bonita virou o mínimo: qualquer agente faz uma. O que convence é como o trabalho é mostrado e contado.

- **Mostre funcionando, não descrito.** Link do protótipo, gravação curta da interação e pares de captura antes e depois valem mais que qualquer parágrafo.
- **Comece pelo problema e pela métrica do brief,** depois a solução. Cada escolha vem com o porquê ligado ao que se quer melhorar.
- **Mostre as alternativas descartadas** e por que perderam: é a prova de que a escolhida não foi a primeira ideia.
- **Mostre os casos difíceis:** a tela com nome enorme, vazia, com erro, no celular. Eles provam que o trabalho aguenta o mundo real.
- **Diga o que ainda não sabe** e qual teste vai responder.
- Escreva o texto da apresentação com as mesmas regras de escrita das telas: frases curtas, sem slogan, sem adjetivo vazio.

## Registro de decisões

`projeto/decisoes.md`, uma entrada por decisão: data, o que foi decidido, alternativas consideradas, motivo, quem decidiu. O Tiago participa das decisões em tempo real; decisões que mudam o objetivo são dele.
