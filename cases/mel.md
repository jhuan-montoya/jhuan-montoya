# Mel: agente de atendimento por IA em produção

Resumo do meu trabalho na Mel, o agente de atendimento por IA da Central de Ajuda do Méliuz. Criei a Mel do zero, levei do MVP ao lançamento gradual para a base de usuários e, depois, liderei a reconstrução dela como microsserviço na plataforma oficial da empresa.

## O problema

Trocar o formulário de chamado por uma conversa que resolve o caso ou, quando não dá, abre o ticket já qualificado, com o contexto de que o atendimento precisa.

## O que eu fiz

- **Do MVP à produção:** desenho, implementação e lançamento gradual do agente, com acompanhamento de qualidade a cada etapa.
- **Arquitetura multiagente:** evoluí de um fluxo fixo para uma arquitetura com especialistas, mais simples de manter. A refatoração removeu cerca de 4.200 linhas de código.
- **Qualidade de IA:** avaliação automática (LLM-as-judge), revisão humana e testes de regressão antes de cada mudança de comportamento.
- **Migração para a plataforma da empresa:** escrevi a ADR, o plano de migração e o contrato de API, e conduzi a execução até produção, com mais de 3.300 testes automatizados e cerca de 98% de cobertura.
- **Entrega ponta a ponta:** mais de 150 PRs em cinco meses, entre serviços, BFFs, front-end e infraestrutura.

## O que aprendi (lições gerais de agentes em produção)

- **Ação do agente se verifica em código, não se confia ao modelo.** Um LLM pode afirmar que fez algo que não fez; a garantia precisa estar fora do prompt.
- **Prompt maior não corrige comportamento.** Escopo estreito e medição por parte funcionam melhor do que acumular regras num prompt só.
- **Avaliação automática precisa de humano no loop.** O juiz automático escala a análise, e a revisão humana mantém o juiz honesto.
- **Contrato de API antes do código** deixa times diferentes avançarem em paralelo numa migração.

---

*Código e arquitetura proprietários do Méliuz. Este texto descreve só o meu papel e lições gerais, sem detalhes internos.*
