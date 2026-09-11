# Relatório de Funcionalidades e Testes de Validação (MVP)
**Projeto:** SIGAA de Bolso Mobile  
**Instituição:** Colégio Técnico de Bom Jesus (CTBJ / UFPI)  
**Disciplina:** Projeto Integrador II  

---

## 1. Mapeamento de Funcionalidades do Sistema

O **SIGAA de Bolso** foi estruturado para resolver a sobrecarga de informações e a falta de responsividade em dispositivos móveis do SIGAA tradicional, oferecendo:

* **Gestão Multi-Cadastro (Isolamento por Matrícula):** Permite que múltiplos discentes utilizem o mesmo dispositivo. Cada login baseado na matrícula cria ou recupera o estado exclusivo daquele aluno no `localStorage`.
* **Perfil Institucional e Controle de Dados:** Exibição dos dados do discente (Nome, Matrícula, Curso e Status) com opção de reset local de dados limitado à conta ativa.
* **Termômetro de Faltas Interativo:** Cálculo dinâmico da porcentagem de ausências em relação ao limite (15 faltas). Exibe barra visual de progresso com alertas de cor (verde para situação regular, amarelo para alerta e vermelho para risco de reprovação).
* **Quadro de Turmas e Detalhes da Disciplina:** Visualização simplificada das matérias matriculadas com abertura de modal interno contendo atividades pendentes/entregues e boletim de notas parciais (N1, N2 e Média).
* **Grade de Horários Integrada:** Tabela adaptada para telas móveis com os horários de aula do Módulo V (período vespertino).
* **Assistente Virtual Mestre JM (LLM + Fallback):** Chatbot inteligente com integração à API da Groq (`llama-3.3-70b-versatile` e modelos em cascata). Em caso de ausência de chave ou falha de conexão, aciona o mecanismo de *Fallback* local sem interrupção da experiência do usuário.
* **Sistema de Notificações Internas (Toasts):** Feedback visual para ações executadas (registro de falta, cadastro de atividades, entrada/saída do sistema).

---

## 2. Casos de Teste e Validação do MVP

Os testes foram executados simulando cenários de uso real no ambiente móvel:

| ID | Funcionalidade Testada | Procedimento do Teste | Resultado Esperado | Status |
| :--- | :--- | :--- | :--- | :---: |
| **CT01** | **Autenticação Multi-Aluno** | Informar a matrícula `202410112`, adicionar faltas, sair e logar com a matrícula `202410113`. | O sistema deve criar um perfil limpo para o segundo aluno e manter as faltas do primeiro salvas de forma isolada ao retornar. | **APROVADO** |
| **CT02** | **Incremento no Termômetro** | Clicar no botão `+1 Falta` na disciplina de Segurança da Informação. | A contagem deve subir, a barra de progresso deve avançar visualmente e exibir a toast alertando o incremento. | **APROVADO** |
| **CT03** | **Persistência de Dados** | Adicionar faltas, fechar o navegador e reabrir o sistema no mesmo dispositivo. | As faltas e os dados do aluno devem ser restaurados automaticamente via `localStorage`. | **APROVADO** |
| **CT04** | **Modal de Turmas e Notas** | Toque em uma turma da lista (ex: *Banco de Dados*). | Abrir modal com dados da matéria, permitindo alternar entre as abas *Atividades* e *Notas*. | **APROVADO** |
| **CT05** | **Chatbot com API Groq** | Enviar mensagem *"Quantas faltas eu tenho em Segurança da Informação?"* com chave válida. | O Mestre JM processa a requisição via API e responde com base no contexto do aluno logado. | **APROVADO** |
| **CT06** | **Chatbot Fallback Offline** | Desconectar a internet ou omitir a API Key e enviar *"Como funciona a grade de horários?"*. | O assistente utiliza o algoritmo de busca por palavras-chave local e responde sem gerar erro em tela. | **APROVADO** |
| **CT07** | **Reset de Armazenamento** | Acessar a aba *Perfil* e clicar em *Resetar Faltas deste Aluno*. | Excluir apenas os dados vinculados à matrícula atual do `localStorage` e retornar à tela inicial. | **APROVADO** |

---

## 3. Resumo da Cobertura de Testes

* **Total de Casos de Teste Executados:** 7
* **Casos de Teste Aprovados:** 7 (100%)
* **Ambiente de Testes:** Chrome DevTools (Mobile View Emulator / iPhone 12 Pro) e Android Chrome Browser.
* **Estado do Código:** Estável e pronto para hospedagem pública no GitHub Pages.
