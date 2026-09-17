# Advice Generator App

Uma solução completa para o desafio de frontend do Frontend Mentor, desenvolvida para gerar conselhos aleatórios consumindo a Advice Slip API. A implementação busca manter a interface fiel ao mockup, priorizando experiência do usuário, performance e acessibilidade.

## Sumário

- [Advice Generator App](#advice-generator-app)
  - [Sumário](#sumário)
  - [Visão geral](#visão-geral)
    - [Contexto do desafio](#contexto-do-desafio)
    - [Objetivo da solução](#objetivo-da-solução)
  - [Requisitos atendidos](#requisitos-atendidos)
  - [Tecnologias e ferramentas](#tecnologias-e-ferramentas)
  - [Arquitetura e estrutura do projeto](#arquitetura-e-estrutura-do-projeto)
    - [Principais arquivos](#principais-arquivos)
  - [Como executar](#como-executar)
    - [Opção 1: abrir diretamente no navegador](#opção-1-abrir-diretamente-no-navegador)
    - [Opção 2: executar localmente com servidor](#opção-2-executar-localmente-com-servidor)
  - [Detalhes de implementação](#detalhes-de-implementação)
    - [Consumo da Advice Slip API](#consumo-da-advice-slip-api)
    - [Lógica de atualização do conteúdo](#lógica-de-atualização-do-conteúdo)
    - [Acessibilidade e UX](#acessibilidade-e-ux)
    - [Robustez e tratamento de erro](#robustez-e-tratamento-de-erro)
  - [Aprendizados e refinamentos](#aprendizados-e-refinamentos)
  - [Resultados alcançados](#resultados-alcançados)
  - [Status](#status)

## Visão geral

### Contexto do desafio

Este projeto foi desenvolvido a partir do desafio "Advice generator app" do Frontend Mentor. O objetivo principal era construir uma interface moderna e responsiva que consumisse a [Advice Slip API](https://api.adviceslip.com) e exibisse um conselho aleatório sempre que o usuário interagisse com o botão principal.

Além do aspecto visual, o desafio exigia atenção a detalhes fundamentais do desenvolvimento frontend, como:

- adaptação mobile-first e responsividade;
- implementação de hover e focus states;
- boa organização semântica em HTML;
- uso eficiente de CSS para manter estética próxima ao protótipo;
- integração dinâmica com API externa;
- acessibilidade para leitores de tela e navegação por teclado.

### Objetivo da solução

A proposta foi criar uma interface simples, elegante e funcional, mantendo a experiência do usuário fluida e intuitiva. A solução também busca demonstrar domínio prático de conceitos importantes como manipulação do DOM, consumo de API, validação de dados, feedback visual e acessibilidade em aplicações reais.

## Requisitos atendidos

A aplicação atende aos principais requisitos esperados pelo desafio:

- layout otimizado para mobile e desktop;
- cartão centralizado com visual próximo ao design reference;
- botão circular para geração de novo conselho;
- hover state para feedback visual;
- atualização dinâmica do conselho e do identificador;
- mensagem de carregamento durante a requisição;
- tratamento de erro em caso de falha de rede ou resposta inválida;
- suporte para leitura assistiva e navegação por teclado;
- prevenção de repetição de conselho em clique consecutivo.

## Tecnologias e ferramentas

- HTML5 semântico
- CSS3 com variáveis e design tokens
- JavaScript ES6+
- Bootstrap 5 (via CDN) e fonte `Manrope` (Google Fonts)
- Fetch API com `AbortController` para timeout e `cache: "no-store"` para evitar respostas em cache
- DevTools para depuração e ajustes visuais
- Acessibilidade web com `aria-live`, `role` e foco visível

## Arquitetura e estrutura do projeto

```text
.
├── css/
│   └── style.css
├── design/
├── images/
├── js/
│   └── app.js
├── AGENTS.md
├── CLAUDE.md
├── index.html
├── README-template.md
├── README copy.md
├── style-guide.md
├── .gitignore
└── README.md (opcional, se renomeado após personalização)
```

### Principais arquivos

- `index.html`: estrutura HTML da página, incluindo card do conselho, botão e elementos acessíveis.
- `css/style.css`: estilos visuais, responsividade, animações sutis, hover e foco.
- `js/app.js`: lógica de consumo da API, atualização do DOM, controle de loading e tratamento de erro.
- `style-guide.md`: guia de paleta, tipografia e padrões visuais do desafio.
- `design/`: arquivos de referência do layout.
- `images/`: assets do projeto, como divider e favicon.

## Como executar

Como é uma aplicação estática, não é necessário instalar dependências externas.

### Opção 1: abrir diretamente no navegador

1. baixe ou clone o repositório;
2. navegue até a pasta do projeto;
3. abra o arquivo `index.html` em um navegador moderno.

### Opção 2: executar localmente com servidor

```bash
cd caminho/para/o/projeto
python -m http.server 8000
```

Em seguida, acesse:

```text
http://localhost:8000
```

## Detalhes de implementação

### Consumo da Advice Slip API

A integração com a API é feita com `fetch` e configurada para `cache: "no-store"`, garantindo que cada clique solicite uma resposta nova (o navegador por padrão pode reaproveitar respostas com cache e bloquear a atualização do conselho).

Também foi usado `AbortController` para aplicar um timeout de requisição (atualmente `8000ms`), evitando requisições pendentes em conexões instáveis.

Além disso, o código tenta reconsultar a API até `3` vezes quando o `id` retornado é igual ao atual, reduzindo a chance do usuário ver a mesma mensagem consecutivamente.

### Lógica de atualização do conteúdo

A atualização do card é feita fora do DOM (clonando o nó, atualizando-o e trocando em uma única mutação), o que mantém a região `aria-live` mais previsível (anúncio atômico).

O parser valida explicitamente o formato da resposta (presença de `slip.id` numérico e `slip.advice` como string não vazia) antes de renderizar. Em caso de formato inesperado, o fluxo lança um erro e exibe uma mensagem amigável ao usuário.

Para melhorar a percepção de resposta, existe também um atraso antes de mostrar a mensagem de carregamento (após `1000ms`), evitando avisos rápidos desnecessários em respostas rápidas.


### Acessibilidade e UX

A aplicação inclui várias melhorias de acessibilidade e usabilidade:

- `aria-live="polite"` e `aria-atomic` para atualizações do conselho sem interromper o usuário;
- mensagens de erro com `role="alert"` para anunciação imediata;
- uso de `aria-disabled` no botão enquanto carrega (mantendo o foco e a navegabilidade por teclado);
- foco visível e estados de hover/focus para feedback perceptível;
- rótulo do botão pode ser temporariamente ocultado com a tecla `Escape` sem mover o foco — o rótulo retorna no próximo `focus` ou `pointerenter`;
- fallback com `<noscript>` para instruir usuários sem JavaScript.

### Robustez e tratamento de erro

O fluxo de erro contempla:

- HTTP não 2xx (exibe mensagem de erro amigável);
- timeout de requisição (AbortController);
- payload com formato inesperado (validação e fallback);
- tentativas adicionais quando a API retorna o mesmo `id`;
- exibição de mensagem de erro amigável ao usuário (texto atual: "Sorry, we couldn't load new advice. Please check your connection and try again.").

## Aprendizados e refinamentos

Durante o desenvolvimento, alguns pontos se destacaram como aprendizados importantes:

- aprofundamento em consumo de API com JavaScript nativo;
- exigência de validação de dados em fluxos dinâmicos;
- importância de manter feedback perceptível para o usuário em interações assíncronas;
- uso estratégico de atributos semânticos e ARIA para melhorar acessibilidade;
- aplicação de mobile-first e atenção aos detalhes visuais que fazem a diferença na percepção de qualidade.

Também foi possível reforçar a importância de:

- manter a UI previsível;
- evitar ações que parecem não responder;
- estruturar lógica em funções pequenas e reutilizáveis;
- planejar acessibilidade desde o início do projeto, e não como ajuste final.

## Resultados alcançados

Entrega de uma solução estável e acessível que reproduz o layout do protótipo, com comportamento robusto frente a falhas de rede e reprodutibilidade reduzida de conselhos repetidos. A base é adequada para evoluções como caching controlado, internacionalização e testes automatizados.

## Status

Projeto concluído e atualizado para refletir melhorias de robustez, mensagens de carregamento e práticas de acessibilidade implementadas no código.
