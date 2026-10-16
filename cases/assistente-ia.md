# Estudo de caso: assistente de atendimento por IA

## Resumo
- **Papel:** criei o MVP, conduzi as decisões técnicas por um período depois dele e participei da migração para a plataforma oficial da empresa
- **Empresa:** Méliuz
- **Período:** 2026
- **Time:** MVP comigo à frente; migração com um time de três engenheiros
- **Impacto:** primeiro assistente de IA da empresa, com lançamento gradual de 10% a 100% do app e tempo mediano de resposta reduzido de ~20 s para ~8 s

---

## Visão geral
O atendimento começava num formulário de chamado. Construí o primeiro assistente de atendimento por IA do Méliuz, que troca esse formulário por uma conversa: o assistente consulta serviços como conta e saldo e resolve o caso ou, quando não dá, abre um ticket já qualificado.

## Problema
- **Para o cliente:** abrir chamado e esperar, mesmo quando a resposta já existia.
- **Para o atendimento:** chamados chegando sem contexto, o que obrigava a perguntar tudo de novo.
- **Restrição:** um assistente que erra em escala é pior do que um formulário. A qualidade precisava ser medida antes de cada ampliação do público.

## Processo

### Abordagem
Comecei pelo menor produto que resolvesse casos reais e validasse a ideia. A partir dele, a ampliação foi gradual (10% da base, depois 100% do app), cada passo condicionado a sinais de qualidade.

### Decisões principais
- **Medir antes de ampliar.** A avaliação automática, a revisão humana e os testes de regressão entraram cedo, antes de o público crescer.
- **Simplificar em vez de remendar.** Quando o desenho inicial começou a pesar na manutenção, troquei-o por uma arquitetura com especialistas, mais simples de manter. A mudança removeu cerca de 4.200 linhas de código.
- **Contrato antes do código na migração.** Escrevi a ADR, o plano de migração e o contrato de API do novo microsserviço, o que deixou os times de front-end e back-end avançarem em paralelo.

## Solução
- **Conversa no lugar do formulário:** o assistente entende o caso, consulta os serviços necessários e resolve ou qualifica o ticket.
- **Qualidade contínua:** avaliação automática das respostas (LLM-as-judge), fila de revisão humana e suíte de regressão.
- **Plataforma oficial:** microsserviço na infraestrutura da empresa, com mais de 5 mil testes automatizados e cerca de 97% de cobertura.
- **Operável sem deploy:** painel de configuração no back-office com versionamento e rollback.

## Resultados

| Métrica | Antes | Depois |
|---|---|---|
| Público com acesso ao assistente | 0% | 100% do app (lançamento gradual a partir de 10%) |
| Tempo mediano de resposta | ~20 s | ~8 s |
| Código do agente | — | ~4.200 linhas a menos após a simplificação |
| Testes do serviço | — | 5 mil+ testes, ~97% de cobertura |

## Aprendizados

### O que funcionou
- **Lançamento gradual com medição** deu segurança para ampliar sem surpresas.
- **Escopo estreito por especialista** foi mais eficaz do que acumular regras num prompt só.
- **Contrato de API antes do código** destravou trabalho paralelo na migração.

### O que eu faria diferente
- **Garantias fora do modelo desde o início.** Um LLM pode afirmar que fez uma ação que não fez; esse tipo de verificação precisa estar em código desde a primeira versão, não chegar depois de aparecer o problema.

---

*Código e arquitetura proprietários do Méliuz. Este texto descreve só o meu papel e lições gerais, sem detalhes internos.*
