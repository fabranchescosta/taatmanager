# Pedido de dados reais para o dashboard Análise Financeira

**Como usar:** abrir o Claude Code no computador do Francisco (onde o MCP
`cockpit` está registado) e colar o pedido abaixo. O resultado é um ficheiro
JSON; enviá-lo para a sessão do dashboard (ou colar aqui no chat) e eu ligo-o
ao artefacto.

---

## Pedido para colar

> Usa o MCP cockpit (read-only) e monta um único ficheiro
> `analise-dados-reais.json` com os resultados das seguintes tools, uma chave
> por tool, sem transformar os valores:
>
> 1. `banco_resumo` — visão geral do banco;
> 2. `banco_sem_documento` — as saídas por explicar;
> 3. `banco_pendentes` — os movimentos pendentes;
> 4. `banco_transferencias_orfas` — transferências não confirmadas;
> 5. `faturas_em_aberto` — o por cobrar com vencimentos;
> 6. `custos_por_snc` — custos por categoria, se possível mês a mês dos
>    últimos 10 meses;
> 7. `recorrencias_listar` — rendas, subscrições e afins;
> 8. `equipa_resumo` — cabeças e custos de pessoal;
> 9. `iva_estado` e `obrigacoes_proximas` — contexto fiscal;
> 10. `health` e `auditoria_integridade` — para sabermos a qualidade da base.
>
> Acrescenta uma chave `meta` com a data/hora da extração e o mês mais
> recente com extrato completo. Não incluas dados de `rhPrivado` nem nada
> fora do que as tools devolvem.

---

## O que acontece a seguir

1. Eu comparo o JSON com o cenário de demonstração e substituo os dados do
   dashboard pelos reais (via ficheiro de dados, para as próximas
   extrações serem só substituir o ficheiro).
2. Primeiro teste com dados reais: o "Por cobrar" — a auditoria encontrou
   uma inconsistência no motor (`valorPendente` vs. escalões, ver
   AUDITORIA_NUMEROS.md) que só se confirma com faturas verdadeiras.
3. Segundo teste: os 19 751 € "sem documento" do demo passam a ser o valor
   real de `banco_sem_documento` — e aí começa a validação a sério (débitos
   diretos de água/luz e comunicações incluídos).

## Alternativa sem MCP

Se for mais fácil, qualquer export estruturado serve (CSV do extrato +
lista de faturas + custos classificados). O contrato de dados que o
dashboard consome está documentado pela auditoria; eu faço a adaptação.
