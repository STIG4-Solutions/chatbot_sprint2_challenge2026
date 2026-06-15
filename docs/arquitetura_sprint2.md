# Arquitetura — ChargeGrid Intelligence Sprint 2

## Fluxo Completo de Uma Interação

![Fluxo completo de uma interação — ChargeGrid Intelligence](arquitetura_sprint2.png)

## Componentes

| Componente | Tecnologia | Função |
|---|---|---|
| Interface | Python CLI / Colab | Entrada e saída do operador |
| System Prompt | LangChain SystemMessage | Define papel, escopo permitido e proibido, formato de saída |
| Few-shot Examples | LangChain HumanMessage / AIMessage | 2 exemplos validados Sprint 1 para calibrar tom e estrutura |
| Memória | Lista Python (`historico`) | Mantém todos os turnos da sessão para diálogo contínuo |
| RAG | FAISS + OpenAI Embeddings | Recupera trechos técnicos relevantes da base de conhecimento |
| Base de Conhecimento | 6 documentos técnicos | OCPP, MODBUS, ANEEL, DSM, Tarifação, Interoperabilidade |
| LLM | gpt-4o-mini (OpenAI) | Geração da resposta fundamentada |

## Fluxo da Memória de Conversa

A cada turno, o histórico cresce e é reenviado integralmente para o modelo:

```
Turno 1  →  [System] [Few-shot x2] [User1] [Asst1]
Turno 2  →  [System] [Few-shot x2] [User1] [Asst1] [User2] [Asst2]
Turno 3  →  [System] [Few-shot x2] [User1] [Asst1] [User2] [Asst2] [User3] [Asst3]
```

Isso garante que o modelo mantenha o contexto completo da conversa sem perder referências a mensagens anteriores.
