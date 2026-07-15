# Changelog

Todas as mudanças notáveis neste repositório são documentadas aqui.

O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/), e o versionamento em `version.json` segue um inteiro incremental simples (`metadataVersion`).

## [Unreleased]

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

### Changed

- `spider-man.universe` passou de `earth-616` para `marvel`, e depois para `marvel-universe`: o campo representa o universo compartilhado da editora, não uma continuidade específica — o mesmo personagem deve valer para 616, Ultimate etc. O ID final `marvel-universe` evita colisão com o ID `marvel` já usado em `publishers.json`.
