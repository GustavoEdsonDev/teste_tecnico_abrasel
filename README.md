# Análise do setor de Alimentação Fora do Lar

Projeto desenvolvido para o teste técnico da Abrasel. A análise utiliza dados públicos de CNPJ da Receita Federal para identificar estabelecimentos ativos do setor de Alimentação Fora do Lar (AFL), explorar sua evolução e caracterizar o porte empresarial.

## Objetivo

Responder, de forma reproduzível, às seguintes perguntas:

- Quantas empresas do setor AFL estão ativas?
- Como evoluiu a abertura de estabelecimentos nos cinco anos disponíveis na base?
- Qual é o porte empresarial predominante entre os registros enriquecidos?

## Estrutura do projeto

```text
.
├── data/
│   ├── raw/
│   │   ├── empresa1.csv
│   │   └── estabelecimento1.csv
│   └── processed/
│       └── estabelecimentos_afl_tratados.csv
├── analise_abrasel.ipynb
├── requirements.txt
└── README.md
```

- `data/raw/`: arquivos brutos, preservados sem alterações.
- `data/processed/`: base final tratada e exportada pelo notebook.
- `analise_abrasel.ipynb`: análise completa, organizada em seis etapas.
- `requirements.txt`: dependências Python do projeto.

## Arquivos de dados e Git

Os arquivos `empresa1.csv` e `estabelecimento1.csv` são grandes e, por isso, a pasta `data/raw/` está listada no `.gitignore`. Dessa forma, os dados brutos não são enviados ao repositório Git nem incluídos no histórico de versões.

Para executar o notebook em outra máquina, é necessário disponibilizar localmente esses dois arquivos dentro de `data/raw/`, mantendo exatamente estes nomes:

```text
data/raw/empresa1.csv
data/raw/estabelecimento1.csv
```

Os arquivos tratados e menores são gerados em `data/processed/` após a execução do notebook.

## Metodologia

1. Leitura dos CSVs sem cabeçalho, separados por `;`, com codificação `latin1`.
2. Aplicação do layout oficial simplificado das tabelas Empresas e Estabelecimentos.
3. Criação do CNPJ completo com 14 dígitos, preservando zeros à esquerda.
4. Seleção de estabelecimentos com situação cadastral ativa (`02`).
5. Filtro dos seis CNAEs definidos para o setor AFL:
   - `4721102`: Padaria e confeitaria
   - `5611201`: Restaurantes
   - `5611203`: Lanchonetes e casas de chá
   - `5611204`: Bares sem entretenimento
   - `5611205`: Bares com entretenimento
   - `5620104`: Fornecimento de alimentos para consumo domiciliar
6. Cruzamento das tabelas pelo CNPJ básico.
7. Análise de volume, evolução temporal e porte empresarial.
8. Exportação da base tratada em CSV.

## Resultados atuais

Com os arquivos presentes em `data/raw/`, a execução gera:

- `4.753.435` estabelecimentos lidos.
- `56.223` estabelecimentos AFL ativos.
- `55.759` empresas distintas, considerando o CNPJ básico.
- Janela temporal analisada: `18/05/2016` a `18/05/2021`.
- Pico de aberturas: `8.123` estabelecimentos em 2020.
- Registros com correspondência na tabela Empresas: `2.597`.
- Cobertura do cruzamento: `4,6%`.
- Porte predominante entre os registros enriquecidos: Microempresa (ME), com `2.132` estabelecimentos.

O resultado exportado está em `data/processed/estabelecimentos_afl_tratados.csv`.

## Limitações e cuidados de interpretação

Os arquivos de Empresas e Estabelecimentos possuem coberturas diferentes. Por isso, parte dos estabelecimentos ativos não encontra correspondência na tabela de Empresas. Esses registros são mantidos na base final, mas apresentam valores ausentes nos campos empresariais, como razão social e porte.

A série de 2021 é parcial, pois a data máxima de início de atividade disponível é 18/05/2021. Portanto, esse ano não deve ser comparado diretamente com anos completos.

## Como executar

1. Abra um terminal na raiz do projeto.
2. Instale as dependências:

   ```bash
   pip install -r requirements.txt
   ```

3. Abra `analise_abrasel.ipynb` no VS Code ou Jupyter.
4. Selecione um ambiente Python com as dependências instaladas.
5. Execute as células em ordem.

O notebook usa caminhos relativos à raiz do projeto. Se os arquivos brutos forem substituídos, mantenha os nomes `empresa1.csv` e `estabelecimento1.csv`, ou atualize os caminhos nas células de leitura do notebook.