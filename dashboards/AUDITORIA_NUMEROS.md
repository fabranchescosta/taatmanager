# Auditoria aos números do dashboard — 30/08/2026

Recalculei de forma independente todos os painéis a partir dos dados brutos
embebidos no dashboard (movimentos, faturas, custos, recibos, recorrências).

## Descoberta principal: os dados são de demonstração

O dataset embebido tem **20 movimentos bancários, 39 custos e 6 faturas**.
O sistema real tem **1 204 movimentos e 370 documentos** (docs/STATUS.md do
taat-cockpit, números do MCP de 29/08). O último movimento do snapshot é de
20/08; a última importação real foi a 29/08. Os clientes e a equipa são
representativos da estrutura real, mas os valores (saldo 2 530 €, receita
14 500 €, resultado +473 €…) **não são os números da TAAT** — são o cenário
de demonstração com que o protótipo foi construído.

## O motor calcula bem — tudo fecha ao cêntimo

| Verificação | Recalculado | Painel | |
|---|---:|---:|---|
| Saídas de agosto (banco) | −30 310,00 | −30 310,00 | ✅ |
| Entradas (brutas 18 480 − 4 000 de transferências) | +14 480,00 | +14 480,00 | ✅ |
| Saldo CCA a 20/08 | 2 530,00 | 2 530,00 | ✅ |
| Receita (FT 2026/44; a FT/47 fica fora — sem base gravada) | 14 500,00 | 14 500,00 | ✅ |
| Custos do mês (tudo menos a execução de MO imputada) | 14 027,00 | 14 027,00 | ✅ |
| Fixos = Pessoal 3 468 + renda 850 + subscrição 72 | 4 390,00 | 4 390,00 | ✅ |
| Por classificar (consumíveis de oficina) | 488,00 | 488,00 | ✅ |
| Categorias com documento | 10 559,00 | 10 559,00 | ✅ |
| Sem documento = 30 310 − 10 559 | 19 751,00 | 19 751,00 | ✅ |
| Por cobrar (totalReais c/ IVA, NC abatidas) | 30 477,00 | 30 477,00 | ✅ |
| Resultado | +473,00 | +473,00 | ✅ |

## Um ponto a verificar no motor da app

A FT 2026/44 tem `valorPendente: 9 845` gravado, mas o escalão "dentro do
prazo" mostra 13 535 € (17 835 c/ IVA − 4 300 abatidos). As duas leituras da
mesma fatura diferem ~1 426 € c/ IVA. Ou o `valorPendente` está noutra base,
ou as notas de crédito usadas pelo escalão não coincidem com o campo — a
confirmar no código do ecrã (taat-cockpit) quando se ligar aos dados reais.

## Próximo passo para ter números reais

O dashboard é um protótipo com dados embebidos; a validação dos números
verdadeiros faz-se na app: correr o mesmo ecrã sobre o Firestore (MCP
`banco_resumo`/`qualidade_dados`), com atenção especial aos itens que os
dados reais já sinalizam — 97 movimentos pendentes, prova documental a
17,11% pós-corte e os débitos diretos (água/luz, comunicações) por
identificar no extrato.
