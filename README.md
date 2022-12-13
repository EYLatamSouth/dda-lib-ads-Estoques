# dda-lib-ads-Estoques
Script de análise de dados que realiza o cálculo de valorização de estoque, item a item, baseado no valor médio de custo do item.


Atualizado por: Camilo José
Última Versão: 18-11-2022

## 1-	Descrição
Esta automação, tem como objetivo analisar os arquivos físicos de estoques (NF-e) e confrontá-los com o arquivo de controle de estoques do cliente, identificando assim falhas ou exceções de acordo com as necessidades das equipes de Auditoria. 

**Lista de testes padrão no ALTERYX:**
1) Validar a quantidade de itens na nota (NF-e) x arquivo de controle de estoques;
2) Validar o valor da nota fiscal (NF-e) x arquivo de controle de estoques;
3) Validar o valor de desconto da nota fiscal (NF-e) x arquivo de controle de estoques;
4) Validar a existência notas fiscais (NF-e), não presentes no arquivo de controle de estoques;
5) Validar a existência notas fiscais no arquivo de controle de estoques, que não tenham nota fiscal física (NF-e) correspondente;
6) Valorar a diferença identificada entre os arquivos físicos de nota fiscal (NF-e) e o controle de estoques;
7) Identificar e valorar os arquivos de nota fiscal não identificados entre o controle de estoques e os arquivos físicos de nota fiscal (NF-e);

Nota: Testes propostivos, mas outros podem ser realizados a partir dos dados extraídos das TAGs do XML.


## 2-	Pré Requisitos

Para executar o processamento dos testes, é necessário que os arquivos físicos de nota fiscal (XML) de cada NF-e, esteja armazenado em um repositório (pasta) que será processada pela automação, bem como o arquivo de controle de estoques do mês de análise.

## 3-	Informações Gerais da Estrutura do XML (NF-e)

		Informações Contidas no XML:
		
		Nota Fiscal Eletrônica: dia e horário da emissão, modelo, número e série, além do valor total;

		Emitente: Identificação básica (Nome, CNPJ, Razão Social), endereço (Inscrição Estadual, e código do município para cálculo do ICMS);

		Destinatário: Identificação básica (Nome, CNPJ, Razão Social), endereço (Inscrição Estadual);

		Produtos: quantidades, valores unitários, e uma breve descrição;

		Encargos: Valor dos produtos, frete e seguro, base de cálculo, PIS/COFINS/IPI/ICMS;

		Transporte: dados da transportadora e do veículo e volume transportado;

		Cobrança: Forma de pagamento e endereço para cobrança.

## 3.1-	 Mapeamento das Principais TAGs do Arquivo XML (NF-e)

|#|Nome da TAG no XML|Descrição|
| ------------ | ------------ | ------------ | ------------ |
|1|NFe|TAG raiz da NF-e|
|2|cNF|Código Numérico que compõe a Chave de Acesso|
|3|mod|Código do Modelo do Documento Fiscal|
|4|dhEmi|Data e hora de emissão do Documento Fiscal|
|5|tpNF|Tipo de Operação|
|6|nNF|Número do Documento Fiscal|
|7|CFOP|Código Fiscal de Operações e Prestações|
|8|qCom|Quantidade Comercial|
|9|vUnCom|Valor Unitário de Comercialização|
|10|vProd|Valor Total Bruto dos Produtos ou Serviços|
|11|vFrete|Valor Total do Frete|
|12|vDesc|Valor do Desconto|
|13|vOutro|Outras despesas acessórias|
|14|vTotTrib|Valor aproximado total de tributos federais, estaduais e municipais.|
|15|pICMS|Alíquota do imposto ICMS|
|16|vICMS|Valor do ICMS|
|17|pIPI|Alíquota do IPI|
|18|vIPI|Valor do IPI|
|19|pPIS|Alíquota do PIS|
|20|vPIS|Valor do PIS|
|21|pCOFINS|Alíquota do COFINS|
|22|vCOFINS|Valor do COFINS|
|23|vNF|Valor Total da NF-e|

Nota: As tags acima são as mais comuns, caso a análise seja mais específica, considere avaliar se todas as TAGs necessárias estão cobertas no fluxo padrão.

## 4-	Passo a Passo

1)	Prepare o repositório de dados (pasta) dos arquivos físicos de XML (NF-e) e armazene neste local todos os arquivos recebidos.
2)	Altere no Alteryx o caminho da pasta, onde encontra-se todos os arquivos XML.
3)	Prepare o arquivo de controle de estoque do cliente, de acordo com o modelo apresentado no Fluxo do Alteryx.

	3.1) Aqui é importante levar em consideração o escopo em análise, e para melhor desempenho, trabalhar somente com os campos necessários, como por exemplo:
	
			- CD_CHAVE_ACESSO_NFE 
			- DT_MOVIMENTO 
			- NR_NF 
			- DT_COMPETENCIA 
			- CD_PRODUTO 
			- DS_PRODUTO
			- QT_MOVIMENTO
			- VL_MOVIM_PRODU
			- VL_DESC_ITEM
			
			Nota: Havendo necessidade de análise de impostos e alíquotas, os mesmos deverão ser considerados no arquivo de controle.
			
4)	Altere no Alteryx a importação do arquivo de Controle de Estoques para referenciar o modelo atualizado.
5)	Após as preparações e alterações realizadas nos itens 1 ao 4, o processo pode ser iniciado (executar).
6)	Aguarde até aparecer uma janela com a mensagem “Finished Running”
7)	Acesse os resultados nos "Browses" ao final do fluxo, no container de "Resultados" e se necessário, exporte-os em xlsx apontando o caminho a ser salvo no container de "Exports".
