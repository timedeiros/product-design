# Estresse e celular

Dado de demonstração é escolhido, quase sempre sem querer, para o layout ficar bonito: nomes que cabem numa linha, números sem separador, todo campo opcional preenchido. Este modo desfaz essa escolha, campo por campo, e depois tira os sinais de "site no navegador" quando a tela roda num celular.

## Parte 1: pior caso

### Regras

1. **Plausível ou do esquema, nunca aleatório.** Cada valor extremo é algo que uma pessoa real produziria, ou o limite real da validação, da coluna do banco ou do contrato da API. Sem limite encontrado, isso já é um achado: diga "sem limite" e teste algo longo mas crível. "aaaaaaa" com 5 mil caracteres não prova nada.
2. **Mude o dado, não o componente.** O pior caso entra pela mesma porta da demonstração (fixture, mock, props, stub da API). Nunca edite marcação ou CSS para provocar a quebra.
3. **Um conjunto, muitas falhas.** Espalhe os casos pelas primeiras linhas (a 1 com nome longo, a 2 com e-mail longo, a 3 com nome de duas letras), como dado real faz.
4. **O seletor é só de desenvolvimento.** Nunca vai para produção.
5. **Relate antes de consertar.** Algumas quebras são decisões de design (cortar ou quebrar a linha?). Conserte só quando pedirem.

### Passos

1. **Mapeie** cada valor que o componente mostra: campo, origem, tipo, limite (ou "sem limite") e se é opcional. Inclua o que se esquece: contadores em cabeçalhos, datas relativas, texto de selo, rótulo de botão vindo de dado, avatar, e o tamanho da própria lista. Anote onde front e back discordam (campo de 50 que salva em coluna de 255).
2. **Monte o pior caso** ao lado da fixture de demonstração, com a mesma forma. Cubra também: **vazio** (zero itens, busca sem resultado), **um** (um item, todo contador em 1) e **enorme** (o limite realista; mais de mil linhas se a lista não pagina).
3. **Ligue o seletor** "Demonstração / Pior caso / Vazio / Um / Mil linhas", fixo no centro inferior, neutro, troca instantânea, guardado na URL (`?dados=pior`).
4. **Quebre:** veja na largura real do contêiner, em 320 px e na mais larga; com zoom de 200%; em modo escuro e com `dir="rtl"` se o produto suporta.
5. **Relate e pare.** Depois, conserte o que for pedido, passe por todos os estados de novo (inclusive a demonstração) e mantenha a fixture como teste de regressão.

### Valores que costumam quebrar

| Tipo | Exemplos |
| --- | --- |
| Nome | `Maria Eduarda Albuquerque-Vasconcellos`, `Luiz Fernando de Souza Bragança Neto` (sufixo engana iniciais), `Bê`, `Z`, `Nguyễn Thị Phương Thảo` (diacríticos empilhados cortados por entrelinha apertada), `李静怡` (sem espaço), `فاطمة الزهراء` (direita para esquerda), `D'Ávila` (apóstrofo), `🌻 Lu` e `👨🏾‍🍳 Davi` (emoji no início quebra `.charAt(0)`), espaços sobrando, nome ausente |
| E-mail, URL, código | `departamento.financeiro.contas-a-receber@distribuidora-exemplo-comercial.example.com`, `a@b.test`, `nome+notas@example.com`, URL longa com parâmetros, UUID, `Proposta comercial FINAL (revisada) v14 [aprovada].pdf`, `IMG_20260914_183022_HDR_editada_editada.HEIC` |
| Texto vindo de dado | cargo de 60 caracteres, selo "Convite expirou há 12 dias", palavra alemã composta sem espaço, tradução 30% mais longa, doze etiquetas num item, uma etiqueta enorme, título vazio ou só espaços, `<script>`, `&amp;`, `**negrito**` (devem aparecer literais), quebra de linha num campo de uma linha, descrição colada com 2 mil caracteres |
| Número e dinheiro | `0`, `1` ("1 membros"), `1284` (separador), `1000000` (forma compacta), `R$ 12.345.678,90`, negativo, `0.1 + 0.2`, `142%` em barra de progresso, `null`/`NaN`, formato de outro idioma, valor que muda ao vivo (`99` → `100`) |
| Coleção | 0, 1, exatamente o tamanho da página e mais um, mais de mil sem paginação, um item 10× maior que os outros, nomes idênticos |
| Tempo | agora ("há 0 segundos"), 12 dias, 11 meses, futuro, `1970-01-01`, horário perto da meia-noite em outro fuso, duração enorme |
| Imagem | avatar que dá 404, sem avatar, panorâmica, imagem altíssima, logo transparente no escuro, carregamento lento sem dimensões |
| Estado | carregando, erro da API com mensagem crua, dados parciais, todos os status numa lista, sem permissão, a própria pessoa na lista |
| Ambiente | 320 px, coluna lateral de 280 px, 2560 px, zoom 200%, modo escuro, RTL, toque (ação que só aparece no hover fica inalcançável) |

Use domínios `example.com` e `.test` para nunca apontar para caixa ou site real.

### O que se vê, por que acontece, como consertar

| Sintoma | Causa | Conserto |
| --- | --- | --- |
| Avatar ou ícone vira oval | Filho flex encolhendo | `flex-shrink: 0` em tudo que tem tamanho fixo |
| Texto vaza da caixa | Filho flex/grid com `min-width: auto` | `min-width: 0` na coluna de texto (`minmax(0, 1fr)` no grid) |
| E-mail ou URL passa da borda | Sem ponto de quebra | `overflow-wrap: anywhere` |
| Ação do fim (menu •••) empurrada para fora | O meio pegou o espaço todo | `min-width: 0` no meio, `flex-shrink: 0` na ação |
| Selo quebra em duas linhas | Selo pode encolher | `white-space: nowrap; flex-shrink: 0`, e decida quem cede |
| Avatar parece solto ao lado de nome de três linhas | `align-items: center` com alturas variáveis | Alinhe pelo topo |
| Última linha cortada no meio da letra | Altura fixa sem rolagem nem degradê | Barra visível ou máscara de fade |
| Palavra quebrada no meio num título | `word-break: break-all` | `overflow-wrap: anywhere` |
| Iniciais erradas ou `�` | `split(' ')[0][0]` | Grafemas com `Intl.Segmenter`, primeira e última palavra pulando sufixos (Jr., Neto, Filho), ícone de fallback |
| Traço órfão onde estaria o cargo | Placeholder para campo vazio | Omita a linha, ou reserve a altura de propósito |
| "1 membros" | Plural fixo | `Intl.PluralRules` ou textos por quantidade |
| Números tremem ao atualizar | Algarismos proporcionais | `font-variant-numeric: tabular-nums` |
| `1284`, `NaN`, `undefined` | Número cru | `Intl.NumberFormat` no idioma da pessoa; proteja nulos |
| Botão traduzido estoura | Largura fixa | Largura pelo conteúdo com `min-width` |
| Diacríticos cortados | Entrelinha apertada com `overflow: hidden` | Entrelinha maior, sem recorte em texto |
| Ícone de imagem quebrada | Sem fallback | Iniciais no erro; `object-fit: cover` |
| Texto cortado sem como ler | Só reticências | `title` ou dica, e o valor inteiro em outro lugar |
| Mil linhas engasgam | Tudo renderizado | Virtualize ou pagine, e diga qual |
| `<b>` ou `&amp;` aparecem crus | Escape na camada errada | Escape uma vez, na renderização |

### Cortar, quebrar ou limitar

Decida por campo, não para a tela toda:

- **Quebrar a linha** no que a pessoa precisa ler inteiro para identificar (nomes, títulos num detalhe). Duas linhas é normal; quatro diz que a coluna é estreita demais.
- **Cortar no fim** em metadado secundário em que o começo carrega o sentido (cargo, prévia de mensagem). Sempre com um jeito de ver tudo.
- **Cortar no meio** quando os itens diferem no fim: arquivos, e-mails do mesmo domínio longo, caminhos, códigos.
- **Limitar a N linhas** (`line-clamp`) em prévias de card, para alturas previsíveis.
- **Nunca cortar** números, valores, datas ou o que se compara.

### Relatório

| # | Gravidade | Campo | Valor extremo | O que acontece | Conserto |
| --- | --- | --- | --- | --- | --- |

Gravidade: **quebrado** (ilegível, ação inalcançável, dado errado), **feio** (legível mas visivelmente errado), **frágil** (ok hoje, a um passo de quebrar). Depois: decisões para o Tiago (com recomendação), o que aguentou, onde está o seletor e "diga `consertar tudo` ou `consertar 1, 3`".

## Parte 2: passada de polimento

Uma rodada de inspeção que o agente faz sozinho antes de mostrar qualquer tela ao Tiago. Rode o app de verdade (servidor local e navegador automatizado) e passe por:

1. **Larguras:** 320, 390, 768, 1024, 1440 e a mais larga suportada.
2. **Toque emulado:** emulação de aparelho com toque ligado, para achar ação que só existe no hover, alvo pequeno e rolagem que briga com gesto.
3. **Rede lenta:** perfil de 3G lento com o cache desligado. Olhe o que a pessoa vê nos primeiros segundos: tela em branco, salto de layout quando imagem ou fonte chega, botão clicável antes de funcionar, indicador de carregamento que pisca em respostas rápidas.
4. **Cenários de estado:** sucesso, erro da API, vazio, carregando, sem permissão e perfis de pessoa diferentes (conta nova, conta antiga cheia de dados, administrador e convidado). Monte cada cenário pela mesma porta dos dados (fixture ou parâmetro na URL), nunca editando o componente.
5. **Tentar quebrar:** os valores extremos da Parte 1.

Para cada problema: tire a captura antes, conserte, tire a captura depois na mesma largura e no mesmo cenário. Salve em `projeto/verificacao/polimento-AAAA-MM-DD/` com nomes que dizem o caso (`390-erro-api-antes.png`, `390-erro-api-depois.png`) e entregue a tabela Antes, Depois, Por quê apontando para os pares. Conserte só o que é claramente defeito; o que é decisão de design vai para a lista de tarefas. O que só um aparelho real mostra (sensação do toque, teclado, entalhe) fica marcado como pendente.

## Parte 3: parecer nativo no celular

Quase todo "está estranho no celular" não é animação: é atraso de toque, flash cinza ou hover que não solta. Quase tudo se resolve com uma linha de CSS ou uma meta tag. Use consulta de capacidade (`hover`, `pointer`, `env()`, `dvh`), nunca adivinhe pelo user agent ou pela largura. Toque e mouse convivem no mesmo aparelho.

| Sintoma | Conserto | Por quê |
| --- | --- | --- |
| Hover preso depois do toque | Todo `:hover` dentro de `@media (hover: hover) and (pointer: fine)` | O navegador simula hover no primeiro toque e não solta |
| Flash cinza ou azul ao tocar | `-webkit-tap-highlight-color: transparent` no `html`, e `:active` próprio em cada clicável | O realce do navegador briga com o seu retorno |
| Altura errada | `100dvh` no app e em gavetas; `100svh` em capa de página | `100vh` é a altura com a barra do navegador recolhida |
| Página dá zoom no campo | Fonte de 16 px nos campos (só em `pointer: coarse` se o desktop pede menor) | Abaixo disso o iOS aproxima e não volta |
| Toque parece atrasado | `touch-action: manipulation` nos clicáveis; retorno no `:active` ou `pointerdown`, 100–160 ms | Retorno no `click` só vem quando o dedo sai |
| Puxar para atualizar sequestra a rolagem | `overscroll-behavior: none` em `html, body`; `contain` nas áreas roláveis internas | Nunca `touchmove` com `preventDefault` |
| Conteúdo para no entalhe | `viewport-fit=cover` e `env(safe-area-inset-*)` no cabeçalho, barra inferior, avisos e folhas | Sem a meta tag, `env()` vale zero |
| Toque longo seleciona texto do botão | `user-select: none` e `-webkit-touch-callout: none` só em controles | Nunca no `body`: endereço e número de pedido são conteúdo |
| Carrossel rola a página junto | `touch-action: pan-y` no carrossel horizontal; ou `scroll-snap` nativo | O valor diz o que o navegador ainda pode fazer |
| Barra de status destoa | Uma `theme-color` por esquema de cor, igual ao topo da página | Uma só cor erra em um dos modos |

Nunca desligue o zoom (`user-scalable=no`, `maximum-scale=1`): é falha de acessibilidade. Conserte a fonte do campo.

Configure o teclado: `inputmode="numeric"` para códigos, `decimal` para valores, `type="email"` e `tel`, `autocapitalize="none"` em usuário e código, `enterkeyhint` dizendo o que o Enter faz.

Base mínima para app que roda no celular (troque as cores pelas do topo da sua página):

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, interactive-widget=resizes-content" />
<meta name="color-scheme" content="light dark" />
<meta name="theme-color" content="#f7f7f5" media="(prefers-color-scheme: light)" />
<meta name="theme-color" content="#111111" media="(prefers-color-scheme: dark)" />
```

```css
html {
  overscroll-behavior: none;            /* tire se for um documento que aceita puxar para atualizar */
  -webkit-text-size-adjust: 100%;       /* sem inflar fonte em paisagem */
  -webkit-tap-highlight-color: transparent;
}
:is(input, select, textarea) { font-size: max(16px, 1rem); }
:is(button, a, [role="button"]) { touch-action: manipulation; }
:is(button, [role="button"]) { -webkit-user-select: none; user-select: none; }
/* e todo :hover dentro de @media (hover: hover) and (pointer: fine) */
```

Nada disso aparece na emulação do navegador. Abra o servidor de desenvolvimento pelo IP da rede num celular de alguns anos atrás, com o inspetor remoto, com o teclado aberto e uma vez em paisagem. Diga o que foi verificado pelo código e o que precisa do aparelho.
