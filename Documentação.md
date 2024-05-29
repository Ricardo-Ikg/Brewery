Visão Geral:
Esse documento descreve o processo de ETL utilizando Azure Data Factory, Azure Storage Account, e Databricks. O objetivo é consumir dados de uma API, transformá-los conforme o necessário e carregá-los em um data lake.
Arquitetura:
•	Componentes principais:
-Azure Storage Account:
Armazenamento desde os dados brutos salvos em parquet, além de toda a cadeia de transformação ao longo do pipeline.

-Azure Data Factory (ADF):
Orquestrador da pipeline de ETL

-Databricks;
Ambiente de processamento e transformação dos dados, responsável pelo fluxo dos dados, consumo da API e persistência dos dados nas camadas.

Processo:
-Inicialmente, para criar a arquitetura foi instanciado um grupo de recursos na nuvem, que envolvia a conta de armazenamento para o data lake, um workspace no databricks e uma instância do Azure Data Factory. Como mostra a figura:
![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/98b0bea8-6943-4e3d-8a9e-2b92194462c8)



Após esse passo, deve-se criar uma conta de armazenamento, onde o data lake será instanciado e os dados transformados serão armazenados em camadas dentro de um contêiner, conforme a arquitetura medallion, como mostra a figura a seguir 
 ![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/98345e08-fa2f-4c37-97c1-4fbac1727e7c)


Com o data lake criado, deve-se fazer a montagem do caminho entre a camada de armazenamento utilizada pelo Databricks (dbfs) e o datalake, para entender com detalhes, veja a documentação da databricks sobre o assunto (https://docs.databricks.com/en/dbfs/mounts.html)
Com as conexões feitas, o passo seguinte é desenvolver os notebooks de requisição e transformação dos dados. Foram feitos 3 notebooks:
 ![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/3d3ec229-d96b-49ad-a5c5-f6ae81c1ecac)


1.	Ingestion_api_bronze:
Foi feito o consumo dos dados da API e ingestão dos dados brutos no formato parquet na camada bronze.
2.	Ingestion_bronze_silver:
Nesse caso foi feita a padronização dos dados conforme o modelo de dados
 
![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/ebb3f251-073d-4a55-ac3e-e50dce4edf06)

![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/a9a56409-9d6d-4105-bc0f-886c1cf0598e)
![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/110be43a-68b2-489e-977e-89cd3e3771c6)

 

Além disso nessa camada, os dados foram particionados por localização (País/State_province/City)

3.	Ingestion_silver_gold:
Aqui, foi feita a ingestão dos dados tratados conforme a regra de negócio descrita: 
	Registros agregados em quantidades de cervejarias por tipo e localização (coluna “State_province”) gerando uma view que foi armazenada na camada gold

Ao final deve-se estabelecer a conexão entre o ADF e o databricks – verificar na documentação da Microsoft (https://learn.microsoft.com/pt-br/azure/data-factory/transform-data-using-databricks-notebook.)
Após a conexão os notebooks foram organizados em um pipeline e orquestrados para que o pipeline possa ser executado.
![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/7d833d25-bef7-4045-9215-72178986a9e2)

1.	API_Bronze – Realiza o consumo de dados da API e grava os dados na camada bronze no formato parquet
2.	Bronze_silver – Executa o notebook de ingestão dos dados na camada silver com os dados particionados a partir da camada bronze e realiza as transformações mencionadas acima.
3.	Silver_gold – Executa o notebook de ingestão da view na camada Gold e cria a view conforme descrito acima.
Além disso o processo foi agendado para ser executado diariamente às 14:30 através de uma trigger
![image](https://github.com/Ricardo-Ikg/Brewery/assets/78457590/93dc0b39-fd0b-4071-938c-7c4da8be4dfc)


