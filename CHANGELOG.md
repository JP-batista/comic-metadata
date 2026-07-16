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

### Changed

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
