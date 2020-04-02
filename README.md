# Data Lake Architectures
Portifólio pessoal com modelos arquiteturais de projetos Big Data utilizando soluções variadas (AWS, GCP, Azure, On Premisse).

##  Data Lake Zones
Geralmente costumo quebrar uma grande solução em pequenas partes.
Em uma solução Big Data começo criando subdivisões para o Data Lake chamadas de zonas. Cada zona possui suas finalidades e premissas específicas. A seguir uma breve descrição de cada uma:

####  Landing Zone (Collected Data)
* Zona de aterrissagem de dados coletados.
* Ponto de entrada dos dados vindos de diversas fontes.
* É uma zona transiente, o carregamento e armazenamento dos dados são temporários.
* Pode haver verificações e validações básicas da qualidade durante ou após o processo de aquisição de dados.
* Fonte de dados para a Raw Zone.
* No caso de streaming, esta zona não possui um storage, podemos considerar a tecnologia utilizada (Kafka, Spark Stream, etc).
* Os agentes que fazem a coleta dos dados nas variadas fontes, conectam somente nesta zona como se fosse uma camada de integração entre as fontes de dados e o Data Lake, garantindo assim, uma melhor segurança no acesso à Raw Zone (core do Data Lake).

#### Raw Zone (Raw Data)
* Zona de dados brutos ou nativos.
* Armazenamento permanente dos dados em sua forma original (em estado bruto).
* É considerada o core do Data Lake.
* Não possui esquema de dados definidos, os dados podem ser semi estruturados ou não estruturados.
* Pode haver verificações e validações básicas da qualidade durante o processo de ingestão de dados.
* Servirá de fonte de dados para todo o Data Lake.

#### Treated Zone (Treated Data)
* Zona de dados tratados.
* Os dados são armazenados de forma estruturada, exige-se a definição do esquema de dados.
* Utiliza os dados brutos (Raw Data) como fonte de dados.
* Os dados são alterados e preparados para que estejam em conformidade com as exigências do negócio.
* Ocorre a aplicação de métodos de limpeza, validação, deduplication (versão única) e padronização de dados durante o processo de tratamento de dados.
* Esta zona permite verificar e validar a qualidade dos dados de forma intensa.
* O catálogo de metadados está disponível para todos que precisarem dos dados.
* Os dados desta zona geralmente são consumidos por analistas e cientistas de dados e podem servir de fonte de dados para Refined Zone ou Sandbox.

#### Refined Zone (Enriched Data)
* Zona refinada ou purificada, os dados são enriquecidos e armazenados nesta zona.
* É a última etapa de purificação dos dados.
* Os dados estão prontos para serem consumidos pelas aplicações LOB’s (Line of Business), prontos para criar modelos analíticos e serem servidos conforme as exigências do negócio.
* Precisamos garantir que os dados estejam em um formato que possam ser facilmente consumidos e de forma ágil.
* A grande maioria dos dados nesta zona são do tipo Hot Data, dados que são requisitados com alta frequência. São informações essenciais para os negócios, por isso exigem mídias de armazenamento mais rápidas e consequentemente mais caras.
* Podemos aplicar as devidas estratégias para armazenamento de Cold, Warm e Hot Data.
* Os dados são frequentemente transformados para atender as necessidades específicas das LOB’s e podem passar por mais etapas de verificações e validações de qualidade, gerenciamento do ciclo de vida e políticas de expurgo.
* O catálogo de metadados está disponível para todos que precisarem dos dados.
* Os dados desta zona também podem ser consumidos por analistas e cientistas de dados ou serem utilizados na Sandbox.

#### Sandbox (Machine Learning Exploration Zone)
* Zona destinada a pesquisas e exploração de dados (EDA - Exploratory Data Analysis).
* É utilizada por analistas e cientistas de dados para explorar dados e trabalhar com modelos preditivos de Machine Learning ou Deeping Learning.
* Permite que analistas e cientistas de dados criem casos de uso em ambientes prontos, sem necessidade de provisionar novos ambientes ou nova infraestrutura para fazerem seus experimentos.
* Provê segurança aos dados tratados e refinados que devem permanecer inalterados.
* Os dados podem ser importados a partir da Treated Zone e/ou Refined Zone onde os dados estão estruturados e devidamente tratados.
* Algumas informações podem ser enviadas de volta à Raw Zone, permitindo que dados derivados atuem como dados de origem.

## Solução Big Data AWS EMR

#### Visão Geral da Solução (Cold Path)
<img src="https://raw.githubusercontent.com/leonardo-jas/data-lake-architectures/master/data-lake-architecture-zones-aws-emr.png" width ="60%" height=60%>
<br>
Esta é uma visão geral da solução para Cold Path (processamento batch). Nesta primeira etapa eu crio subdivisões da solução chamadas de zonas.
<br>
Uma zona se comunica com outra atraves de alguma camada de processamento, interligando assim, todo o processo da linha de produção de dados (pipeline de dados).
<br>
Cada camada da solução tem uma responsabilidade única de efetuar um processamento específico com objetivo de atingir milestones dentro do pipeline de dados.
<br>
Milestones são marcos ou etapas que delimitam o caminho em que os dados percorrem entre sua fonte e seu destino. Um pipeline de dados possui vários milestones, por exemplo: etapas de aquisição, ingestão, tratamento, enriquecimento, consumo.
<br>
A tabela no canto inferior esquerdo mostra provisionamento de serviços AWS.

#### Data Stack (Cold and Hot Path)
<img src="https://raw.githubusercontent.com/leonardo-jas/data-lake-architectures/master/data-lake-architecture-stack-aws.png" width ="80%" height=80%>
<br>
Contém todas as tecnologias de computação (EMR) e mecanismos de armazenamento (S3, banco de dados) que constituirão a solução.

* Location: define se a solução será on premisse, cloud ou híbrida.
* Compute: são as tecnologias de processamento utilizadas.
* Storage: são os mecanismos de armazenamento utilizados.
* Sandbox: opcionalmente pode se construir uma Sandbox para que analistas e cientistas de dados possam trabalhar em seus experimentos e exploração de dados.
* LOB Applications: são as aplicações que consumirão os dados (Power BI, Tableau, Apps, etc.)
