# 📄 RPA Case – Nubank

## 1. Introdução

Este documento descreve a estrutura do projeto RPA desenvolvido para o **case do Nubank**, detalhando sua organização interna, os diretórios utilizados e os arquivos gerados durante a execução do processo.

A documentação apresenta:

- Como o projeto foi estruturado utilizando o **ReFramework do UiPath**.
- As pastas de entrada, temporárias e de saída.
- Explicação sobre cada tipo de relatório produzido.

---

## 2. Estrutura do Projeto

O projeto foi desenvolvido utilizando o **ReFramework padrão do UiPath**, garantindo boas práticas de organização, tratamento de exceções e uso de componentes.

A automação foi estruturada em **três fases principais**, de acordo com os passos descritos no arquivo `Case RPA – Nubank`:

### 2.1 Passo 1 – InitAllApplications

- Inicialização das aplicações.
- Leitura dos arquivos de entrada.
- Enriquecimento dos dados (conversão de valores, validações e busca de endereço).

### 2.2 Passo 2 – Process

- Lógica principal de geração dos relatórios individuais de vendas.
- Aplicação de regras de negócio (desconto, taxas e prazo de emissão).
- Conversão dos arquivos para PDF protegido.
- Consolidação do relatório de erros.

### 2.3 Finalização – EndProcess

- Envio do e-mail final contendo os relatórios gerados e o relatório de erros como anexos.

💡 **Nota:** Toda a lógica do case foi organizada dentro da pasta `WORKFLOWS_CASE`, que concentra os workflows principais e auxiliares para facilitar a análise do desenvolvimento.  
Além dos workflows, a pasta contém também **dois scripts VBA**, utilizados para aplicar formatações adicionais nos relatórios antes do envio por e-mail.

---

## 3. Estrutura da Pasta `Data`

### 3.1 Input

Contém os arquivos de entrada:

- `Vendor List.xlsx`
- `Sales List.pdf`

Esses arquivos são usados para análise e processamento.

### 3.2 Temp

Contém o **template** para o relatório final.

### 3.3 Output

Contém os arquivos e diretórios gerados após a execução do processo.

---

### 3.3.1 Relatório de Vendas

Arquivo contendo todas as informações resultantes da execução do processo.  
Este documento centraliza os dados extraídos do `Sales List.pdf` e do `Vendor List.xlsx`, além das informações complementares vindas das APIs de câmbio e CEP dos Correios.

> **Importante:**
>
> - Este relatório consolida **todas as vendas processadas**, inclusive as que não se enquadram nos critérios de geração de relatórios individuais por vendedor.
> - Serve como fonte de **auditoria e acompanhamento** do processo.
> - Não deve ser confundido com os relatórios individuais enviados aos vendedores.

---

### 3.3.2 Relatório de Erros

Arquivo consolidado com todas as inconsistências encontradas durante a validação dos dados do processo.

**Regras de negócio verificadas:**

- Invoice Number inválido (contendo caracteres não numéricos).
- Vendor ID em formato incorreto (fora do padrão `AA000000`).
- Período da Invoice fora do intervalo permitido (2018 a 2021).
- Status do vendedor diferente de `Checked`.

**Campos do relatório:**

- Número da Invoice.
- Vendor ID.
- Descrição do erro identificado.

> Este documento dá visibilidade às falhas de consistência encontradas durante a execução e serve de base para ajustes futuros nos dados de entrada.

---

### 3.3.3 Pasta `Sales Report`

Diretório criado pelo processo para armazenar os relatórios gerados.

Ele é dividido em duas subpastas:

- **Relatorios_Processamento**  
  Contém os arquivos em formato `.xlsx`, utilizados durante o processamento das informações.  
  Esses arquivos são intermediários e servem como base para aplicação das regras de negócio, conversões e formatações antes da geração final.

- **Relatorios_Prontos**  
  Contém os relatórios finais prontos para envio, já no formato `.pdf` protegido por senha.  
  Cada relatório corresponde a um vendedor válido conforme as regras de negócio estabelecidas.

> Essa estrutura permite separar de forma clara os arquivos de trabalho temporário dos documentos finais entregáveis.

---

## 🗂 Estrutura Final de Pastas

Data/
├── Input/
│ ├── Vendor List.xlsx
│ └── Sales List.pdf
├── Temp/
│ └── Template.xlsx
└── Output/
├── Relatório de Vendas.xlsx
├── Relatório de Erros.xlsx
└── Sales Report/
├── Relatorios_Processamento/
└── Relatorios_Prontos/

## 🛠 Tecnologias Utilizadas

- UiPath ReFramework
- VBA para formatação de relatórios
- Integração com APIs externas (Câmbio e CEP dos Correios)
