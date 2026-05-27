# Trello Cash Flow Analyzer

Aplicação Python para análise de fluxo de caixa baseada em boards do Trello. Coleta automaticamente informações de contas a pagar e gera relatórios visuais interativos em HTML.

## Funcionalidades

- **Integração com Trello**: Conecta-se a qualquer board do Trello via API
- **Análise de Fluxo de Caixa**: Calcula saídas diárias e saldo acumulado
- **Gráficos Interativos**: Visualizações modernas com Plotly
- **Cálculo de Gastos Mensais**: Rastreia gastos do mês atual
- **Design Minimalista**: Interface limpa e intuitiva
- **Suporte Completo a Português Brasileiro**: Formatação de datas e valores em padrão BR

## Pré-requisitos

- Python 3.8+
- Uma conta no Trello com acesso a um board

## Instalação

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/trello-cash-flow-analyzer.git
cd trello-cash-flow-analyzer
```

### 2. Crie um ambiente virtual

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Instale as dependências

```bash
pip install -r requirements.txt
```

### 4. Configure as credenciais do Trello

#### Obtenha suas credenciais:

1. Acesse [https://trello.com/app-key](https://trello.com/app-key)
2. Copie sua **API Key**
3. Clique em "Token" para gerar um **Token**
4. Copie ambos

#### Configure o arquivo `.env`:

```bash
# Copie o arquivo de exemplo
cp .env.example .env

# Edite o arquivo .env com suas credenciais
nano .env  # ou use seu editor favorito
```

Seu arquivo `.env` deve conter:

```env
TRELLO_API_KEY=sua_chave_aqui
TRELLO_TOKEN=seu_token_aqui
TRELLO_BOARD_URL=https://trello.com/b/WgSarYPK/seu-board
DAYS_AHEAD=7
```

## 📖 Uso

### Uso Básico

```bash
python main.py
```

A aplicação usará as configurações do arquivo `.env`.

### Uso com Argumentos CLI

```bash
# Especificar URL do board
python main.py --board-url https://trello.com/b/WgSarYPK/seu-board

# Especificar número de dias
python main.py --days 14

# Especificar arquivo .env customizado
python main.py --env-file /path/to/.env

# Combinar múltiplos argumentos
python main.py --board-url https://trello.com/b/WgSarYPK/seu-board --days 14
```

### Argumentos Disponíveis

| Argumento | Tipo | Padrão | Descrição |
|-----------|------|--------|-----------|
| `--board-url` | string | variável de ambiente | URL do board do Trello |
| `--days` | inteiro | 7 | Número de dias à frente para análise |
| `--env-file` | string | `.env` | Caminho do arquivo .env |

## 📋 Formato dos Cards do Trello

Os cards devem seguir o formato padrão para serem parseados corretamente:

```
DD/MM/YY - R$VALOR - NOME
```

### Exemplos Válidos

- `25/11/24 - R$1.250,50 - Fornecedor ABC`
- `26/11/24 - R$500,00 - Aluguel`
- `27/11/24 - R$2.500,99 - Salários`

### Exemplos Inválidos

- `25/11/24 - Fornecedor ABC` (falta valor)
- `R$1.250,50 - Fornecedor ABC` (falta data)
- `25/11 - R$1.250,50 - Fornecedor ABC` (formato de data incorreto)

## Estrutura de Pastas

```
trello-cash-flow-analyzer/
├── main.py                    # Entry point da aplicação
├── cash_flow_analyzer.py      # Orquestrador principal
├── trello_client.py           # Cliente da API Trello
├── card_parser.py             # Parser de cards
├── report_generator.py        # Gerador de relatórios HTML
├── utils.py                   # Utilitários e helpers
├── requirements.txt           # Dependências Python
├── .env.example              # Exemplo de configuração
├── .gitignore                # Configuração do Git
├── outputs/                  # Pasta de saída dos relatórios
└── README.md                 # Este arquivo
```

## Arquitetura

A aplicação segue a arquitetura modular com separação de responsabilidades:

### `cash_flow_analyzer.py`
Orquestrador principal que coordena todo o fluxo de análise.

### `trello_client.py`
Encapsula toda a comunicação com a API do Trello:
- Autenticação
- Obtenção de listas
- Coleta de cards
- Identificação de meses

### `card_parser.py`
Responsável por fazer parsing dos títulos dos cards:
- Validação de formato
- Extração de data, valor e nome
- Tratamento de erros e validações

### `report_generator.py`
Gera os gráficos e relatórios interativos:
- Criação de figuras Plotly
- Formatação de dados para visualização
- Salvamento em HTML

### `utils.py`
Utilitários reutilizáveis:
- `BrazilianFormatter`: Formatação de valores e datas
- `MonthCalculator`: Cálculos com meses
- `URLValidator`: Validação de URLs
- Funções de logging com emojis

## O Relatório

O relatório HTML gerado inclui:

- **Gráfico de Barras**: Saídas diárias coloridas por valor
- **Linha de Saldo**: Saldo acumulado em tempo real
- **Resumo Visual**: Gastos do mês, total do período e saldo final
- **Interatividade**: Zoom, pan, hover com detalhes
- **Exportação**: Botão para exportar como PNG

## Troubleshooting

### Erro: "Arquivo .env não encontrado"
- Certifique-se de que o arquivo `.env` existe na pasta do projeto
- Use `cp .env.example .env` para criar um novo

### Erro: "Credenciais não encontradas"
- Verifique se `TRELLO_API_KEY` e `TRELLO_TOKEN` estão corretos no `.env`
- Gere novas credenciais em [https://trello.com/app-key](https://trello.com/app-key)

### Erro: "URL do board inválida"
- A URL deve estar no formato: `https://trello.com/b/BOARDID/board-name`
- Você pode copiar direto do navegador

### Nenhum card encontrado
- Verifique se o board tem listas com nomes de meses (ex: "Outubro/25")
- Verifique se os cards seguem o formato correto: `DD/MM/YY - R$VALOR - NOME`

## 🛠️ Clean Code Principles

O código segue as melhores práticas:

- **SRP** (Single Responsibility Principle): Cada classe tem uma responsabilidade
- **DRY** (Don't Repeat Yourself): Código reutilizável em utilitários
- **SOLID**: Princípios SOLID aplicados na arquitetura
- **Type Hints**: Anotações de tipo em todas as funções
- **Docstrings**: Documentação completa das funções
- **Nomes Significativos**: Nomes claros e descritivos
- **Tratamento de Erros**: Exceções customizadas e tratamento apropriado

## Logging

A aplicação usa um sistema de logging com emojis para melhor visualização:

- ✅ `log_success()`: Operações bem-sucedidas
- ⚠️ `log_warning()`: Avisos
- ❌ `log_error()`: Erros
- ℹ️ `log_info()`: Informações gerais

## Contribuindo

1. Faça um Fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está licenciado sob a MIT License - veja o arquivo LICENSE para detalhes.

## 👤 Autor

Rafael Fernandes Loureiro Pereira 
GitHub: [@rafaeloureiro](https://github.com/rafaeloureiro)

## Roadmap

- [ ] Suporte a Streamlit para interface web
- [ ] Gráficos de previsão de caixa
- [ ] Integração com outras plataformas
- [ ] Dashboard em tempo real
- [ ] Testes unitários
- [ ] CI/CD com GitHub Actions
