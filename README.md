# FIAP - Faculdade de Informática e Administração Paulista

<p align="center">
<a href= "https://www.fiap.com.br/"><img src="assets/logo-fiap.png" alt="FIAP - Faculdade de Informática e Admnistração Paulista" border="0" width=40% height=40%></a>
</p>

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

- Oracle Database XE 21c (ou superior) instalado e em execução
- Python 3.9+
- Biblioteca `oracledb`: `pip install oracledb`
- Oracle SQL Developer (para visualização e execução das consultas)

### Fase 1 — Configurar a conexão

Edite as variáveis no topo do arquivo `scripts/importar_oracle.py`:

```python
DB_USER     = "seu_usuario"
DB_PASSWORD = "sua_senha"
DB_DSN      = "localhost:1521/XEPDB1"
```

### Fase 2 — Importar os dados para o Oracle

```bash
cd scripts
python importar_oracle.py
```

O script irá:
1. Criar (ou recriar) a tabela `sensor_readings`
2. Ler o arquivo `FarmtechsolutionsPBL.csv`
3. Inserir os 48 registros em lote via `executemany`
4. Exibir confirmação com total de registros importados

### Fase 3 — Executar as consultas
### Conexão e estrutura

| Conexão ativa | Tabela criada | Colunas | Importação dos dados | Acionamentos da bomba de irrigação | Leituras críticas (baixa umidade + nutrientes ausentes)
<img width="1364" height="724" alt="image" src="https://github.com/user-attachments/assets/7cacb065-fc14-4fa8-93e7-d6dec366336d" />
<img width="1363" height="727" alt="image" src="https://github.com/user-attachments/assets/c7ec5010-1988-496b-9ee7-5fddeb623841" />
<img width="1365" height="724" alt="image" src="https://github.com/user-attachments/assets/b9e29078-9bf5-47c3-a31c-1bcb8efb28e7" />
<img width="1364" height="729" alt="image" src="https://github.com/user-attachments/assets/6191d4b8-2083-4278-a81b-fb4f4b16e908" />

## Visualizações dos Dados

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

## 🗃 Histórico de lançamentos

- 0.3.0 - 02/05/2026 — Fase 3: importação Oracle + consultas SQL analíticas
- 0.2.0 - 14/04/2026 — Fase 2: código C/C++ de coleta de dados dos sensores
- 0.1.0 - 23/03/2026 — Fase 1: concepção da FarmTech Solutions e modelagem inicial

---

