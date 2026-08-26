# Relatório de Arquitetura e Modelagem (MVP)

Este documento apresenta a modelagem técnica da solução focada no Produto Mínimo Viável (MVP) do projeto **SIGAA de Bolso**, atendendo às especificações da Etapa 2.

**Descrição do Fluxo do Sistema:**
Para esta etapa do projeto, o aplicativo foi desenvolvido utilizando uma arquitetura híbrida, uma estratégia que combina duas formas distintas de funcionamento: uma parte roda localmente no dispositivo (sem necessidade de internet) e outra conecta-se à rede para acessar recursos externos avançados.

Essa estrutura foi escolhida para a criação do MVP (Minimum Viable Product ou Produto Mínimo Viável), que consiste na versão mais enxuta e funcional da solução. O objetivo do MVP é testar e validar as ideias centrais do sistema com o menor custo de tempo possível antes de desenvolver a versão completa.

O funcionamento dessa arquitetura divide-se nos seguintes componentes técnicos:

* Telas de Consulta (Grade Horária e Faltas): Operam consumindo um arquivo estático local no formato Mock JSON.
* Módulo de Assistente Virtual: Conecta-se diretamente à API do Gemini para executar o processamento em nuvem em tempo real.

**Diagrama de Arquitetura (Mermaid.js)**

# Arquitetura da Solução — SIGAA de Bolso

## 1. Fluxo do sistema
```mermaid
flowchart TD
    A[Estudante] --> B{Ação no App}
    B --> C[Login / Autenticação]
    B --> D[Consultar Grade Horária]
    B --> E[Consultar Faltas]
    B --> F[Perguntar ao Chatbot IA]
    C --> G[(Cache Local / Mock JSON)]
    D --> G
    E --> G
    F --> H[API do Gemini.ia]
    H --> F

```

## 2. Arquitetura em camadas

```mermaid
graph LR
    Frontend[App Mobile - React Native/Expo] --> Services[Camada de Serviços]
    Services --> Auth[Módulo de Autenticação]
    Services --> Academico[Módulo Acadêmico]
    Services --> IA[Módulo de Chatbot IA]
    Auth --> Cache[(Cache Local / Mock JSON)]
    Academico --> Cache
    IA --> Gemini[API do Gemini.ia]

```

## 3. Modelo de dados (entidades principais)

```mermaid
erDiagram
    ESTUDANTE ||--o{ MATRICULA : possui
    MATRICULA }|--|| DISCIPLINA : pertence
    MATRICULA ||--o{ REGISTRO_FALTA : registra
    ESTUDANTE ||--o{ MENSAGEM_CHAT : envia

    ESTUDANTE {
        int id PK
        string matricula
        string nome
        string curso
    }
    DISCIPLINA {
        int id PK
        string codigo
        string nome
        string local_sala
        string horario
    }
    REGISTRO_FALTA {
        int id PK
        int quantidade_faltas
        int limite_maximo
    }
    MENSAGEM_CHAT {
        int id PK
        string pergunta
        string resposta
        date data_envio
    }

```

## 4. Justificativa das escolhas

* **App Mobile e Cache Local (Offline-First)**: atende ao RNF01 e RNF02 (acesso instantâneo à grade e locais de aula mesmo sem conexão com a internet).
* **Módulo de Autenticação e Mock Data Store**: cobre RF01, RF02, RNF03 e RNF07 (garante a simulação do fluxo de login e leitura segura dos dados para o MVP).
* **Módulo Acadêmico**: cobre RF03 e RF04 (exibição estruturada da grade semanal, horários, prédios/salas e acompanhamento do limite de faltas).
* **Módulo de Chatbot IA (API Gemini)**: cobre RF08 e RF09 (interface de conversa em linguagem natural para tirar dúvidas acadêmicas).
* **Link para o Trello:** https://trello.com/invite/b/6a8a36de3ae7e7515fed6ab6/ATTI3d6fdeff5870f66e1a879dad36b4a57d581CE463/projeto-sigaa-de-bolso
