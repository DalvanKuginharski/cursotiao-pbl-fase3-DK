# FIAP - Faculdade de Informática e Administração Paulista

<p align="center">
<img width="1364" height="728" alt="image" src="https://github.com/user-attachments/assets/6bf817aa-098b-4561-8941-235371c12065" />

<br>

# FarmTech Solutions — Banco de Dados com Oracle

## FarmTech Solutions - PBL

## 👨‍🎓 Integrante:
- Dalvan Kuginharski
---

## 📜 Descrição

O PBL (Project-Based Learning) do Curso de Inteligência Artificial da FIAP simula a trajetória de crescimento de uma startup. A empresa fictícia FarmTech Solutions atua como consultoria em soluções tecnológicas para o agronegócio — setor apontado como um dos mais promissores para aplicação de IA no Brasil, segundo o Global AI Jobs Barometer da PwC (2025).

Nesta Fase 3, os dados coletados pelos sensores da Fase 2 são carregados em um banco de dados relacional Oracle. Os sensores simulados via código C/C++ (Arduino/ESP32) registram, a cada 5 minutos, variáveis críticas do solo e da irrigação: umidade, pH, presença de nutrientes (N, P, K), probabilidade de chuva, volume pluviométrico e estado da bomba d'água.

O arquivo de dados foi nomeado como FarmtechsolutionsPBL e carregado em uma instância Oracle (oracle.fiap.com.br). A tabela sensor_readings foi criada via DDL, e a importação automatizada foi realizada por script Python utilizando a biblioteca oracledb. Ao todo, 48 registros foram importados, correspondendo a leituras coletadas em 14/04/2026 das 07:00 às 10:55.

Após a importação, foram executadas 10 consultas SQL analíticas no Oracle, abrangendo estatísticas descritivas, análise de acionamentos da bomba, detecção de leituras críticas (baixa umidade + ausência de nutrientes), leituras com pH fora da faixa ideal e visão agregada por hora. Os resultados demonstram que a bomba foi acionada em 45,8% das leituras, o pH médio do solo ficou em 6,04 e a umidade média foi de 50,92%.

## 🎥 Vídeo Demonstrativo
 
> Demonstração completa do projeto: conexão Oracle, importação do CSV, execução das 10 consultas SQL e geração dos gráficos Python.
 
[![Assista ao vídeo de demonstração](https://img.youtube.com/vi/SEU_VIDEO_ID/maxresdefault.jpg)
 
> 🔒 Vídeo publicado como **não listado** no YouTube.

---

## 📁 Estrutura de pastas

Dentre os arquivos e pastas presentes na raiz do projeto, definem-se:

.github: Arquivos de configuração do GitHub para gerenciar e automatizar processos no repositório.
assets: Arquivos relacionados a elementos não-estruturados, como imagens, logo da FIAP e gráficos gerados pelo script Python.
config: Arquivos de configuração usados para parâmetros e ajustes do projeto.
document: Documentos do projeto solicitados pelas atividades. Na subpasta other, documentos complementares.
scripts: Scripts auxiliares — neste projeto, o script principal de importação e análise Oracle e as consultas SQL.
src: Todo o código fonte criado ao longo das fases (inclui código C/C++ da Fase 2).
README.md: Arquivo que serve como guia e explicação geral sobre o projeto.

```
📦 cursotiao-pbl-fase3-DK/
│
├── .github/
├── assets/
│   ├── logo-fiap.png
│   ├── graficos/
│   │   ├── dashboard_geral.png
│   │   ├── grafico_umidade.png
│   │   ├── grafico_ph.png
│   │   ├── grafico_chuva.png
│   │   ├── consulta4_bomba.png
│   │   ├── consulta7_nutrientes.png
│   │   └── consulta9_por_hora.png
│   └── prints/
│       ├── 01_conexao_oracle.png
│       ├── 02_tabela_criada.png
│       └── ...
├── config/
├── document/
│   └── other/
│       └── Relatorio_FarmTech_PBL_Fase3.docx
├── scripts/
│   ├── FarmtechsolutionsPBL.csv       ← base de dados dos sensores
│   ├── farmtech_oracle.py             ← script principal Python + Oracle
│   └── consultas_oracle.sql           ← 10 consultas analíticas SQL
├── src/
│   ├── Fase 2 - Farm Tech - Irrigacao.py   ← código Fase 2
│   └── weather_fetch.py                    ← busca meteorológica API
├── .gitignore
└── README.md

```
---

## 🗃️ Estrutura da Tabela Oracle
**Base:** `FarmtechsolutionsPBL` | **Servidor:** `oracle.fiap.com.br:1521/ORCL` | **Tabela:** `sensor_readings`
 
| Coluna | Tipo Oracle | Descrição |
|---|---|---|
| `id` | NUMBER (PK, Identity) | Identificador único auto-incremento |
| `ts` | TIMESTAMP | Data/hora da leitura do sensor |
| `humidity` | NUMBER(5,2) | Umidade do solo (%) |
| `ph` | NUMBER(4,2) | pH do solo |
| `n_nutrient` | NUMBER(1) | Presença de Nitrogênio (0 = ausente / 1 = presente) |
| `p_nutrient` | NUMBER(1) | Presença de Fósforo (0 = ausente / 1 = presente) |
| `k_nutrient` | NUMBER(1) | Presença de Potássio (0 = ausente / 1 = presente) |
| `rain_chance` | NUMBER(3) | Probabilidade de chuva (%) |
| `rain_mm` | NUMBER(5,2) | Volume de chuva registrado (mm) |
| `pump_status` | VARCHAR2(3) | Estado da bomba de irrigação: ON ou OFF |
---

## 🔧 Como executar o código

### Pré-requisitos
 
- Python 3.9+
- Acesso ao Oracle FIAP (`oracle.fiap.com.br:1521/ORCL`) ou Oracle Database XE local
- Bibliotecas Python: `pip install oracledb pandas matplotlib`
### Passo 1 — Configurar credenciais
 
Abra o arquivo `scripts/farmtech_oracle.py` e edite as variáveis no topo:
 
```python
DB_USER     = "seu_rm"        # ex: rm568860
DB_PASSWORD = "sua_senha"     # senha do Oracle FIAP
DB_DSN      = "oracle.fiap.com.br:1521/ORCL"
```
 
### Passo 2 — Executar o script principal
 
```bash
cd scripts
python farmtech_oracle.py
```
 
O script executa automaticamente em 6 módulos:
 
1. **Módulo 1** — Conexão com o Oracle
2. **Módulo 2** — Criação da tabela `sensor_readings` (DDL)
3. **Módulo 3** — Importação dos 48 registros do CSV via `executemany`
4. **Módulo 4** — Execução das 10 consultas SQL analíticas
5. **Módulo 5** — Geração de 7 gráficos salvos em `assets/graficos/`
6. **Módulo 6** — Relatório final no terminal
### Passo 3 — Consultas SQL individuais
 
Para executar as consultas separadamente, abra o arquivo `scripts/consultas_oracle.sql` em qualquer cliente Oracle (SQL Developer, SQL*Plus, etc.).

---
 
## 🗄️ Evidências do Banco de Dados Oracle
 
### Conexão, tabela criada e estrutura de colunas
<img width="1365" height="729" alt="image" src="https://github.com/user-attachments/assets/0be6ce36-c420-4927-9b7a-fc73bd95e682" />

### Importação dos dados e prévia dos registros
<img width="1366" height="727" alt="image" src="https://github.com/user-attachments/assets/e1a6f17b-0ef0-4dda-894b-5390aeb76a49" />

### Consulta SQL — Todos os registros (SELECT *)
<img width="1363" height="730" alt="image" src="https://github.com/user-attachments/assets/593c0e93-7067-45c7-8509-f311eca40244" />

### Consulta SQL — Leituras críticas (baixa umidade + nutrientes ausentes)
<img width="1364" height="728" alt="image" src="https://github.com/user-attachments/assets/67dbd840-5c16-410c-bd35-34bc10ae8ed2" />

---
 
## 📊 Visualizações dos Dados
 
### Dashboard Geral
<img width="2000" height="1395" alt="dashboard_geral" src="https://github.com/user-attachments/assets/13387836-1be5-41b1-979f-525e31d52165" />

### Umidade do Solo ao longo do tempo
<img width="1934" height="585" alt="grafico_umidade" src="https://github.com/user-attachments/assets/f9c36289-58f6-4b5a-bbdf-22fd9e14a1f1" />

### pH do Solo
<img width="1934" height="585" alt="grafico_ph" src="https://github.com/user-attachments/assets/ff989137-7433-44f4-a425-9ff555a3b78e" />

### Chuva e Probabilidade de Precipitação
<img width="1935" height="585" alt="grafico_chuva" src="https://github.com/user-attachments/assets/7001c238-e667-42b7-94c3-35fe2f866932" />

### Consulta 4 — Acionamentos da Bomba
<img width="885" height="585" alt="consulta4_bomba" src="https://github.com/user-attachments/assets/4707985a-f777-46a8-9fe8-68c9be59cb84" />

### Consulta 7 — Nutrientes N, P, K
<img width="1035" height="585" alt="consulta7_nutrientes" src="https://github.com/user-attachments/assets/2d12c450-00d8-4211-9b07-96730f3c42ad" />

### Consulta 9 — Médias por Hora
<img width="2084" height="595" alt="consulta9_por_hora" src="https://github.com/user-attachments/assets/13b8d497-da5c-4b77-81d6-b470d7d70f1c" />
 
---
 
## 🔍 Consultas SQL Implementadas
 
| # | Descrição |
|---|---|
| 1 | Total de registros importados |
| 2 | Todos os registros ordenados por timestamp |
| 3 | Estatísticas agregadas (AVG, MIN, MAX) de umidade, pH e chuva |
| 4 | Frequência de acionamento da bomba (ON x OFF) com percentual |
| 5 | Leituras com umidade alta (≥ 60%) |
| 6 | Leituras com chuva detectada (rain_mm > 0) ordenadas por volume |
| 7 | Contagem de registros com nutrientes N, P, K presentes |
| 8 | Leituras com pH fora da faixa ideal (< 5.5 ou > 7.0) |
| 9 | Visão agregada por hora: média de umidade, pH e acionamentos da bomba |
| 10 | Leituras críticas: baixa umidade com ausência de nutrientes |
 
---
 
## 🗃 Histórico de lançamentos
 
- 0.3.0 — 02/05/2026 — Fase 3: importação Oracle + 10 consultas SQL analíticas + gráficos Python
- 0.2.0 — 14/04/2026 — Fase 2: código C/C++ de coleta de dados dos sensores (ESP32/Arduino)
- 0.1.0 — 23/03/2026 — Fase 1: concepção da FarmTech Solutions e modelagem inicial
---
 
## 📋 Licença
 
<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1">
<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1">
[MODELO GIT FIAP](https://github.com/agodoi/template) por [FIAP](https://fiap.com.br) está licenciado sobre [Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1).
---

