# Aprender o gosto

Toda correção do Tiago é gosto que o agente não tinha. Se não for escrita, a próxima sessão erra igual e ele corrige de novo. Rode no fim de cada rodada, na mesma sessão, antes de limpar o contexto.

Este modo só escreve regras. Nunca muda código de componente e nunca inventa regra que o Tiago não expressou.

## Regras

1. **Só o gosto dele.** Cada regra aponta para algo que ele disse, editou ou confirmou. "Parece boa prática" é o seu gosto, e foi justamente ele que estava sendo corrigido.
2. **Não invente o porquê.** Sem motivo dado, proponha um marcado como *inferido* e confirme antes de escrever. Um porquê errado é pior que nenhum: o próximo agente estende o raciocínio para onde ele não serve.
3. **Proponha e pare.** Escreva só o que ele aprovar.
4. **Uma checagem vale mais que uma frase.** Se a violação se encontra por busca no código (valor proibido, cor crua em vez de token, propriedade banida), proponha regra de lint, restrição de token ou teste, e mantenha uma linha escrita com o porquê.
5. **Escreva para quem não estava aqui.** Nada de "como combinamos" ou "aquele card". Nomeie a classe de componente, o valor, o token, o arquivo.

## Onde procurar correções

| Sinal | Exemplo | O que diz |
| --- | --- | --- |
| Correção explícita | "Não, sombra translúcida, não borda" | A regra, muitas vezes com o porquê |
| Edição silenciosa | Ele trocou `400` por `200` num arquivo seu | Seu padrão estava errado; o valor dele é a regra |
| Empurrões repetidos | "Menor." "Ainda grande." "Ok." | Registre onde ele parou, não os passos |
| Desfazer ou rejeitar | "Volta", escolheu a versão B, apagou o que você pôs | O que não fazer; o porquê costuma faltar |
| Escolha não óbvia confirmada | "Isso, deixa o ícone fora do botão" | Só se um agente novo faria diferente |

Ignore o que não é gosto: bug funcional, conteúdo de uma tela só, correção do seu entendimento da tarefa.

## Filtrar e generalizar

- Um agente novo, fazendo **outro** componente neste projeto, cometeria o mesmo erro? Se não, pule.
- Já existe regra que cobre? Se diz o mesmo, ela falhou: reescreva mais estrita (um número, um token, "nunca"). Se contradiz, mostre as duas e ele escolhe.
- Escolha o nível mais estreito que teria evitado o erro no próximo componente diferente. "O card de preços usa sombra" é estreito demais; "nunca use borda" é largo demais; "superfícies elevadas (cards, popovers, menus) usam o token de sombra elevada, nunca borda sólida" é o nível certo. A mesma correção em dois componentes diferentes é evidência para o nível de classe.
- Escreva estrito: valores e tokens deste código, "sempre" ou "nunca". Linguagem suave só quando ele falou como preferência.
- Autoteste: lendo só a regra e o porquê, um agente sem mais nada faria o que ele queria? Se ainda pode errar, acrescente o valor, o token ou a exceção.

## Onde escrever

- Por padrão, no arquivo de regras de design do projeto (`DESIGN.md` na raiz, ou `projeto/decisoes.md` nos projetos do time), na seção certa: Superfícies, Cor, Tipografia, Espaço e layout, Movimento, Interação, Texto.
- Uma seção que passou de ~40 linhas e só importa para alguns trabalhos vira uma skill do projeto, com um ponteiro de uma linha no arquivo.
- Garanta que será lido: se o `CLAUDE.md` não aponta para o arquivo, proponha a linha "Antes de criar ou mudar interface, leia DESIGN.md."

Formato da entrada:

```md
## Superfícies

- **Superfícies elevadas (cards, popovers, menus) usam `--sombra-elevada`, nunca borda sólida.**
  Por quê: borda sólida deixa a superfície chapada e pesada; a sombra translúcida separa do fundo e funciona nos dois temas.
  <!-- 2026-11-19: "não, sombra translúcida, não borda" -->
```

O comentário guarda a data e as palavras dele, para saber qual regra é mais nova quando surgir conflito.

## Relatório

| # | Você disse | Regra | Por quê | Onde |
| --- | --- | --- | --- | --- |

Depois: **precisa da sua resposta** (cada porquê inferido e cada dúvida de nível, com recomendação), **conflitos**, **virar checagem** (a implementação exata e o que ela marcaria hoje) e **ignorados** (uma linha cada). Feche com "diga `escrever tudo` ou `escrever 1, 3`". Se a sessão não teve correção de gosto, diga em uma linha.

Variante de manutenção: reler o arquivo de regras e propor juntar duplicadas, resolver contradições e endurecer as vagas, com a mesma regra de propor e parar.
