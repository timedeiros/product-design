# Movimento

Movimento bom quase não se nota: a interface parece ouvir a pessoa. Movimento ruim se nota sempre: atrasa, chega do lugar errado, trava ou se repete cem vezes por dia. Na dúvida, o movimento mais forte é apagar.

## 1. O portão: deve animar?

Toda animação passa pelas duas perguntas antes de qualquer curva. Às vezes o resultado certo é zero linha de código.

**Frequência**

| Quantas vezes a pessoa vê | Decisão |
| --- | --- |
| Centenas por dia (atalho de teclado, paleta de comandos, navegação principal) | Nada de animação |
| Dezenas por dia (hover, navegar em lista, alternar com frequência) | Quase imperceptível, ou nada |
| De vez em quando (modal, gaveta, aviso, configurações) | Animação padrão |
| Raro ou primeira vez (boas-vindas, sucesso, estado vazio, comemoração) | Aqui mora o orçamento de encanto |

Ação disparada pelo teclado nunca anima. Não é questão de gosto: repetida centenas de vezes, qualquer duração vira atraso.

**Propósito.** Nomeie em uma destas palavras, ou não construa:

- **Retorno:** confirmar que a interface ouviu (botão que afunda ao apertar).
- **Continuidade espacial:** mostrar de onde veio e para onde foi (aviso que entra e sai pela mesma borda).
- **Mudança de estado:** tornar legível que algo mudou.
- **Evitar salto:** costurar um conteúdo que, sem isso, se teletransporta.
- **Explicação:** mostrar como algo funciona (só em página de apresentação ou primeira vez).
- **Encanto:** permitido só no nível raro.

"Fica bonito" num elemento frequente é motivo para parar. E dado que a pessoa está lendo ou usando (gráfico, tabela, número de painel) não se mexe por estilo; só se anima uma mudança real de valor causada pela pessoa.

## 2. Ferramenta: a mais barata que resolve

| Precisa de | Use |
| --- | --- |
| Hover, aperto, cor, alternância por classe ou atributo | Transição CSS |
| Entrada ao montar, sem estado em JS | `@starting-style` (fallback: atributo `data-montado` ligado num efeito) |
| Movimento pré-definido que tem que ficar liso com a página ocupada | Animação CSS em `transform`/`opacity` |
| Controle programático com desempenho de CSS, sem biblioteca | Web Animations API (`elemento.animate()`) |
| Molas, animação de layout, saída de componente desmontado, valor guiado por gesto | Biblioteca de movimento (ex.: Motion) |

Não instale biblioteca para um fade. Se a tarefa é um componente (aviso, gaveta, menu de comandos, diálogo, seleção), use um componente acessível pronto em vez de montar à mão: é assim que se acaba com um `<div>` de dropdown sem foco gerenciado.

## 3. Propriedades

- **`transform` e `opacity` por padrão.** Elas pulam layout e pintura. `clip-path` é a exceção aceita; `height` só em sanfona, com a altura medida em JS (nunca animar para `auto`).
- **Nunca de `scale(0)`.** Nada no mundo surge do nada. Comece em `scale(0.95)` (faixa 0.9 a 0.97) com `opacity: 0`.
- **Origem no gatilho.** Popover, menu, dica e seleção crescem a partir do elemento que os abriu (`transform-origin` no gatilho; bibliotecas headless costumam expor uma variável para isso). Modal é exceção: não tem gatilho ancorado, fica centralizado.
- **Porcentagem no `translate`** é relativa ao próprio elemento: `translateY(100%)` desloca a própria altura, seja qual for o conteúdo. Prefira a pixels fixos.
- **`scale()` leva os filhos junto** (texto, ícone). No aperto de botão, é isso que faz parecer físico.
- **Nunca `transition: all`.** Liste as propriedades.

## 4. Curva e duração

Tokens de fallback (use os do projeto se existirem):

```css
--curva-saida: cubic-bezier(0.23, 1, 0.32, 1);       /* entra/sai: forte no começo */
--curva-percurso: cubic-bezier(0.77, 0, 0.175, 1);   /* algo na tela indo de A para B */
--curva-folha: cubic-bezier(0.32, 0.72, 0, 1);       /* folha presa à borda da tela */
```

| Situação | Curva |
| --- | --- |
| Entrando ou saindo | `--curva-saida` |
| Movendo ou se transformando na tela | `--curva-percurso` |
| Hover e mudança de cor | `ease` |
| Movimento constante (letreiro, barra de progresso, segurar para confirmar) | `linear` |
| Na dúvida | `--curva-saida` |

**Nunca `ease-in` em interface.** Começa devagar justo no instante em que a pessoa está olhando. As curvas prontas do CSS são fracas para movimento deliberado; use curvas fortes, e pegue uma curva nova de uma biblioteca de curvas em vez de inventar.

| Elemento | Duração |
| --- | --- |
| Retorno de aperto | 100–160 ms |
| Dica, popover pequeno | 125–200 ms |
| Menu, seleção | 150–250 ms |
| Modal, gaveta, folha | 200–500 ms |
| Aviso (toast) | ~400 ms com `ease`, mais macio de propósito |
| Apresentação e explicação | Pode ser mais longo |

**Interface fica abaixo de 300 ms.** Modal, gaveta, folha e aviso são as exceções. Uma folha pequena que flutua (sem encostar na borda) volta para ~200 ms. Velocidade percebida importa tanto quanto a real: um menu de 180 ms parece mais responsivo que um de 400; depois da primeira dica aberta, as vizinhas abrem na hora, sem atraso e sem animação; um indicador de carregamento que gira mais depressa faz a mesma espera parecer mais curta (uma volta em 0,6 a 0,8 s, não em 1,5 s).

**Mola no lugar da curva** quando há arrasto com impulso, algo que deve parecer vivo, gesto que pode ser revertido no meio ou efeito decorativo que segue o mouse:

```js
{ type: "spring", duration: 0.4, bounce: 0 }    // padrão: sem ultrapassar
{ type: "spring", duration: 0.5, bounce: 0.2 }  // só depois de um gesto com força
```

Quique (0.1 a 0.3) só quando a pessoa aplicou força: soltar um arrasto, arremessar um card. O mesmo elemento fechado por um botão volta com quique zero. Elementos pequenos pedem um pouco mais de quique que os grandes para parecer iguais. Mola esquisita costuma se resolver com mais amortecimento.

## 5. Interrupção e saída

- **Transição, não keyframe,** para tudo que se dispara rápido (avisos empilhando, alternâncias, expandir e recolher). Transição parte do valor atual; keyframe recomeça do zero e dá salto.
- **Sempre a partir do valor na tela,** nunca do valor-alvo lógico. Ao interromper, leia a posição atual e comece dali.
- **Nunca trave a entrada durante uma transição.** A pessoa pode pegar de novo algo que está fechando.
- **Sai por onde entrou.** Painel que entrou pela direita sai pela direita; aviso que sobe de baixo se dispensa arrastando para baixo.
- **Tempo assimétrico onde a pessoa decide.** A fase deliberada é lenta (segurar para apagar: 2 s linear, porque mostra tempo passando); a resposta do sistema é rápida (soltar antes: 200 ms com `--curva-saida`). Em geral a saída é um pouco mais rápida que a entrada.
- **Crossfade que mostra dois estados sobrepostos** e não melhora com curva nem duração: um desfoque leve (`blur(2px)`) durante a troca funde os dois num só. Fique abaixo de 20 px.

## 6. Vários elementos juntos (coreografia)

O erro típico é somar: cada filho ganha um atraso, o painel entra e depois os itens pingam, e a pessoa espera a interface terminar de se apresentar. Boa coreografia é cortar partes móveis e rodar o resto em paralelo.

**Regras**

1. **Uma entrada por contêiner.** Se o painel desliza, o conteúdo já está lá dentro.
2. **Nada de escalonar em superfície frequente ou de teclado** (itens de menu, linhas de lista, conteúdo de aba).
3. **Escalonamento uniforme é proibido.** Atraso, distância e duração iguais para todos parecem metrônomo. Varie pela importância; atrasos que encolhem ao longo do grupo parecem orgânicos. Cada tipo de elemento ganha seu próprio tempo: um título desloca mais e dura mais que um botão, um ícone só aparece.
4. **Nunca bloqueia o clique.** Tudo visível já é clicável.
5. **Sobrevive a interrupção.** Disparar de novo no meio para o que está em voo antes de começar o novo; nunca empilha.

**Elenco.** Antes de qualquer tempo, liste o que se mexe e dê um papel: **protagonista** (o que a pessoa precisa notar: move primeiro e mais), **apoio** (segue com movimento menor), **enfeite** (só aparece, sem deslocar, ou nada), **parado** (todo o resto, inclusive contêineres cujo conteúdo anima). Dois protagonistas é problema de hierarquia, que movimento não resolve.

**Relação entre cada par:**

- **Juntos** se são o mesmo objeto ou o mesmo momento, ou se a ordem não carrega sentido (ordem sem sentido é só atraso).
- **Sobrepostos** (escalonados) se a ordem tem sentido e mostrar os dois juntos não quebra nada. Interface de produto: 30–80 ms entre itens e o grupo inteiro em ~300 ms; em lista longa, escalone só os primeiros. Primeira visita ou apresentação: passos de 100–150 ms em entradas de 0,5–0,6 s.
- **Em sequência** só se juntos pareceria quebrado: dois estados ricos num contêiner que muda de largura e altura, um elemento que passaria através de outro, causa e efeito (o quadrado desenha, depois o visto). Comece o segundo um pouco antes do primeiro acabar: é isso que transforma fila em gesto.
- **Carregar:** quando não dá para controlar o filho, anime o pai e deixe os filhos irem junto.

**Detalhes que fazem diferença**

- Elemento que sai deixa de ocupar espaço na hora (posicionamento absoluto, ou o modo "pop layout" da biblioteca), para os vizinhos fecharem o buraco enquanto ele some.
- Trocar de visão no mesmo contêiner é crossfade: a velha sai enquanto a nova entra.
- Sub-animações de um componente compartilham a mesma família de tempo e curva.
- Efeito secundário se prende ao momento certo do protagonista (o clique, o impacto, o pouso), não a um atraso "que parece bom".
- Duração proporcional à distância: um salto pequeno com o fade inteiro parece piscar. Derive e limite (`clamp(150ms, delta/500, 270ms)`).
- Avançar move o conteúdo para a esquerda; voltar, para a direita. Passe a direção para a animação de saída, que lê o estado de quando começou.
- Saídas são simultâneas, mais curtas e simples: nada de escalonar na saída.
- Animação ociosa que convida a interagir espera ~2,5 s antes da primeira vez, repete raramente (~6 s) e para durante a interação.

Antes de codar, escreva a linha do tempo:

| Elemento | Papel | Começa | Duração | Movimento | Relação |
| --- | --- | --- | --- | --- | --- |

## 7. Gestos

Enquanto o dedo está na tela, a interface é o dedo. Quando ele sai, a física termina o movimento. Misturar as duas fases é a causa número um de gesto que parece errado.

- **Acompanhar (dedo na tela):** posição 1:1 com o ponteiro, sem curva, sem mola, respeitando o ponto onde a pessoa pegou. Use captura de ponteiro para continuar mesmo fora do elemento. Escreva `transform` direto no elemento arrastado (nunca uma variável CSS no pai: ela herda e recalcula todos os filhos). Nada de estado do React a cada movimento. Ignore um segundo dedo depois que o arrasto começou.
- **Limite elástico:** passar da borda move uma fração da distância, com resistência crescente, em vez de parar seco.
- **Soltar:** mola alimentada com a velocidade do dedo, para não haver costura entre arrastar e animar. Dispensa por distância (40–50% do percurso) **ou** por velocidade: um peteleco curto basta. Retorno rápido quando não passou do limite.
- **Projetar o impulso:** escolha o ponto de parada mais próximo de onde o gesto iria parar, não de onde soltou.
- **Valores ligados** (o fundo escurecendo conforme a gaveta desce) derivam da posição do gesto, nunca de animações separadas.
- **Intenção:** exija ~10 px de movimento antes de decidir a direção; dê realce no toque (pressionar), confirme ao soltar.
- **Toque antes de travar a rolagem:** num componente dentro de uma página que rola (mapa, carrossel, gráfico arrastável, assinatura), o primeiro gesto de passagem rola a página. O componente só captura o arraste depois de um toque que o ativa (com estado visível de "ativo"), e devolve a rolagem ao tocar fora ou ao terminar. Assim ninguém fica preso tentando passar por ele.
- **Toque:** alvo de 44×44 px (amplie com pseudo-elemento invisível), `touch-action` no eixo certo, teste em aparelho real.
- Gaveta e folha completas (impulso, fundo acompanhando, foco preso, Esc) dão semanas de casos de borda: use um componente pronto e só construa à mão o que nenhum cobre.

## 8. Desempenho

O orçamento é 16,7 ms por quadro a 60 Hz e 8 ms a 120 Hz. Projete para o menor. O notebook do desenvolvedor é o aparelho menos representativo.

| Custo | Propriedades |
| --- | --- |
| Só composição (barato) | `transform`, `opacity` |
| Pintura | `box-shadow`, `border-radius`, `color` |
| Layout (caro) | `width`, `height`, `padding`, `margin`, `top`, `left`, `border-width` |

Trocas: crescer com `scale()` em vez de tamanho; mover com `translate()` em vez de posição; esconder com `opacity` e depois `visibility: hidden`; sombra animada com `filter: drop-shadow()` (meça).

Diagnóstico, nesta ordem (nomeie a causa antes de mudar código):

1. Qual propriedade anima? Layout e sombra são os primeiros suspeitos.
2. O travamento coincide com a página ocupada (navegação, carregamento, hidratação)? Leve para CSS ou Web Animations, que rodam fora da thread principal. Animação em `requestAnimationFrame` e animação de layout de biblioteca rodam sempre na thread principal.
3. O React re-renderiza a cada quadro? Escreva em `ref.style` ou num valor de movimento.
4. Piora quando o conteúdo cresce? Variável CSS herdada ou nós demais animando.
5. Desfoque animado acima de 20 px?
6. Ainda desloca um pixel no começo ou fim? Só agora `will-change: transform`, no elemento que anima, nunca em contêiner grande.

Confirme gravando (painel de desempenho, câmera lenta) num celular intermediário, não pela sensação.

## 9. Movimento reduzido e hover

Vai junto com a animação, sempre.

```css
@media (prefers-reduced-motion: reduce) {
  .elemento { animation-name: aparecer; } /* mesmo tempo, só opacidade */
}
@keyframes aparecer { from { opacity: 0; } }

@media (hover: hover) and (pointer: fine) {
  .elemento:hover { /* todo hover mora aqui: toque gera hover fantasma */ }
}
```

Reduzir é menos e mais suave, não zero: mantenha opacidade e cor que ajudam a entender; tire deslocamento, mola, paralaxe e escalonamento (o grupo aparece junto). No gesto, o acompanhamento fica (é a pessoa produzindo o movimento); somem arremesso e quique. Respeite também transparência reduzida (superfície mais sólida) e contraste maior (borda definida).

## 10. Os quatro trabalhos

**Construir uma animação.** Portão, propósito, ferramenta, propriedades, curva ou mola, interrupção e saída, movimento reduzido. Entregue o código e três linhas: resultado do portão, ingredientes, o que conferir a olho.

**Revisar movimento** (a postura é desconfiar: aprovação se conquista). Tabela Antes, Depois, Por quê, citando `arquivo:linha`, e depois o comentário agrupado do mais grave ao menos grave: sensação quebrada (curva lenta, surge do nada, anima em ação frequente ou de teclado), simplificações perdidas, desempenho, interrupção e tempo, origem e coesão, acessibilidade. Termine com **Bloquear** ou **Aprovar**. Bloqueia: qualquer sensação quebrada, animação em teclado ou superfície frequente, `scale(0)` ou `ease-in` em interface, propriedade de layout com troca fácil. Ordem de preferência dos consertos: apagar, reduzir, corrigir curva, corrigir origem, tornar interrompível, levar para composição, tempo assimétrico, polimento, acessibilidade.

Sinais para marcar na hora: `transition: all`; `scale(0)` ou fade puro sem transformação inicial; `ease-in`; animação em atalho; interface acima de 300 ms sem motivo; origem no centro em popover; keyframe em aviso ou alternância; propriedade de layout animada; variável CSS no pai movendo filho; sem movimento reduzido; hover sem portão; tempo simétrico em segurar e soltar; tudo entrando de uma vez onde cabia um escalonamento.

**Auditar o app inteiro** (só lê, não edita). Reconhecimento (pilha, bibliotecas, onde mora o movimento, tokens, personalidade do produto, mapa de frequência), auditoria em oito frentes (propósito e frequência, curva e duração, origem, interrupção, desempenho, acessibilidade, coesão e tokens, oportunidades), confira cada achado na linha citada, tabela por alavanca (impacto sobre esforço) com gravidade alta, média ou baixa, até quatro oportunidades, e pare para o Tiago escolher. Para cada escolhido, escreva um plano autocontido em `planos/NNN-nome.md`: caminhos exatos, trecho atual, valores exatos, passos, limites de escopo e como conferir. Quem executa não tem contexto nem gosto.

**Faxina completa** (quando o pedido é "arruma as animações do app"): sempre nesta ordem, porque cada passo diminui o trabalho do seguinte. Primeiro corte o que não precisa existir; depois melhore curva, duração, origem e interrupção do que ficou; por último meça o desempenho só do que sobreviveu.

**Procurar oportunidades ou cortar excesso.** São filtros, não listas de desejos. Para procurar: varra retornos ausentes (clicável sem `:active`), estados que se teletransportam, painéis sem ligação com o gatilho, grupos que surgem de uma vez, gestos sem física, momentos raros sem encanto; passe cada candidato pelo portão e entregue no máximo 5 a 7, com os rejeitados e o motivo. Para cortar: o ônus da prova se inverte (teclado, centenas por dia, dado funcional e sem propósito são apagados; dezenas por dia são reduzidos para cor ou opacidade em até 150 ms; loop infinito em elemento fixo roda uma vez). Antes de apagar, confira os riscos: estado inicial escondido (`opacity: 0` que nunca mais volta), código que espera o fim da animação, saída que segurava a desmontagem, animação de layout escondendo um salto, foco movido no fim da animação, testes que esperam a classe. Depois limpe keyframes, wrappers e imports órfãos, e proponha (não faça) remover a dependência. Liste o que ficou de propósito.

## 11. Celular nativo (React Native e Expo)

Mesmo portão e mesmas tabelas, com três mudanças: não existe hover (todo retorno vai para o aperto); existem duas threads, e o movimento tem que rodar na de interface (worklets), nunca dependendo de estado a cada quadro; e o dedo está em cima do elemento, então interrupção e passagem de velocidade são o mínimo. Háptico só em momentos que importam (confirmar, encaixar, erro), disparado no mesmo quadro do visual. Julgue a sensação numa build de produção, no aparelho mais lento disponível.

## Vocabulário rápido

Para pedir o efeito certo: **fade** (opacidade), **slide** (entra deslizando), **scale in** (cresce ao aparecer), **pop** (aparece com leve ultrapassagem), **reveal** (descoberto por recorte ou máscara), **stagger** (um depois do outro), **crossfade** (um some enquanto o outro aparece no mesmo lugar), **morph** (uma forma vira outra), **shared element** (um elemento viaja e se transforma de um lugar a outro), **layout animation** (anima ao mudar de tamanho ou posição), **origin-aware** (cresce a partir do gatilho), **rubber-band** (resistência ao passar do limite), **hold to confirm** (preenche enquanto segura), **swipe to dismiss** (arrasta para fechar), **number ticker** (dígitos rolando), **tabular nums** (dígitos de largura fixa), **skeleton** (placeholder de carregamento), **jank** (quadros perdidos).
