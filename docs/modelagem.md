# Relatório de Arquitetura e Modelagem (MVP)

Este documento apresenta a modelagem técnica da solução focada no Produto Mínimo Viável (MVP) do projeto **SIGAA de Bolso**, atendendo às especificações da Etapa 2.

**Descrição do Fluxo do Sistema**
O aplicativo opera sob uma arquitetura híbrida para o MVP: as telas de consulta (grade horária e limite de faltas) consomem um arquivo estático local (Mock JSON) para garantir exibição imediata e simular a experiência mobile, enquanto o módulo de assistente virtual se conecta diretamente à API do Gemini para processamento em nuvem em tempo real.

**Diagrama de Arquitetura (Mermaid.js)**

```mermaid
graph TD
    A[Usuário / Aluno] --> B(SIGAA de Bolso - Aplicativo)
    
    subgraph Módulo Offline e Interface
        B --> C[Tela de Login]
        B --> D[Visualizador de Grade e Faltas]
        D --> E[(Base Local - Mock JSON)]
    end
    
    subgraph Módulo de Inteligência Artificial
        B --> F[Interface do Chatbot]
        F --> G[API do Gemini.ia]
        G -->|Respostas em Tempo Real| F
    end
