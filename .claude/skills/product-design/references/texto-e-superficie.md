# Texto e superfície

Objetivo: a tela parar de parecer gerada por IA. Mantenha as palavras de que a pessoa precisa para agir e corte o resto. Por padrão, mexa só em texto e enfeite. A parte de superfície (S1 a S4) só entra quando o pedido inclui visual, ou na verificação antes da entrega.

## Como trabalhar

1. **Conte.** Rode as buscas do fim deste arquivo no alvo e anote os achados por regra.
2. **Leia** cada texto visível e cada enfeite. Marque cada problema com o código da regra.
3. **Dê o veredito:** pesado (problemas na maioria das regras, ou o mesmo repetido pela tela toda: passe por todas), leve (poucos: corrija só esses e não toque no resto) ou limpo (não edite nada).
4. **Edite.**
5. **Conte de novo** com as mesmas buscas. Sobrar achado é normal quando cada um ficou de propósito. Nunca apague algo legítimo para zerar a conta.
6. **Revise o diff:** sobrou invólucro vazio, margem órfã, import sem uso? Todo campo ainda tem rótulo visível? Todo botão de ícone ainda tem nome? Se der para abrir a tela, olhe: um buraco onde havia um elemento também é sobra.
7. **Relate** no formato da skill, com a tabela de contagem:

| Regra | Antes | Depois | Mantidos |
| --- | --- | --- | --- |

## Texto da interface

- **T1. Sobretítulo.** Texto pequeno em caixa alta ou espaçado acima do título ("RECURSOS", "COMO FUNCIONA"), inclusive em pílula com bolinha e como legenda sobre uma lista. Apague: o título já diz o que é a seção.
- **T2. Numeração de enfeite.** "01 02 03", "01 //", "§". Apague, a não ser que a ordem seja real e a pessoa a siga (passos de um checkout, um guia de instalação).
- **T3. Subtítulo por reflexo.** Uma linha embaixo de todo título ou de todo item de menu. Fica só a que diz um fato que o título não diz. "Relatórios / Entenda seus dados" perde a segunda linha. Em listas, a descrição fica só nos itens que o nome sozinho não distingue (planos, configurações).
- **T4. Explicar o que se vê.** "Clique no botão abaixo para começar", "Este gráfico mostra a receita". Apague. Estado vazio, erro e confirmação destrutiva não somem: viram uma ou duas frases simples dizendo o que aconteceu e o que fazer, sem "Ops!" e sem exclamação.
- **T5. Bastidor vazado.** Texto sobre como a tela funciona ou por que foi desenhada assim, parênteses longos, "(atualiza em tempo real)", "(com IA)". Apague, ou leve para uma dica que alguém procuraria de verdade.
- **T6. Linha de status em corrente.** "12 itens · Atualizado há 2 min · Sincronizado · v2.1". Fique com o fato que a pessoa usa para agir. Uma vírgula ou ponto médio isolado ("3 jan · 14:02") é pontuação normal.
- **T7. Rótulo em dobro.** Rótulo que repete o placeholder, o ícone, o botão ou o título. Escolha uma pista. Em formulário fica o rótulo visível; o placeholder que o repete sai (um que mostra formato, "nome@empresa.com.br", pode ficar); a ajuda que repete o rótulo sai; uma restrição que ninguém adivinha ("mínimo de 12 caracteres") fica.
- **T8. Slogan picotado.** "Simples. Rápido. Seu.", "Feito diferente." Vira uma frase que nomeia a coisa e o que ela faz, com palavras que a tela já usa, ou sai.

Tamanho: título de seção, de card ou legenda dentro do app tem até 5 palavras. O título principal de uma página pode ser uma frase.

## Enfeite

Teste de estado real (vale para T6, E1 e E2): o elemento nasce de uma condição ou de um valor (`{online && ...}`, `status === "ativo"`, um dado da consulta). Marcação que sempre aparece com texto fixo é enfeite.

- **E1. Selo sem estado.** "NOVO", "AO VIVO", "IA", "Beta", "Mais vendido". Fica só se reflete um dado que o app acompanha.
- **E2. Bolinha de status.** Ponto colorido ou pulsando ao lado de um texto. Fica só ligado a um status real, com `aria-label`. Esqueleto de carregamento pulsando não é bolinha de status.
- **E3. Ícone e emoji de enfeite.** Em títulos, botões, rótulos de indicador e listas de recursos, inclusive o ícone num quadrado colorido acima de cada card (sai junto com o quadrado). Fica o ícone que é o próprio controle (com nome acessível), o que carrega sentido que o texto não tem, e o de uma barra ou menu de ações onde todo item tem um.

## Gráficos e painéis

- **G1. Título que nomeia o tipo.** "Gráfico de barras da receita mensal" vira "Receita mensal". Com dado fixo (um relatório), o título pode ser a conclusão: "A receita dobrou desde março". Com dado ao vivo, fica neutro, porque a conclusão envelhece.
- **G2. Instrução de leitura.** "Cada barra representa um mês", "Passe o mouse para ver". Apague.
- **G3. Dizer duas vezes.** Cada quantidade tem uma codificação. Barra, número em cima, linha de grade na altura e marca no eixo dizendo o mesmo é demais. Indicador fica com o número, uma pista de direção e o período de comparação: "▲ 12% vs. mês passado".
- **G4. Andaime que compete com o dado.** Moldura em volta do gráfico, grade escura ou densa, marcas menores, eixo grosso, 3D, sombra nas marcas, fundo atrás da área. Apague, ou deixe em linha fina cinza-clara. Dado em cima, rótulo depois, andaime por último. Não redesenhe: mesmo tipo, mesmos dados, mesmas cores de série.
- **G5. Legenda onde cabe rótulo direto.** Escreva o nome da série na ponta da linha, de preferência com o último valor ("Este ano 1.150"). A legenda fica com mais de quatro séries ou marcas próximas demais.

## Escrita (vale para toda frase que fica ou que você escreve)

Rótulos, botões, abas, cabeçalhos e selos são fragmentos por natureza ("Últimos 30 dias"). As regras sobre frases valem só para frases inteiras: chamada principal, descrições, estados vazios, erros.

- Sem travessão em frase: use ponto ou vírgula. Um "—" sozinho representando valor vazio fica, e a meia-risca em intervalo ("jan–mar") também.
- Caixa de frase em títulos e botões: "Ver pedidos", não "Ver Pedidos".
- Um nome por coisa. Se é "projeto", não vira "espaço" e "área" na tela seguinte.
- Diga o que faz, não como se sente. "Finanças, simplificadas" é sentimento; nomeie o mecanismo ou o número. Se a frase caberia igual na página de outro produto, corte.
- Uma ideia por frase. Voz ativa: "O editor salva suas alterações".
- Sem gerúndio de efeito no fim ("garantindo uma experiência...", "mostrando forte crescimento"): corte.
- Sem "não é só X, é Y". Diga o ponto.
- Sem trio forçado. Use o número natural de itens.
- Sem conclusão genérica ("O futuro chegou"). Fato ou corte.
- Palavra simples no lugar da pomposa: "usar" (não "utilizar"), "fazer" (não "realizar"), "para" (não "visando"), "tem" (não "conta com").
- Vocabulário que denuncia IA, para cortar ou trocar: crucial, fundamental, robusto, potencializar, alavancar, impulsionar, elevar, desbloquear, empoderar, jornada, fluido, sem complicação, perfeitamente, facilmente, inovador, revolucionário, tudo em um, poderoso, incrível.
- Advérbio de exagero (rapidamente, significativamente): corte ou dê o número.
- Tratamento consistente: escolha "você" e mantenha.

## Superfície (só quando o pedido inclui visual)

Estas regras trocam um valor por outro melhor. Nunca mudam paleta, família tipográfica, tamanhos de texto, raios, estrutura do layout ou ordem das seções.

- **S1. Degradê e brilho.** Degradê de um matiz para outro em superfície, botão ou item ativo, texto em degradê, brilho colorido, mancha borrada de cor atrás do conteúdo. Troque por superfície sólida neutra, cor de texto sólida, sombra neutra pequena (`0 1px 2px rgb(0 0 0 / 0.05)`; para algo flutuante, `0 8px 24px rgb(0 0 0 / 0.06)`) ou borda fina na cor do texto a 6–10% de opacidade. Variação de tom dentro de um mesmo neutro pode ficar.
- **S2. Espaço fora da escala.** Valores avulsos como `13px` ou `7px`. Escala: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64; dentro de um controle também 2, 6, 10 e 14. Irmãos com a mesma função usam o mesmo valor. Espaço dentro do grupo é menor que entre grupos. Um valor ímpar que centraliza algo exatamente pode ficar.

  | Onde | Valor |
  | --- | --- |
  | Dentro de um controle | 2 a 12 |
  | Entre linhas de uma lista | 2 a 8 |
  | Entre grupos | 12 a 24 |
  | Entre seções | 32 a 64 |
  | Respiro interno de card | 16 a 24, igual entre irmãos |

- **S3. Peso pesado demais.** 800 e 900 viram 600, ou 500 onde os irmãos já usam. A hierarquia vem de tamanho e cor.
- **S4. Movimento de enfeite.** `transition: all`, card que cresce no hover, quique, mola em interface simples. Ver [movimento.md](movimento.md); o movimento que explica algo fica.

## Limites

- Mexa só em texto visível e enfeite (mais andaime de gráfico em G4, e superfície quando pedida). Não toque em lógica, dados, cores, tokens de espaço ou estrutura.
- Ao apagar um elemento, apague o que só existia por ele: invólucro vazio, invólucro com um filho só, margem, import, variável.
- Nunca apague `aria-label`, `alt`, `title` de botão de ícone ou texto só para leitor de tela. Se o texto cortado era o único nome acessível, leve-o para `aria-label`.
- Campo de formulário mantém rótulo visível ligado ao input (`for` e `id`, ou `<label>` envolvendo). Nunca troque por placeholder.
- Texto que vem de dados, arquivo de tradução ou CMS: edite na fonte ou liste como tarefa. Não fixe no componente.
- Afirmação que o código não comprova (número de clientes, disponibilidade, depoimento, logos de clientes): não apague nem reescreva. Liste como tarefa para confirmar.
- Caso marcado que merece ficar (estado real, ordem real, texto legal): mantenha e diga por quê.

## Exemplos

Chamada com sobretítulo e slogan:

```html
<!-- antes -->
<p class="uppercase tracking-widest text-xs">Conheça o Caixa</p>
<h1>Contas. Em dia. Sempre.</h1>
<p>A plataforma completa que ajuda equipes modernas a organizar o dinheiro de forma simples e sem complicação.</p>

<!-- depois -->
<h1>Organize o dinheiro da sua equipe</h1>
```

Indicador:

```jsx
// antes
<Tile><span>💰 Receita total</span><strong>R$ 48.200</strong><span>▲ 12% de alta</span>
  <p>A receita cresceu 12% em relação ao mês passado, mostrando forte crescimento.</p></Tile>
// depois
<Tile><span>Receita</span><strong>R$ 48.200</strong><span>▲ 12% vs. mês passado</span></Tile>
```

Estado vazio: "Ops! Nada por aqui ainda 👀 Parece que você ainda não criou nenhum projeto!" vira título "Nenhum projeto" e descrição "Crie um projeto para ele aparecer aqui."

## Buscas (regex do ripgrep, pule `node_modules` e build)

Achado é candidato, não veredito. Conte uma vez por elemento.

- T1: `uppercase.{0,200}tracking-(wide|wider|widest|\[)`, `text-transform:\s*uppercase` (cabeçalho de tabela e tecla não contam)
- T2: `>\s*0[1-9]\s*<`, `['"]0[1-9]['"]`, `padStart\(\s*2\s*,\s*['"]0['"]` (datas e horas não contam)
- T6: dois ou mais `·`, `•`, `&middot;` em poucas linhas; `join\(\s*['"]\s*(·|•|\|)\s*['"]\s*\)`
- E1: `>\s*(NOVO|AO VIVO|IA|BETA|PRO|NEW|LIVE|AI)\s*<`, `>\s*(Mais vendido|Em alta|Destaque|Com IA|Em breve|Popular)\s*<`
- E2: `animate-(pulse|ping)`, `rounded-full` em elemento de 1 a 3 de tamanho
- E3: com `-P`, `[\x{1F300}-\x{1FAFF}\x{2600}-\x{27BF}\x{2B50}\x{2B55}]`
- G1: `(?i)\bgr[aá]fico de (barras|linhas|pizza|[aá]rea)\b`, `(?i)\b(bar|line|pie) chart of\b`
- G2: `(?i)\bcada (barra|ponto|linha|coluna) (representa|mostra)\b`, `(?i)passe o mouse`
- G4: `drop-shadow\(|shadowBlur|shadowColor`, grade ou borda com espessura 1,5 ou mais
- G5: `legend\s*:\s*\{[^}]*(display|show)\s*:\s*true`, `<(Chart)?Legend\b`
- Travessão: `—|&mdash;|&#8212;`
- Vocabulário: `(?i)\b(potencializ|alavanc|impulsion|desbloque|empoder|fluid[ao]|sem complica|perfeitamente|facilmente|revolucion|tudo em um)`
- S1: `(linear|radial|conic)-gradient\(`, `bg-gradient-`, `bg-clip-text`, `shadow-\[0_0_`, `blur-(2xl|3xl)`
- S2: `-?(m|p)[trblxy]?-\[\d+px\]`, `gap:\s*\d+px`, `(margin|padding)[a-z-]*:\s*\d+px` (conta quando é ímpar, ou acima de 16 e não múltiplo de 4)
- S3: `font-(extrabold|black)`, `font-weight:\s*[89]00`
- S4: `transition-all`, `transition:\s*all`, `hover:scale-`, `animate-bounce`

T3, T4, T5, T7, T8 e G3 nenhuma busca encontra: exigem leitura.
