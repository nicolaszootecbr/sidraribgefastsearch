# SIDRA Data Pipeline & Diagnostic Tools 📊🌾🥛

Um ecossistema robusto em **R** para busca interativa, diagnóstico de metadados e pipelines automáticos de ETL (Extração, Transformação e Carga), geoprocessamento e geração de relatórios estatísticos baseados nas APIs do **IBGE / SIDRA**.

---

## 🚀 Funcionalidades do Sistema

O repositório está estruturado em três módulos principais:

### 1. Ferramentas Interativas e Diagnóstico (`/interactive`)
*   **Interactive Table Search (`search_interactive.R`)**: Sistema de busca dinâmico que pesquisa tabelas do SIDRA por nomes ou categorias usando filtros fonéticos e semânticos robustos (ex: café, milho, gado, leite). Permite testar requisições e baixar dados via pacote `sidrar` de forma assistida.
*   **Localities Explorer (`location_explorer.R`)**: Navegador interativo conectado diretamente à API de Localidades do IBGE. Facilita encontrar códigos e relações hierárquicas de UFs, Mesorregiões, Microrregiões, Municípios e Distritos, exportando as seleções em CSV, RDS ou JSON.
*   **Database Diagnostics (`catalog_diagnostics.R`)**: Analisador de metadados que carrega catálogos estruturados do SIDRA para cruzamento de variáveis, identificando coberturas de tabelas e gerando relatórios de diagnóstico em formato HTML.

### 2. Pipelines de Produção (`/pipelines`)
Processadores de dados ponta a ponta projetados para lidar com oscilações de rede (mecanismos de *fallback* e *retry*), codificações corrompidas de texto de APIs e padronizações ultra-resistentes:
*   **Análise da Cadeia Leiteira da Bahia (`/bahia_dairy_chain`)**: Consolida séries temporais (2015-2024) das tabelas 74, 94 e 3939 da PPM, tabela 1086 (PTL) e Censo Agropecuário 2017 (tabela 9130). Produz gráficos de tendência, mapas coropléticos refinados sob o **CRS projetado EPSG:5880 (SIRGAS 2000 Polyconic)** e relatórios técnicos autoauditados (*"Prova dos Nove"*).
*   **Produção de Grãos no Paraná (`/parana_grains`)**: Extrai dados históricos de área, rendimento e produção de Milho e Soja (1991-2020) da tabela 1612 (PAM) por *chunks* temporais. Constrói mapas de tendências municipais e mapas comparativos em círculos proporcionais (1991 vs 2020) com controle espacial de sobreposição (`ggrepel`).
*   **Produção de Mel no Paraná (`/parana_honey`)**: Avalia a apicultura municipal (tabela 6935) com mapas coropléticos, resumos estatísticos e visualizações de rankings de valor bruto.

### 3. Prompt Engineering (`/prompt_generator`)
*   **Prompt Generator v5.0 (`prompt_generator_v5.R`)**: Ferramenta interativa que coleta parâmetros de projetos de forma guiada no console e gera prompts estruturados (Markdown e JSON). Esse prompt serve como contrato de especificação técnica rigoroso para que LLMs (como Claude, Gemini ou GPT) possam gerar códigos R de produção que sigam exatamente o padrão de qualidade e robustez dos pipelines deste repositório.

---

## 📁 Estrutura Física das Saídas dos Pipelines

Cada script de análise gerará de forma automática e organizada a seguinte estrutura local no diretório definido de saída:
*   `dados/`: Arquivos CSV contendo dados brutos limpos, agregados históricos, tabelas de rankings locais e indicadores consolidados.
*   `graficos/`: Gráficos PNG de altíssima definição (300 DPI) contendo linhas de tendências e análises de distribuição (como produtividade × preço).
*   `mapas/`: Mapas espaciais complexos em formato PNG, utilizando projeções cartográficas adequadas para evitar distorções de escala, incluindo rosas dos ventos e barras de escala métricas.
*   `analise/`: Relatórios técnicos completos em texto estruturado (`.txt`), ideais para subsídios rápidos de tomada de decisão.

---

## 🛠️ Pré-requisitos e Dependências

Para rodar as ferramentas deste repositório, você precisará do **R (versão >= 4.2)** e do compilador GIS local (em distribuições Linux, certifique-se de ter `libgdal-dev` instalado para suporte a manipulações espaciais).

Instale as dependências executando o comando abaixo no console do seu R:

```R
pacotes <- c("sidrar", "tidyverse", "janitor", "lubridate", "scales", "sf", "ggspatial", "ggrepel", "httr", "jsonlite", "stringi", "cli")
install.packages(pacotes, dependencies = TRUE)
```

---

## 🚦 Como Utilizar os Scripts de Análise

1.  **Prepare as Malhas Geográficas**: Coloque os arquivos shapefiles oficiais (em formato `.zip` obtidos no FTP do IBGE) no diretório indicado pelas variáveis globais de entrada de cada script (ou utilize o fallback automático por meio do download direto das APIs).
2.  **Ajuste o Arquivo de Configuração**: Renomeie `config.example.yml` para `config.yml` e defina os caminhos locais para onde as saídas de dados e relatórios devem ser salvas.
3.  **Execute o script**:
    ```R
    source("pipelines/bahia_dairy_chain/bahia_dairy_chain.R")
    ```

---

## 📄 Licença

Este projeto está licenciado sob a licença MIT - consulte o arquivo [LICENSE](LICENSE) para mais detalhes.
