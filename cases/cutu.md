# Estudo de caso: Cutu, uma IA que participa de grupos de WhatsApp

## Resumo
- **Papel:** criador e único desenvolvedor
- **Tipo:** produto próprio (código fechado)
- **Período:** 2026
- **Stack:** Node.js, PostgreSQL com pgvector, Next.js, vários provedores de LLM
- **Impacto:** cerca de US$ 0,15 por mil mensagens num grupo bem ativo, depois de reduzir o custo por chamada em cerca de 4x

---

## Visão geral
O Cutu entra num grupo de WhatsApp como mais um participante: responde quando é chamado, comenta quando tem algo a dizer, lembra de conversas antigas e confere boatos. O desafio de engenharia mais interessante não é fazer a IA falar, e sim fazê-la saber **quando ficar quieta**, com custo previsível.

## Problema
- **Bot que responde a tudo cansa o grupo** em um dia.
- **Custo cresce com cada mensagem:** num grupo ativo, cada chamada de modelo pesa no preço do produto.
- **Provedores falham:** queda ou resposta inválida de um provedor não pode derrubar o grupo.

## Processo

### Abordagem
Separei o trabalho em papéis, cada um com o modelo adequado ao que exige: um papel que lê todo lote de mensagens (o mais barato, porque roda sempre), um que decide se vale entrar na conversa e um que escreve o texto final. Assim dá para trocar o modelo de um papel sem mexer nos outros e medir o custo de cada etapa.

### Decisões principais
- **Silêncio como resultado válido.** Ficar calado é um desfecho de primeira classe, com custo zero no papel que escreve. Há cota de interações, horário silencioso e ritmo configurável.
- **Determinístico antes do generativo.** Perguntas como "quando a gente falou de viagem?" são resolvidas por busca na memória, sem chamar modelo.
- **Trocar modelo só com medição.** Antes de baratear um papel, a troca passa por avaliação com juiz cego. A parte aberta dessa avaliação está em [llm-roles-eval](https://github.com/jhuan-montoya/llm-roles-eval).

## Solução
- **Pipeline por papéis** com um modelo por papel e reserva para cada um.
- **Memória com busca** sobre o histórico do grupo.
- **Áudio e imagem:** áudio vira texto; imagem é vista só pelo papel que lê, e o arquivo não é guardado.
- **Fallback entre provedores:** falha troca de modelo e, no pior caso, volta para um texto fixo.

## Resultados

| Métrica | Valor |
|---|---|
| Custo num grupo bem ativo | ~US$ 0,15 por mil mensagens |
| Custo por chamada após trocar a ordem fixa de provedor por roteamento por preço | ~4x menor, sem trocar de modelo |
| Divisão do gasto | texto 96%, imagem gerada 3%, áudio 1% |
| Base de código | ~13 mil linhas de código e ~15,5 mil linhas de testes (71 arquivos de teste) |

## Aprendizados

### O que funcionou
- **A maior economia veio do roteamento, não do prompt.**
- **Separar papéis** tornou custo e qualidade mensuráveis por etapa.
- **Limite de tempo por teste** transformou travamentos em falhas visíveis, em vez de suíte pendurada.

### Próximo passo
- **Modelos menores nos papéis de decidir e escrever**, mas só depois de passarem pela avaliação com juiz cego.

---

*Produto próprio; código fechado.*
