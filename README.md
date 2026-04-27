# 📚 ETL Boletim Inteligente — IA aplicada à Educação

> Pipeline ETL completo com Python e Claude AI para geração de boletins pedagógicos personalizados.

---

## 💡 Sobre o Projeto

Este projeto foi desenvolvido como um desafio prático de **ETL (Extract, Transform, Load)** aplicado ao domínio da **Educação**.

A ideia central é simples e poderosa: em vez de boletins genéricos com apenas números, a escola passa a entregar para cada aluno um **diagnóstico pedagógico real** — com análise do desempenho nas 12 disciplinas, identificação de riscos de reprovação, recomendações específicas de estudo e uma mensagem motivacional personalizada. Tudo gerado automaticamente por **Inteligência Artificial**.

Inspirado no projeto **Santander Dev Week 2023**, o pipeline segue a mesma lógica ETL, mas com um novo domínio, uma nova fonte de dados e uma IA diferente — o **Claude (Anthropic)** no lugar do ChatGPT.

---

## 🔄 Fluxo ETL

```
📂 students.csv        🤖 Claude AI              💾 JSON + CSV
   (Extract)    ──►    (Transform)     ──►        (Load)

Notas e faltas      Boletim pedagógico        boletins_output
de cada aluno       personalizado por aluno   .json  e  .csv
```

### E — Extract
Lê o arquivo `students.csv` com a biblioteca **pandas**. Para cada aluno, calcula automaticamente:
- Média geral das 12 disciplinas
- Disciplina com menor nota
- Lista de disciplinas com nota abaixo de 5,0 (risco de reprovação)

### T — Transform
Envia os dados de cada aluno para o **Claude** via API da Anthropic. O modelo age como um pedagogo especialista e retorna um JSON estruturado com quatro campos:

| Campo | Descrição |
|-------|-----------|
| `diagnostico` | Avaliação geral do desempenho do aluno |
| `riscos` | Pontos de atenção prioritários (disciplinas em risco) |
| `recomendacoes` | Ações práticas e específicas de estudo |
| `mensagem` | Frase motivacional personalizada para o aluno |

### L — Load
Salva os dados enriquecidos em dois formatos:
- `boletins_output.json` — estrutura completa, ideal para integração com sistemas
- `boletins_output.csv` — formato tabular, ideal para planilhas e relatórios da coordenação

---

## 🗂️ Estrutura do Projeto

```
📁 ETL_Boletim_Inteligente/
│
├── 📓 ETL_Boletim_Inteligente.ipynb   # Notebook principal com todo o pipeline
├── 📄 students.csv                    # Dados de entrada dos alunos
├── 📄 boletins_output.json            # Gerado após execução — boletins completos
├── 📄 boletins_output.csv             # Gerado após execução — formato tabular
└── 📄 README.md                       # Este arquivo
```

---

## 📋 Formato do CSV de Entrada

O arquivo `students.csv` contém uma linha por aluno com as seguintes colunas:

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `Student` | texto | Nome completo do aluno |
| `Year` | texto | Ano escolar (ex: `1º ano`) |
| `Arts` | número | Nota em Artes (0–10) |
| `PhysicalEducation` | número | Nota em Educação Física (0–10) |
| `Philosophy` | número | Nota em Filosofia (0–10) |
| `Sociology` | número | Nota em Sociologia (0–10) |
| `English` | número | Nota em Inglês (0–10) |
| `Physics` | número | Nota em Física (0–10) |
| `Chemistry` | número | Nota em Química (0–10) |
| `Biology` | número | Nota em Biologia (0–10) |
| `Geography` | número | Nota em Geografia (0–10) |
| `History` | número | Nota em História (0–10) |
| `Math` | número | Nota em Matemática (0–10) |
| `Portuguese` | número | Nota em Língua Portuguesa (0–10) |
| `Absences` | inteiro | Total de faltas no bimestre |

**Exemplo:**
```csv
Student,Year,Arts,PhysicalEducation,Philosophy,Sociology,English,Physics,Chemistry,Biology,Geography,History,Math,Portuguese,Absences
Ana Souza,1º ano,8.0,9.0,7.5,8.5,7.0,6.5,7.0,8.0,7.5,6.0,9.5,7.5,2
Carlos Lima,1º ano,6.0,7.5,4.0,5.0,4.5,3.5,4.0,5.0,4.5,3.5,4.0,6.0,12
```

---

## 🚀 Como Executar

### Pré-requisitos
- Python 3.8+
- Conta na [Anthropic](https://console.anthropic.com) com API Key ativa

### Passo a passo

**1. Clone o repositório ou faça upload dos arquivos no Google Colab**

**2. Instale a dependência:**
```bash
pip install anthropic pandas
```

**3. Configure sua API Key no notebook:**
```python
ANTHROPIC_API_KEY = 'sua-chave-aqui'
```

> 💡 No Google Colab, use `userdata` para não expor a chave:
> ```python
> from google.colab import userdata
> ANTHROPIC_API_KEY = userdata.get('ANTHROPIC_API_KEY')
> ```

**4. Execute as células na ordem:**
- Célula 1 — Instala o SDK
- Célula 2 — Define a API Key
- Célula 3 — Extract: lê e prepara os dados
- Célula 4 — Transform: gera os boletins com Claude
- Célula 5 — Load: salva os arquivos de saída

---

## 🤖 Modelo de IA Utilizado

| Propriedade | Valor |
|-------------|-------|
| Provedor | Anthropic |
| Modelo | `claude-haiku-4-5-20251001` |
| Motivo | Rápido, eficiente e ideal para geração de texto estruturado em escala |
| Saída esperada | JSON com 4 campos pedagógicos |

O prompt instrui o modelo a retornar **somente JSON válido**, sem markdown ou texto extra, facilitando o parse automático pelo pipeline.

---

## 🌱 Possibilidades de Expansão

Este projeto foi pensado como ponto de partida. Algumas direções naturais para evoluir:

- **📧 Envio automático por e-mail** — integrar com SendGrid ou Gmail API para enviar o boletim aos responsáveis
- **📄 Geração de PDFs individuais** — usar `fpdf2` ou `reportlab` para criar um arquivo por aluno com layout profissional
- **📊 Dashboard de desempenho** — visualizar médias por turma e ano com `plotly` ou `streamlit`
- **🗄️ Banco de dados histórico** — persistir os boletins em SQLite para acompanhamento bimestral
- **🌐 API REST** — expor os boletins via FastAPI para integração com sistemas escolares existentes
- **🔄 Automação periódica** — agendar execução bimestral com GitHub Actions

---

## 🎯 Contexto do Desafio

Este projeto faz parte de um desafio pessoal de aprendizado em **Ciência de Dados e IA Generativa**, com foco em:

- Compreender o fluxo ETL na prática
- Aplicar IA Generativa em um domínio real e útil
- Usar o SDK da Anthropic com o modelo Claude
- Estruturar saídas de IA em JSON para consumo programático

---

## 🛠️ Tecnologias

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=flat-square&logo=python)
![Anthropic](https://img.shields.io/badge/Claude-Anthropic-orange?style=flat-square)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat-square&logo=pandas)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter)

---

*Desenvolvido com 🤖 [Claude (Anthropic)](https://www.anthropic.com) + 🐍 Python By Brian Marpfy*
