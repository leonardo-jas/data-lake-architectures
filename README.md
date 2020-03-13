# Data Lake Architectures
Portifólio pessoal com modelos arquiteturais de projetos Big Data utilizando soluções variadas (AWS, GCP, Azure, On Premisses).
<br>
##  Data Lake Zones
Geralmente costumo quebrar uma grande solução em pequenas partes.
Uma solução Big Data começo criando subdivisões para o Data Lake chamadas de zonas. Cada zona possui sua finalidade específica. A seguir uma breve descrição de cada zona:
#### Transient Landing Zone (Landing Data)
* Zona de aterrisagem de dados.
* Carregamento e armazenamento de dados temporários durante o processo de aquisição de dados.
* Verificações e validações básicas da qualidade.
* Fonte de dados para a Raw Zone.
<br>
## Solução Big Data AWS EMR
#### data-lake-architecture-emr-zones.png
Neste desenho mostro uma visão geral da solução criando subdivisões chamadas de zonas, como funciona o fluxo, as camadas do Data Lake e o provisionamento de serviços AWS.
#### data-lake-architecture-emr-stack.png
Contém todas as tecnologias que farão parte da computqação, mecanismos de armazenamento (S3, banco de dados). Opcionalmente pode se construir a zona Sandbox para que analista de dados e cientistas de dados possam trabalhar em seus experimentos e exploração de dados.
