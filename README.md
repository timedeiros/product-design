# Product Design

Uma skill para o Claude Code que leva um produto digital da ideia ao protótipo testado, priorizando conversão e facilidade de uso. Ela decide sozinha qual modo usar a partir do pedido, ou você chama com `/product-design <tela, pasta ou pergunta>`.

## O que ela faz

**Organiza o projeto como um time sênior.** Monta o brief (problema em uma frase, métricas, hipóteses da mais arriscada para a menos), o backlog com critérios de aceite testáveis, a spec de cada tela com todos os estados, o plano e o roteiro de testes com usuários e o parecer de qualidade antes da entrega. Registra cada decisão com o motivo e ajuda a apresentar o trabalho: mostrando funcionando, a partir do problema e com as alternativas descartadas.

**Gera alternativas para você escolher olhando.** Constrói de três a cinco versões realmente diferentes da mesma tela, todas funcionando, atrás de um seletor no rodapé que troca com o teclado. Você escolhe, e ela integra a vencedora no código.

**Tira a cara de IA das telas.** Corta sobretítulos, numeração de enfeite, subtítulos repetidos, textos que explicam o óbvio, slogans picotados, selos e bolinhas sem estado real e ícones decorativos. Limpa gráficos (título, legenda, grade) e, quando pedido, a superfície visual (degradês, espaçamentos fora da escala, pesos exagerados). Escreve microcopy em português simples e nunca inventa números ou benefícios.

**Decide, constrói e revisa movimento.** Antes de animar, pergunta se deve animar, pela frequência de uso e pelo propósito. Escolhe ferramenta, propriedades, curva e duração com valores definidos, coordena vários elementos juntos, constrói gestos que acompanham o dedo e terminam com embalo, e diagnostica animações que travam. Também revisa, audita o app inteiro, procura onde falta movimento e corta o que sobra.

**Testa com dados extremos e dá um polimento.** Alimenta a tela com nomes enormes, e-mails sem quebra, listas vazias, números gigantes, emoji e texto em outros alfabetos, atrás de um seletor de cenários, e relata o que quebrou com a correção de cada item. Roda o app em várias larguras, com toque emulado, em 3G lento e em cenários de sucesso, erro e perfis diferentes, salvando capturas de antes e depois.

**Faz o app parecer nativo no celular.** Corrige hover travado, flash ao tocar, altura errada da tela, zoom ao tocar em campos, toque atrasado, seleção de texto no toque longo, conteúdo sob o entalhe e rolagem que briga com gestos.

**Aprende o seu gosto.** No fim de cada rodada, transforma as suas correções em regras escritas com o porquê, para que a próxima sessão não repita o erro.

## Instalar

```bash
git clone https://github.com/timedeiros/product-design.git
cp -r product-design/product-design ~/.claude/skills/
```

Para um projeto só, copie a pasta para `.claude/skills/` do projeto.
