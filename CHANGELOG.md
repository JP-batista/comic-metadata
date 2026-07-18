# Changelog

Todas as mudanças notáveis neste repositório são documentadas aqui.

O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/), e o versionamento em `version.json` segue um inteiro incremental simples (`metadataVersion`).

## [Unreleased]

### Added

- 13 novos personagens: `captain-america`, `daredevil`, `hawkeye`, `hulk`, `punisher`, `iron-fist`, `thor`, `wolverine`, `iron-man` (Marvel); `green-arrow` (DC); `invincible`, `spawn` (Image Comics); `tex` (Sergio Bonelli Editore). Dados estruturais completos + traduções pt-BR/en-US/es-ES + aliases/`search_index.json`. Imagens ainda pendentes.
- Novo time: `x-men` (Marvel), com membros. `avengers.members` passou a incluir `hawkeye`.
- Novas editoras: `dc`, `image-comics`, `bonelli`, com dados estruturais e traduções.
- Novos universos: `dc-universe`, `invincible-universe`, `spawn-universe`, `tex-universe` — cada propriedade da Image Comics ficou com seu próprio universo (Image não é uma continuidade compartilhada como Marvel/DC).
- Novo gênero: `western` (para `tex`), com tradução.
- Imagens (`avatar`, `banner`) de `captain-america` e `daredevil`, em arte de quadrinho.
- Imagens (`avatar`, `banner`) de `green-arrow`, `hawkeye`, `hulk`, `invincible`, `iron-fist`, `iron-man`, `punisher`, `spawn`, `tex`, `thor` e `wolverine`, em arte de quadrinho — completa todos os 14 personagens.
- Imagens de editoras: `dc` (logo + banner), `image-comics` (logo + banner), `bonelli` (logo + banner) — completa todas as editoras.
- Banners de `dc-universe`, `invincible-universe`, `spawn-universe`, `tex-universe` — completa todos os universos.
- `x-men`: banner + logo — completa todos os times.
- 2 novos personagens DC: `batman` (Detective Comics #27, 1939, Bob Kane e Bill Finger) e `green-lantern` (Showcase #22, 1959, John Broome e Gil Kane). Dados estruturais completos, imagens (`avatar`, `banner`), traduções pt-BR/en-US/es-ES, aliases e `search_index.json`.
- `data/aliases.json` e `data/search_index.json` passaram a cobrir todos os tipos de entidade (editoras, times, universos, gêneros), não só personagens.

### Changed

- `data/search_index.json` mudou de formato: valores eram um ID simples (`"nome": "spider-man"`), agora são um objeto `{ type, id }` (`"nome": { "type": "character", "id": "spider-man" } }`), necessário para o app saber em qual arquivo de `data/` procurar cada resultado agora que o índice cobre vários tipos.
- Logos de `avengers`, `fantastic-four` e `x-men` substituídos por novas artes de quadrinho.
- `teams.json` ganhou os campos `universe`, `created`, `firstAppearance` e `creators` para os 3 times (mesmo padrão já usado em `characters.json`): Avengers #1 (1963), Fantastic Four #1 (1961) e The X-Men #1 (1963), todos por Stan Lee e Jack Kirby. `firstAppearance` (texto de exibição) também adicionado nas traduções.
- `data/search_index.json` regenerado do zero a partir de `aliases.json` + todos os arquivos de `data/` (109 → 122 entradas). Corrige o alias `invencible` (es-ES) que nunca tinha sido propagado, e passa a incluir o próprio ID de cada entidade como termo pesquisável (antes só as variações cadastradas em `aliases.json` funcionavam).
- Os 22 criadores que ficavam como referência pendente agora têm entrada própria em `creators.json` (nascimento/morte) e tradução (nome + bio) em pt-BR/en-US/es-ES, pesquisados via API do Comic Vine em vez de estimados de memória — algumas datas que tínhamos em mente (dia/mês) estavam erradas, corrigidas agora com fonte verificada.
- 20 novos personagens DC: `superman`, `wonder-woman`, `flash`, `aquaman`, `cyborg`, `martian-manhunter`, `joker`, `catwoman`, `robin`, `sinestro`, `black-canary`, `lex-luthor`, `supergirl`, `shazam`, `nightwing`, `deathstroke`, `zatanna`, `darkseid`, `harley-quinn`, `doctor-fate`. Dados estruturais completos, traduções pt-BR/en-US/es-ES, aliases e `search_index.json`. Imagens ainda pendentes (36 personagens no total agora).
- Novo time: `justice-league` (The Brave and the Bold #28, 1960, Gardner Fox e Mike Sekowsky), resolvendo a referência pendente que `batman`, `green-lantern`, `green-arrow` e outros já apontavam. Imagens ainda pendentes.
- Resolvidas as referências soltas de `relatedCharacters` que existiam desde os lotes anteriores: `joker`, `catwoman`, `robin` (de `batman`), `sinestro` (de `green-lantern`) e `black-canary` (de `green-arrow`) agora têm ficha própria.

- Imagens (`avatar`, `banner`) dos 20 personagens DC, obtidas via Comic Vine API (arte de capa oficial, verificada por nome exato + contagem de aparições para evitar personagens homônimos errados). `robin` usou a capa de Detective Comics #38 (1940) via CoverBrowser, já que no Comic Vine ele e o Asa Noturna compartilham a mesma página de personagem.
- Imagens (`avatar`, `banner`) dos 22 criadores pendentes, com fotos reais obtidas via Comic Vine API.

### Nota

- Durante a pesquisa dos 20 personagens, a busca automática na Comic Vine API retornou registros errados para nomes populares (ex.: um "Superman" alternativo da Tangent Comics, histórias de "Flash" e "Coringa" sem relação com a primeira aparição real) — a base tem múltiplos personagens com nomes iguais/parecidos e a busca simples pegou o registro errado algumas vezes. Nesses casos, os dados estruturais (ano, edição, criadores) vieram do conhecimento já verificado em vez da API, para não propagar erro. Para as imagens, o problema foi contornado buscando por nome exato e priorizando o registro com mais aparições em edições.

### Added (continuação)

- 20 novos personagens Marvel: `black-widow`, `bucky-barnes`, `cyclops`, `elektra`, `green-goblin`, `human-torch`, `invisible-woman`, `jean-grey`, `kingpin`, `loki`, `luke-cage`, `mary-jane`, `mister-fantastic`, `miles-morales`, `professor-x`, `she-hulk`, `storm`, `thing`, `venom`, `war-machine`. Dados estruturais, imagens (avatar/banner via Comic Vine, mesma técnica de nome exato + contagem de aparições), traduções pt-BR/en-US/es-ES, aliases e `search_index.json`.
- `avengers.members` passou a incluir `black-widow` e `war-machine`.

Esse lote resolve **todas** as referências pendentes de `relatedCharacters` e membros de time que existiam desde os primeiros lotes (só sobrou `omni-man`, do Invencível). 56 personagens no total agora, todos com imagens completas.

- Creators referenciados pelos personagens desta leva (ex.: `jack-kirby`, `joe-simon`, `stan-lee`, `bill-everett`, `don-heck`, `gerry-conway`, `john-romita-sr`, `ross-andru`, `roy-thomas`, `gil-kane`, `larry-lieber`, `len-wein`, `herb-trimpe`, `mort-weisinger`, `george-papp`, `robert-kirkman`, `cory-walker`, `todd-mcfarlane`, `gian-luigi-bonelli`, `aurelio-galleppini`) ainda não têm entrada própria em `creators.json` — ficam como referência pendente, no mesmo padrão já usado para `relatedCharacters`.

## [1] - 2026-07-15

### Added

- Estrutura inicial do repositório: `data/`, `i18n/` (pt-BR, en-US, es-ES, fr-FR, de-DE), `images/`, `assets/`.
- `version.json` e `packages.json`.
- README descrevendo convenções de cada arquivo e o fluxo de montagem de uma entidade.
- Primeiro personagem: `spider-man`, com dados estruturais, imagens (`avatar`, `banner`), traduções em pt-BR/en-US/es-ES, aliases e entradas em `search_index.json`.
- Primeira editora: `marvel`, com dados estruturais e imagens (`logo`, `banner`, `icon`).
- Imagens `logo` e `icon` de `spider-man`.
- `universes.json`: `marvel`, com imagens (`banner`, `logo`).
- `creators.json`: `stan-lee` e `steve-ditko`, com datas de nascimento/morte e imagens (`avatar`, `banner`).
- `teams.json`: `avengers` e `fantastic-four`, com membros e imagens (`banner`, `logo`).
- `genres.json`: `superhero`, com ícone simbólico (`hero`).
- Traduções (pt-BR/en-US/es-ES) de `publishers`, `universes`, `creators`, `teams` e `genres`.
- Documentação das classes de imagem (`avatar`, `banner`, `logo`, `icon`, `cover`) e de quais se aplicam a cada tipo de entidade.

### Removed

- `events.json` e `series.json` (dados, i18n e pastas de imagem), em todos os locales. O app trabalha sobre os arquivos que o usuário já tem, não sobre um catálogo editorial de eventos/edições — esses tipos foram considerados específicos demais para o escopo atual.
- Campos `events` e `series` removidos de `spider-man` em `data/characters.json`.

### Changed

- `spider-man.universe` passou de `earth-616` para `marvel`, e depois para `marvel-universe`: o campo representa o universo compartilhado da editora, não uma continuidade específica — o mesmo personagem deve valer para 616, Ultimate etc. O ID final `marvel-universe` evita colisão com o ID `marvel` já usado em `publishers.json`.
- Logo de `marvel` substituído por uma versão em alta resolução com fundo transparente (2000×2000).
