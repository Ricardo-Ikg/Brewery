#Monitoramento:

Essa proposta de solução de monitoramento visa prevenir a perda de dados e a potencial falha do pipeline de ingestão, essa solução envolve ferramentas disponibilizadas pela própria Azure e ferramentas terceiras com integração ao ambiente como o DataDog.

Para o monitoramento da pipeline, evitando falhas de processo, evitando que a falha passe despercebida, pode-se verificar o monitor do próprio ADF afim de garantir diariamente a saúde do processo, além disso, o ADF fornece uma ferramenta de alertas e métricas, onde podemos
programar uma mensagem no meio de comunicação que for mais conveniente ao time de sustentação do processo para que ele possa atuar com urgência.

Além das ferramentas do Data Factory, recomenda-se a integração do ADF com a plataforma Data Dog, sendo ferramenta mais robusta e com integração com o ADF, pode-se obter métricas mais detalhadas e customizadas sobre o log dos processos, e sobre a quantidade de dados em fluxo ao longo do processo,
evitando assim perda de registros, é possível também criar dashboards com os resultados das métricas e monitores para que se possa analisar o comportamento das ferramentas ao longo do tempo  prevenindo falhas da pipeline ao utilizar métricas customizadas. Além disso, como se trata de uma ferramenta de monitoramento unificada, ela dá suporte ao processo como um todo e não somente o que diz respeoito ao pipeline, por exemplo, podemos monitorar a saúda dos cluster do databricks. 
