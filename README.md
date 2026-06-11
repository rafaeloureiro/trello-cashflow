# 📊 Trello Cash Flow Analyzer

Dashboard de fluxo de caixa integrado ao Trello, construído com Streamlit. Lê os cards de um board de contas a pagar, processa os lançamentos e exibe métricas, gráficos e comparativos ano a ano.

---

## Funcionalidades

- **KPIs com comparativo YoY** — gasto acumulado no mês (dia 1 → hoje) e total do período selecionado, ambos com delta percentual em relação ao mesmo período do ano anterior
- **Seletor de período dinâmico** — qualquer intervalo de datas, até 2 anos atrás, via sidebar
- **Gráfico de fluxo diário** — barras de saídas por dia + linha de saldo acumulado em eixo secundário
- **Gráfico de fornecedores** — top 12 categorias/fornecedores do período em barras horizontais
- **Tabela detalhada** — cards do período com data formatada (DD/MM/AAAA) e valor em padrão BR
- **Cards inválidos** — seção colapsável listando cards que não seguem o padrão esperado, com motivo do erro
- **Cache de 10 minutos** — evita chamadas desnecessárias à API do Trello a cada rerun
- **Fuso horário Brasil** — "hoje" calculado em `America/Sao_Paulo`, não UTC

---

## Estrutura do Board no Trello

O app espera um board com listas nomeadas no padrão:

```
mês / AA
```

Exemplos válidos: `junho / 26`, `janeiro / 25`, `março/26`

Cada card dentro dessas listas deve seguir o padrão:

```
DD/MM/AA - R$ valor - descrição
```

Exemplos válidos:
```
10/06/26 - R$ 1.250,00 - Fornecedor X
05/06/26 - R$ 750,50 - Aluguel
```

Cards fora desse padrão são ignorados nos cálculos e listados na seção **"Cards com formato inválido"** ao final da página.

---

## Configuração

### 1. Credenciais do Trello

Obtenha sua API Key e Token em [https://trello.com/power-ups/admin](https://trello.com/power-ups/admin).

**Streamlit Cloud:** adicione nos Secrets do app:

```toml
# .streamlit/secrets.toml
TRELLO_API_KEY = "sua_api_key"
TRELLO_TOKEN   = "seu_token"
```

**Local:** crie o arquivo `.streamlit/secrets.toml` com o mesmo conteúdo acima.

### 2. URL do Board

No arquivo principal, ajuste a constante `BOARD_URL` para a URL do seu board:

```python
BOARD_URL = "https://trello.com/b/SEU_BOARD_ID/nome-do-board"
```

---

## Instalação local

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/trello-cashflow.git
cd trello-cashflow

# Instale as dependências
pip install -r requirements.txt

# Configure os secrets
mkdir -p .streamlit
cp .streamlit/secrets.toml.example .streamlit/secrets.toml
# Edite o arquivo com suas credenciais

# Rode o app
streamlit run trello-cashflow-v2.py
```

---

## Dependências

| Pacote | Versão mínima | Uso |
|--------|---------------|-----|
| streamlit | 1.32.0 | Framework do app |
| requests | 2.31.0 | Chamadas à API do Trello |
| pandas | 2.0.0 | Processamento dos dados |
| plotly | 5.18.0 | Gráficos interativos |
| pytz | 2024.1 | Fuso horário Brasil |
| python-dotenv | 1.0.0 | Variáveis de ambiente (local) |

---

## Como o comparativo YoY funciona

O app busca automaticamente as listas do mesmo intervalo do ano anterior no mesmo board. Se existirem listas como `junho / 25` quando o período selecionado for `junho / 26`, o delta percentual é calculado e exibido abaixo de cada KPI.

- 🔴 **Vermelho** = gasto aumentou em relação ao ano anterior
- 🟢 **Verde** = gasto reduziu em relação ao ano anterior

Se não houver dados do ano anterior, o delta simplesmente não é exibido.

---

## Deploy no Streamlit Cloud

1. Faça push do repositório para o GitHub
2. Acesse [share.streamlit.io](https://share.streamlit.io) e conecte o repositório
3. Defina o arquivo principal como `trello-cashflow-v2.py`
4. Adicione `TRELLO_API_KEY` e `TRELLO_TOKEN` nos **Secrets** do app
5. Deploy

> O cache de 10 minutos é renovado automaticamente. Para forçar atualização dos dados, pressione **F5** — o Streamlit invalida o cache e rebusca os cards do Trello.

---

## Roadmap

- [ ] Filtro de exclusão de categorias via sidebar (ex: retiradas de lucro)
- [ ] Suporte a múltiplos boards (multi-marca)
- [ ] Exportação para Excel
- [ ] Comparativo mês a mês no mesmo gráfico
