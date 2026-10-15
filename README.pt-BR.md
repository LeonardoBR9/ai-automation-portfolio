# Portfólio de Automação com IA

[English](README.md) | **Português**

## Perfil

**Leonardo Lima Chaves**

Especialidades: automação com IA, agentes de IA conversacionais, n8n, integrações de mensageria/WhatsApp, RAG e operação de automações.

## Resumo

Este repositório reúne exemplos pequenos e didáticos de automação com IA construídos em n8n: agentes conversacionais, respostas com recuperação de contexto (RAG), roteamento de mensagens, lógica de agendamento e lembretes. Os exemplos são genéricos de propósito e usam dados fictícios. Eles ilustram ideias, não sistemas de produção.

## O que este portfólio demonstra

- Um agente de IA no n8n com modelo de chat, memória curta de conversa e uma base de conhecimento simples em memória (RAG).
- Recebimento de mensagens por um webhook genérico, normalização e resposta via HTTP.
- Roteamento de mensagens por assunto e ramificação a partir de uma verificação de disponibilidade simulada.
- Um ramo agendado que gera lembretes.

## Stack e conceitos

- **Usados nestes exemplos:** n8n (apenas nós nativos), AI Agent, modelos de chat e embeddings da OpenAI, memória de janela, vector store em memória, Webhook, Schedule Trigger, Switch, IF, Set, Wait, HTTP Request.
- **Conceitos:** agentes conversacionais, RAG, normalização de mensagens, roteamento de intenção, agendamento, segurança operacional de automações.
- **Contexto:** versões de produção de fluxos como estes usam gateways de mensageria (por exemplo Evolution API ou WAHA), Postgres/Supabase com pgvector, Redis e Chatwoot, entre outros. Nada disso está incluído aqui.

## Exemplos

### 01 — Agente de suporte com RAG simples

Objetivo: responder perguntas de clientes com uma base de conhecimento pequena e memória de conversa.
Fluxo: webhook → normalização → pausa curta → agente de IA (modelo + memória + ferramenta de vector store) → resposta.
Veja: [`workflows/01-support-agent-example.json`](workflows/01-support-agent-example.json) · [`workflows/01-support-agent-example.md`](workflows/01-support-agent-example.md)

### 02 — Matrícula e agendamento de visita

Objetivo: rotear mensagens, coletar dados de matrícula, checar um horário simulado e confirmar, mais um ramo de lembretes.
Fluxo: webhook → normalização → switch → agente de IA → verificação simulada → confirmação ou alternativas; schedule trigger → lembrete.
Veja: [`workflows/02-enrollment-scheduling-example.json`](workflows/02-enrollment-scheduling-example.json) · [`workflows/02-enrollment-scheduling-example.md`](workflows/02-enrollment-scheduling-example.md)

> Os arquivos `.md` dos exemplos estão em inglês.

## Como importar e usar com segurança

1. No n8n, escolha **Workflows → Import from file** e selecione um `.json` de `workflows/`.
2. Adicione suas próprias credenciais (por exemplo, sua chave da OpenAI). Nenhuma credencial ou segredo está incluído.
3. Mantenha os workflows inativos até revisá-los; teste com a URL de teste do webhook.
4. Troque todas as URLs de exemplo e nunca exponha webhooks de teste publicamente sem autenticação.

## Versões completas

Versões completas disponíveis sob solicitação.

## Direitos

Todos os direitos reservados. Apenas para visualização de portfólio. Nenhuma licença é concedida para reutilização, redistribuição, uso comercial ou obras derivadas.

## Contato

- E-mail: contato@proll.ia.br
- LinkedIn: https://www.linkedin.com/in/leonardolc99
- Site: https://proll.ia.br
- GitHub: https://github.com/LeonardoBR9
