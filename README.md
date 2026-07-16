# comic-metadata

Base de metadados de quadrinhos (personagens, editoras, universos, equipes, criadores e gêneros), versionada, multilíngue e desacoplada de qualquer aplicativo específico.

Este repositório é a fonte de dados "editável". Qualquer aplicação (InkShelf ou outra) que precise classificar, exibir ou buscar informações sobre quadrinhos pode consumi-lo — diretamente ou através de um pacote compilado gerado a partir destes arquivos.

## Filosofia

- **Dados de origem, não dados otimizados.** Os arquivos aqui são pensados para serem fáceis de ler e editar por humanos. Otimizações de performance (índices de busca compilados, banco SQLite, etc.) devem ser geradas por uma ferramenta de build separada a partir destes arquivos — nunca editadas manualmente aqui.
- **Estrutura e tradução são coisas diferentes.** `data/` contém apenas relações e fatos estruturais (IDs, referências entre entidades, caminhos de imagem). Textos voltados ao usuário (nome, descrição, ocupação) vivem em `i18n/<locale>/`, indexados pelo mesmo ID.
- **IDs são slugs estáveis.** `spider-man`, `marvel`, `avengers` — minúsculos, com hífen, nunca traduzidos. É o contrato entre `data/`, `i18n/` e `images/`.
- **Sem catálogo de eventos/séries.** O app trabalha em cima dos arquivos que o próprio usuário já tem, não de uma lista editorial de edições — por isso não existe `events.json`/`series.json`. `relatedCharacters` continua existindo, mas é sempre opcional: um personagem pode não ter nenhum, e o usuário pode não ter nenhum arquivo do personagem relacionado na biblioteca dele.

## Estrutura

```
comic-metadata/
├── version.json          # versão do pacote de metadados e versão mínima de app compatível
├── packages.json          # lista de pacotes que podem ser baixados separadamente (core, marvel, dc, ...)
├── data/                  # fatos estruturais por tipo de entidade (sem texto para humanos)
│   ├── characters.json
│   ├── publishers.json
│   ├── universes.json
│   ├── teams.json
│   ├── creators.json
│   ├── genres.json
│   ├── aliases.json       # variações de nome por idioma, usadas para gerar o índice de busca
│   └── search_index.json  # nome normalizado -> id; tratar como derivado (ver "Índice de busca")
├── i18n/
│   ├── pt-BR/  en-US/  es-ES/  fr-FR/  de-DE/
│   │   ├── characters.json   # name, description, occupation, ...
│   │   ├── publishers.json
│   │   ├── universes.json
│   │   ├── teams.json
│   │   ├── creators.json
│   │   └── genres.json
├── images/
│   ├── characters/<id>/{avatar,banner,logo,icon}.webp
│   ├── publishers/<id>/...
│   ├── teams/ universes/ creators/
└── assets/
    ├── logos/  backgrounds/  placeholders/
```

## Convenções por arquivo

### `data/characters.json`

```json
{
    "spider-man": {
        "publisher": "marvel",
        "universe": "marvel-universe",
        "created": 1962,
        "firstAppearance": "amazing-fantasy-15",
        "creators": ["stan-lee", "steve-ditko"],
        "teams": ["avengers", "fantastic-four"],
        "genres": ["superhero"],
        "relatedCharacters": ["venom", "green-goblin", "mary-jane", "miles-morales"],
        "images": {
            "avatar": "images/characters/spider-man/avatar.webp",
            "banner": "images/characters/spider-man/banner.webp",
            "logo": "images/characters/spider-man/logo.webp",
            "icon": "images/characters/spider-man/icon.webp"
        }
    }
}
```

Os demais arquivos de `data/` (`publishers.json`, `teams.json`, `universes.json`, `creators.json`, `genres.json`) seguem o mesmo padrão: chave = ID em slug, valor = apenas relações e fatos, sem texto traduzível.

### `data/aliases.json`

Variações de nome por idioma, usadas para *gerar* o índice de busca — não são o índice em si. Cobre todos os tipos de entidade (personagens, editoras, times, universos, gêneros), não só personagens — como os IDs são únicos entre tipos (ver "IDs são slugs estáveis" acima), todos cabem no mesmo mapa por idioma sem ambiguidade.

```json
{
    "pt-BR": {
        "spider-man": ["homem-aranha", "homem aranha"],
        "marvel": ["marvel", "marvel comics"],
        "avengers": ["vingadores"]
    },
    "en-US": {
        "spider-man": ["spider-man", "spiderman", "asm"],
        "marvel": ["marvel", "marvel comics"],
        "avengers": ["avengers"]
    }
}
```

### `data/search_index.json` — tratar como derivado

Mapeia nome normalizado → `{ type, id }`. É consultado pelo app ao classificar um arquivo, para evitar percorrer todos os aliases. O campo `type` diz em qual arquivo de `data/` procurar o registro completo (`characters.json`, `publishers.json`, `teams.json`, `universes.json` ou `genres.json`).

```json
{
    "homem-aranha": { "type": "character", "id": "spider-man" },
    "spider-man": { "type": "character", "id": "spider-man" },
    "marvel": { "type": "publisher", "id": "marvel" },
    "vingadores": { "type": "team", "id": "avengers" },
    "universo marvel": { "type": "universe", "id": "marvel-universe" },
    "faroeste": { "type": "genre", "id": "western" }
}
```

**Não edite este arquivo manualmente.** Ele deve ser (re)gerado a partir de `characters.json`/`publishers.json`/`teams.json`/`universes.json`/`genres.json` + `aliases.json` por uma ferramenta de build, para nunca ficar dessincronizado. Está commitado por conveniência enquanto não existe uma pipeline de build separada — quando essa ferramenta existir, ele deixa de ser fonte e passa a ser artefato de release.

### `i18n/<locale>/characters.json`

```json
{
    "spider-man": {
        "name": "Homem-Aranha",
        "description": "Peter Parker é um super-herói criado por Stan Lee e Steve Ditko.",
        "firstAppearance": "Amazing Fantasy #15",
        "occupation": "Fotógrafo"
    }
}
```

### `images/`

Um diretório por ID: `images/<tipo>/<id>/<classe>.webp`. Os caminhos referenciados em `data/*.json` devem sempre apontar para dentro de `images/`. Nem toda entidade precisa ter todas as classes — só inclua a chave em `images` quando o arquivo existir.

#### Classes de imagem

| Classe   | Descrição                                                              | Proporção de referência |
|----------|-------------------------------------------------------------------------|--------------------------|
| `avatar` | Retrato/close-up, quadrado. Rosto do personagem ou do autor.            | 1:1                      |
| `banner` | Imagem larga, cinematográfica. Usada em cabeçalhos e telas de detalhe.  | 16:9                     |
| `logo`   | Logotipo/wordmark oficial da entidade (editora, equipe, franquia).      | livre (fundo transparente ou de marca) |
| `icon`   | Versão simplificada/reduzida, para listas, badges e tabs.               | 1:1                      |

#### Classes aplicáveis por tipo de entidade

| Tipo         | Classes aplicáveis                  |
|--------------|--------------------------------------|
| `characters` | `avatar`, `banner`, `logo`\*, `icon` |
| `publishers` | `logo`, `banner`, `icon`             |
| `universes`  | `banner`, `logo`                     |
| `teams`      | `banner`, `logo`                     |
| `creators`   | `avatar`, `banner`                   |
| `genres`     | nenhuma (usa `icon` como nome simbólico de ícone, não caminho de imagem — ver `data/genres.json`) |

\* `logo` em `characters` é opcional: só faz sentido quando o personagem tem um emblema/wordmark próprio usado oficialmente em capas e merchandising (ex.: o aranha do Homem-Aranha). Nem todo personagem tem um.

### `version.json`

```json
{
    "version": 1,
    "releaseDate": "2026-07-15",
    "minimumAppVersion": 1,
    "metadataVersion": 1
}
```

Incrementar `version` a cada mudança publicada. Um app consumidor compara esse valor com a última versão baixada para decidir se atualiza.

### `packages.json`

Permite baixar subconjuntos dos dados (por editora, por exemplo) em vez do repositório inteiro.

```json
{
    "packages": [
        { "id": "core", "version": 1 }
    ]
}
```

## Como montar uma entidade completa

Um consumidor monta os dados de exibição combinando `data/`, `i18n/<locale>/` e `images/` pelo mesmo ID:

```
data/characters.json["spider-man"]
    + data/publishers.json["marvel"]
    + i18n/pt-BR/characters.json["spider-man"]
    + images/characters/spider-man/*
    ↓
CharacterDetails
```

## Roadmap

- [ ] Ferramenta de build separada (`comic-metadata-builder`) para validar relações, gerar `search_index.json` automaticamente e empacotar releases.
- [ ] Publicar pacotes compilados (`metadata.zip` ou `metadata.db`) via GitHub Releases, deixando de versionar artefatos derivados neste repositório.

## Licença

MIT — veja [LICENSE](LICENSE).
