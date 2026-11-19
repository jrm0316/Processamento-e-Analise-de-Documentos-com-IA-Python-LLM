# Processamento e Análise de Documentos com IA - Python + LangChain + Groq

Projeto desenvolvido em **Python**, utilizando **LangChain**, **PromptTemplate** e **Groq API** para realizar a leitura de documentos de texto e gerar automaticamente resumos e identificação dos principais pontos com Inteligência Artificial.

O sistema lê o conteúdo de um arquivo `.txt`, constrói um prompt estruturado e envia o texto para um modelo de linguagem (LLM), exibindo a análise no terminal.

## Objetivo do projeto

Demonstrar a integração entre manipulação de arquivos e modelos de linguagem, utilizando Python para automatizar a leitura e a análise de conteúdo textual.

O projeto explora conceitos fundamentais que podem servir de base para aplicações como:

- Resumo de relatórios.
- Análise de documentos corporativos.
- Leitura e organização de informações textuais.
- Processamento inicial de documentos.
- Ferramentas de apoio à análise de conteúdo com IA.

A implementação atual trabalha com um único arquivo `.txt` por execução.

## Funcionalidades

- Leitura automática de um arquivo de texto.
- Utilização de caminhos absolutos baseados na localização do script.
- Leitura de conteúdo com codificação UTF-8.
- Construção de prompts dinâmicos utilizando `PromptTemplate`.
- Integração com a Groq API por meio do LangChain.
- Geração de resumo do documento.
- Identificação dos principais pontos abordados.
- Exibição da análise diretamente no terminal.
- Configuração da chave de API por variável de ambiente.

## Tecnologias utilizadas

- Python 3.12
- LangChain Core
- LangChain Groq
- PromptTemplate
- Groq API
- Modelo `openai/gpt-oss-120b`
- Python Dotenv
- Biblioteca padrão `os`
- File I/O
- Prompt Engineering

## Estrutura do projeto

```text
Processamento-e-Analise-de-Documentos-com-IA-Python-LLM/
│
├── IA_lendo_Arquivos_automaticamente_OK_OK.py
├── dados.txt
├── README.md
└── .env
```

O arquivo `dados.txt` contém o texto utilizado na análise.

O arquivo `.env` armazena a chave de acesso à Groq API e deve permanecer apenas no ambiente local.

O ambiente virtual `.venv` é criado durante a configuração.

**Importante:** não publique `.env` nem `.venv` no GitHub. Recomenda-se adicionar ambos ao `.gitignore`.

## Como funciona

O programa realiza cinco etapas principais.

### 1. Carregamento das configurações

```python
from dotenv import load_dotenv

load_dotenv()
```

A função `load_dotenv()` carrega as variáveis de ambiente definidas no arquivo `.env`.

### 2. Localização do documento

```python
import os

BASE_DIR = os.path.dirname(os.path.abspath(__file__))
ARQUIVO = os.path.join(BASE_DIR, "dados.txt")
```

A variável `BASE_DIR` identifica o diretório onde o script Python está localizado.

A função `os.path.join()` constrói o caminho para `dados.txt`.

Essa abordagem evita depender exclusivamente do diretório a partir do qual o comando de execução foi iniciado.

### 3. Leitura do conteúdo

```python
with open(ARQUIVO, "r", encoding="utf-8") as f:
    texto = f.read()
```

O programa abre o documento e carrega seu conteúdo em uma variável Python.

O comando `with` permite fechar automaticamente o arquivo após a leitura.

### 4. Construção do PromptTemplate

```python
from langchain_core.prompts import PromptTemplate

prompt = PromptTemplate(
    input_variables=["texto"],
    template="""
    Analise o texto abaixo e retorne:
    - Resumo
    - Principais pontos

    Texto:
    {texto}
    """
)

prompt_final = prompt.format(texto=texto)
```

A variável `{texto}` é preenchida dinamicamente com o conteúdo do arquivo.

Dessa forma, a mesma estrutura de prompt pode ser utilizada para analisar documentos com conteúdos diferentes.

### 5. Integração com o modelo de IA

```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    model="openai/gpt-oss-120b",
    temperature=0
)

resposta = llm.invoke(prompt_final)

print(resposta.content)
```

O prompt é enviado ao modelo de linguagem disponibilizado pela Groq.

A resposta gerada é exibida no terminal.

## Como executar o projeto

### 1. Clonar o repositório

```bash
git clone https://github.com/jrm0316/Processamento-e-Analise-de-Documentos-com-IA-Python-LLM.git
```

Entre na pasta:

```bash
cd Processamento-e-Analise-de-Documentos-com-IA-Python-LLM
```

### 2. Criar o ambiente virtual

Recomenda-se utilizar Python 3.12.

No Windows:

```powershell
py -3.12 -m venv .venv
```

Ative o ambiente virtual:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Instalar as dependências

```powershell
python -m pip install langchain-groq langchain-core python-dotenv
```

### 4. Configurar a chave da Groq

Crie um arquivo `.env` na raiz do projeto:

```env
GROQ_API_KEY=sua_chave_da_api_aqui
```

A chave pode ser obtida no [Groq Console](https://console.groq.com/).

### 5. Preparar o documento

Crie um arquivo chamado `dados.txt` na mesma pasta do script Python.

Adicione o texto que deseja analisar.

Exemplo:

```text
Python é uma linguagem de programação utilizada no desenvolvimento de sistemas, automação e inteligência artificial.

FastAPI permite desenvolver APIs REST com Python.

LangChain facilita a integração de aplicações com modelos de linguagem.

Essas tecnologias podem ser combinadas para desenvolver aplicações backend com inteligência artificial.
```

### 6. Executar o programa

```powershell
python .\IA_lendo_Arquivos_automaticamente_OK_OK.py
```

O programa lerá o documento, enviará seu conteúdo para o modelo de IA e exibirá a análise no terminal.

É necessária uma conexão com a internet e uma chave válida para acessar a Groq API.

## Teste funcional realizado

O projeto foi executado no Windows, utilizando o terminal PowerShell do Cursor.

### Documento utilizado

O arquivo `dados.txt` continha informações sobre:

- Python e suas aplicações.
- FastAPI para desenvolvimento de APIs REST.
- LangChain para integração com modelos de linguagem.
- Combinação dessas tecnologias em aplicações backend com IA.

### Resultado obtido

A IA retornou duas seções principais.

**Resumo**

Apresentou uma síntese do conteúdo, identificando as tecnologias mencionadas e sua utilização conjunta em aplicações backend com Inteligência Artificial.

**Principais pontos**

Identificou quatro tópicos:

1. Python como linguagem utilizada em desenvolvimento, automação e IA.
2. FastAPI como framework para criação de APIs REST.
3. LangChain como ferramenta de integração com LLMs.
4. Possibilidade de combinar essas tecnologias em aplicações backend com IA.

**Resultado do teste: aprovado.**

O programa conseguiu localizar o arquivo, ler o conteúdo, enviar o prompt ao modelo e exibir a análise.

O teste demonstrou o funcionamento principal da aplicação para o documento utilizado.

## Alterando o documento analisado

Para analisar outro conteúdo:

1. Abra o arquivo `dados.txt`.
2. Substitua o texto pelo novo conteúdo.
3. Salve o arquivo.
4. Execute novamente o programa.

Não é necessário alterar o código Python para analisar outro texto no mesmo arquivo.

## Conceitos demonstrados

### File I/O

Leitura de arquivos de texto utilizando recursos nativos do Python.

### Caminhos absolutos

Construção de caminhos a partir da localização do script, utilizando `os.path.abspath()`, `os.path.dirname()` e `os.path.join()`.

### PromptTemplate

Utilização de um template parametrizado para inserir dinamicamente o conteúdo do documento no prompt.

### Integração com LLM

Envio de conteúdo textual para um modelo de linguagem por meio da Groq API.

### Prompt Engineering

Definição de instruções para orientar a geração de resumos e a identificação dos principais pontos de um documento.

## Limitações atuais

- Processa apenas um arquivo `.txt` por execução.
- O nome do arquivo é definido diretamente no código.
- Não possui interface gráfica ou web.
- Não armazena automaticamente os resultados.
- Não possui tratamento específico para arquivos inexistentes ou com codificação inválida.
- O conteúdo completo é enviado ao modelo, estando sujeito aos limites de contexto e de requisição da API.
- Não utiliza embeddings, banco vetorial ou arquitetura RAG.
- Não implementa validação rígida do formato da resposta.
- As análises geradas por IA podem conter imprecisões ou informações não presentes no documento original.

## Possíveis melhorias futuras

- Permitir selecionar diferentes arquivos pelo terminal.
- Implementar leitura de múltiplos documentos.
- Adicionar suporte a PDF e outros formatos.
- Salvar automaticamente os resumos em arquivos.
- Implementar tratamento de exceções.
- Criar uma interface web utilizando Streamlit ou FastAPI.
- Implementar validação estruturada das respostas.
- Adicionar divisão de textos extensos em partes menores.
- Explorar integração com RAG para consulta de documentos maiores.

Essas funcionalidades são possibilidades de evolução e não fazem parte da versão atual.

## Relevância técnica

Este projeto demonstra a integração prática entre leitura de arquivos, construção de prompts e utilização de modelos de linguagem em Python.

Os conceitos implementados podem servir de base para sistemas de análise documental, automação de relatórios e aplicações backend com recursos de Inteligência Artificial.

## Autor

**Juliano Rodrigues Madeira**

Projeto desenvolvido para estudo e aplicação prática de Python, LangChain, Prompt Engineering e integração com modelos de linguagem.

GitHub: [jrm0316](https://github.com/jrm0316)
