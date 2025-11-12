# Introdução e Storytelling

## Contexto
O varejo opera com milhares de SKUs em dezenas de lojas. O principal dilema é equilibrar **ruptura** (perda de venda e imagem) versus **excesso de estoque** (capital imobilizado e custo de armazenagem). 
Utilizaremos os **dados reais do M5 (Walmart)**: vendas diárias por SKU/loja, calendário com feriados/eventos e histórico de preços.

## Problema de negócio
Como **prever a demanda dos próximos 28 dias** por SKU/loja e, a partir disso, **definir políticas de estoque** que reduzam rupturas e minimizem custo?

## Dados utilizados
- `sales_train_validation.csv`: vendas diárias (30k+ SKUs)  
- `calendar.csv`: datas, feriados, eventos e identificadores de semana  
- `sell_prices.csv`: preço por SKU/loja/semana  
*(Fonte: Kaggle – M5 Forecasting – Accuracy)*

## Objetivo do projeto
1) Construir um **modelo de previsão T+28** por SKU/loja (XGBoost/LightGBM).  
2) Traduzir a previsão em **política de estoque**:
   - **Safety Stock (SS)** para nível de serviço desejado;  
   - **EOQ** (Economic Order Quantity) para tamanho de lote;  
   - Sinalizar **risco de ruptura** e impacto financeiro.

## Hipóteses principais
- H1: Preço e eventos (feriados/promoções) **explicam parte relevante** da variação da demanda.  
- H2: Lags (7, 28, 56) e médias móveis (7, 28, 90) **capturam sazonalidade semanal/mensal**.  
- H3: Com previsão + SS/EOQ é possível **reduzir ruptura** mantendo custo sob controle.

## Métricas
- **RMSE** e **MAPE** para acurácia de previsão.  
- (Opcional) **WRMSSE** do M5 (métrica hierárquica).  
- Indicadores de estoque: **Fill Rate**, % de **stock-out** evitado, **custo total** (pedido + manter).

## Fórmulas-chave
- **EOQ**: \\( \text{EOQ} = \sqrt{\frac{2 \cdot D \cdot K}{h}} \\)  
  onde *D* = demanda anual, *K* = custo por pedido, *h* = custo de manter estoque (ano).  
- **Safety Stock (SS)**: \\( \text{SS} = z \cdot \sigma_{dL} \\)  
  onde *z* = nível de serviço (ex.: 1,64 ≈ 95%), *σ₍dL₎* = desvio da demanda no lead time.

## Escopo e Entregáveis
- **EDA** (melt, merges, features de calendário/preço)  
- **Feature Engineering** (lags/rollings/price change)  
- **Model selection** (XGB vs LGBM)  
- **Forecast T+28** por SKU/loja  
- **Política de estoque** (SS/EOQ) e **relatório executivo**  
- (Opcional) **Dashboard Streamlit** + **WRMSSE** + **tuning com Optuna**

## Impacto esperado
- ↓ **Ruptura** e multas por indisponibilidade  
- ↓ **Capital parado** (estoque mais enxuto)  
- ↑ **Precisão do planejamento** e negociação com fornecedores

## Riscos e limitações
- Ruído por mudanças táticas (promoções não mapeadas, rupturas passadas)  
- Dados de preço semanais (sell_price) → necessidade de join via `wm_yr_wk`  
- **Cold start** para SKUs novos

## Cronograma (macro)
1) EDA/Preparação ✦ 2) Features ✦ 3) Modelagem ✦ 4) Forecast ✦ 5) Estoque ✦ 6) Relatório

> Projeto desenvolvido como **Trabalho Final EBAC (parceria Sermantix)**, com foco em aplicação prática de Data Science em Supply Chain.
🧾 Versão curta (para slide 1 de abertura)
markdown
Copiar código
**Contexto**  
Ruptura vs. excesso de estoque no varejo. Dados reais do M5 (Walmart): vendas diárias, calendário e preços.

**Objetivo**  
Prever T+28 por SKU/loja e converter em política de estoque (SS/EOQ) para reduzir ruptura com menor custo.

**Métricas**  
RMSE, MAPE (e WRMSSE opcional) + Fill Rate/stock-out evitado.

**Entregáveis**  
EDA → Features → Modelo (XGB/LGBM) → Forecast → SS/EOQ → Relatório executivo / Dashboard.
