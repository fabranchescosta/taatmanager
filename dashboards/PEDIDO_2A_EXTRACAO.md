# Pedido da 2.ª extração — para acender o que falta em Tesouraria e Financeiro

**Como usar:** igual à 1.ª vez — abrir o Claude Code no computador do
Francisco (onde o MCP `cockpit` está registado), colar o pedido abaixo, e
enviar-me o JSON resultante (ou colar aqui no chat) para eu ligar aos
dashboards.

Isto não é um pedido genérico: é exatamente o que falta para os campos que
hoje mostram "—" no Números Reais TAAT — saldo da conta, os fluxos
mensais de Tesouraria, e a receita mensal que falta ao Financeiro.

---

## Pedido para colar

> Usa o MCP cockpit (read-only) e monta um único ficheiro
> `analise-2a-extracao.json` com o seguinte, sem transformar os valores:
>
> 1. **Saldo atual da conta CCA** — se `banco_resumo` tiver um campo de
>    saldo que a 1.ª extração não trouxe, usa-o; caso contrário, diz-me que
>    tool/parâmetro dá o saldo atual (ou o saldo de fecho do último mês
>    completo), porque a 1.ª extração só trouxe contagens de movimentos,
>    não montantes nem saldo.
> 2. **Fluxos mensais do banco, últimos 6 meses** — entradas e saídas em
>    euros por mês (não só contagem de movimentos), e o saldo de fecho no
>    fim de cada mês. Se existir uma tool tipo `banco_fluxo_mensal` ou
>    equivalente, usa-a; se não existir, o que der para juntar meses a
>    partir dos movimentos com montante serve.
> 3. **Receita mensal** — faturas emitidas por mês (valor, não só
>    `faturas_em_aberto` que já temos) dos últimos 6 meses, para a Margem
>    Bruta e a linha "Vendas" do Mapa de Resultados.
> 4. **`custos_por_snc` com quebra mensal**, se ainda não veio assim na
>    1.ª extração — precisamos de Matéria-Prima, FSE, Custos Financeiros e
>    Amortizações separados (categorias SNC), não só o total por tipo
>    interno.
>
> Acrescenta uma chave `meta` com a data/hora desta extração. Não incluas
> dados de `rhPrivado`.

---

## O que isto acende

| Onde | Campo | Precisa de |
|---|---|---|
| Tesouraria | Saldo Conta | 1 |
| Tesouraria | Previsão Saldo | 1 + o que já temos (próx. entradas/saídas) |
| Tesouraria | Entradas / Saídas (totais + vs. média 3 meses) | 2 |
| Tesouraria | Entradas e saídas, mês a mês (+ saldo de fecho) | 2 |
| Financeiro | Margem bruta, Break even | 3 |
| Financeiro | Mapa de Resultados (Vendas, Matéria-Prima, FSE, Custos Financeiros, Amortizações) | 3 + 4 |

## O que isto NÃO acende (é outra coisa, não dados)

- **Custos fixos / Custos variáveis / Custo por tipo** — não é a 2.ª
  extração que falta, é a taxonomia nova (Rendas, Comunicações, Trabalhos
  especializados, Água & Eletricidade, Pessoal Interno/Externo) ainda por
  criar na app — ver `CRITERIO_FIXO_VARIAVEL.md`. Sem isso, `custos_por_snc`
  continua a vir na classificação antiga.
- **Repartição do "sem documento"** — precisa dos 982 movimentos
  classificados na Inbox/Conciliar da app, não de uma extração adicional.

## Ordem sugerida

Como são coisas independentes, dá para andar em paralelo: pedes a 2.ª
extração acima quando puderes, e sempre que a taxonomia nova ou a
classificação do sem-documento avançarem na app, dizes-me e eu ligo isso
também — não é preciso esperar que tudo esteja pronto de uma vez.
