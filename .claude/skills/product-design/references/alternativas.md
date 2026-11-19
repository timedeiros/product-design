# Alternativas

Quando a pergunta é "como isso deve ser?", não se responde em texto. Constroem-se versões que funcionam, lado a lado no tempo (não no espaço), e a pessoa escolhe olhando.

## Regras

1. **Uma peça por rodada.** "O painel inteiro" vira a peça de maior alavanca: diga qual e por quê, e ofereça as outras como próximas rodadas. Reescreva o pedido numa frase: o que é, onde vive, o que precisa fazer.
2. **Cada versão diverge num eixo com nome.** Estrutura, densidade, ordem, modelo mental, modelo de interação, personalidade, história de movimento. Duas versões que só mudam a cor ou o texto são uma versão só: troque uma delas.
3. **Nomes descrevem a direção:** "Silenciosa", "Editorial", "Densa", "Guiada". Nunca "Opção A".
4. **Quantidade.** Três por padrão. Telas P0 de projeto do time pedem cinco. Mais de cinco dilui a comparação.
5. **Toda versão funciona de verdade.** Interações reais, textos reais, nomes e números plausíveis. Sem lorem ipsum, sem botão morto.
6. **Divergir não é desculpa para fazer mal.** Cada versão passa sozinha nas regras de texto, movimento e acessibilidade.
7. **Nada toca o código de produção durante a exploração.** As versões vivem numa rota isolada (`/prototipos/<nome>`) ou num HTML único autocontido. Nada de produção importa dali.

## O seletor

Uma pílula fixa no centro inferior da tela, discreta e neutra, com um botão por versão. Ela é moldura, não concorrente: não usa as cores, fontes nem tokens do projeto.

- Mostra uma versão por vez, em tamanho real e em contexto (um aviso precisa de uma página atrás; um card precisa de vizinhos). Miniaturas lado a lado distorcem escala e espaço.
- Teclas 1 a N e setas trocam de versão; R reinicia a animação de entrada. Ignore teclas quando o foco está num campo ou com modificador pressionado.
- A troca de versão é instantânea, sem animação: é uma ação repetida dezenas de vezes.
- A versão escolhida fica na URL (`?v=2`), para sobreviver ao recarregar.
- Se a peça ocupa o centro inferior (um aviso, uma folha), a pílula sobe para o topo.

## Entrega

Rode tudo, passe por todas as versões, confira o console limpo e tire uma captura de cada. Depois apresente e pare, porque a escolha é do Tiago:

| # | Versão | Eixo | Quando ela vence | O que ela custa |
| --- | --- | --- | --- | --- |

Não marque favorita na tabela. Se perguntarem qual você escolheria, responda pela frequência de uso e pela métrica do brief, não por estética.

Quando houver escolha: integre a vencedora no lugar certo, com as convenções e tokens do projeto, e apague a área de protótipo (a menos que peçam para manter). Se a pessoa pedir outra rodada, gere um novo conjunto divergindo em volta da direção preferida. Registre escolhidas e descartadas em `projeto/design/exploracoes.md`.
