Jupyter Notebook
mardown-tests
Last Checkpoint: um minuto atrás
(unsaved changes)
Current Kernel Logo
Python 3 
File
Edit
View
Insert
Cell
Kernel
Widgets
Help

Referencias
https://www.datacamp.com/community/tutorials/markdown-in-jupyter-notebook https://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Working%20With%20Markdown%20Cells.html

Headers
(Header 1, title)
(Header 2, major headings)
(Header 3, subheadings)
(Header 4)
(Header 5)
(Header 6)
Code section
codigo python
if 1==1:
    print("Hello World")
Python
str = "This is a block level code"
print(str)

Blockquotes
This is a blockquote
This is another blockquote
Bold and Italic Text
This is bold text

** This is bold text

__ This is bold text

Ordered List
Fish
Eggs
Cheese
List
item 1
item 2
item 3
6
# Data Lake Architectures
Portifólio pessoal com modelos arquiteturais de projetos Big Data utilizando soluções variadas (AWS, GCP, Azure, On Premisse).
​
##  Data Lake Zones
Geralmente costumo quebrar uma grande solução em pequenas partes.
Uma solução Big Data começo criando subdivisões para o Data Lake chamadas de zonas. Cada zona possui sua finalidade específica. A seguir uma breve descrição e premissas de cada zona:
​
#### Transient Landing Zone (Landing Data)
* Zona de aterrisagem de dados.
* Ponto de entrada dos dados vindos de diversas fontes.
* Carregamento e armazenamento dos dados são temporários.
* Pode haver verificações e validações básicas da qualidade durante ou após o processo de aquisição de dados.
* Fonte de dados para a Raw Zone.
* No caso de streaming, esta zone não possui um storage, pode se considerar a tecnologia utilizada (Kafka, Spark Stream, etc).
* Os coletores que fazem aquisição de dados nas variadas fontes conectam somente nesta zona.
​
#### Raw Zone (Raw Data)
* Zona de dados brutos ou nativos.
* Armazenamento permanente dos dados em sua forma original (em estado bruto).
* Não possui esquema de dados definidos, os dados podem ser semi estruturados ou não estruturados.
* Pode haver verificações e validações básicas da qualidade durante o processo de ingestão de dados.
* Servirá de fonte de dados para todo o Data Lake.
​
#### Trusted Zone (Treated Data)
* Zona confiável, zona dos dados tratados.
* Os dados são armazenados de forma estruturada, há a definição do esquema de dados.
* Utiliza os dados brutos da Raw Zone como fonte de dados.
* Os dados são alterados e preparados para que estejam em conformidade com as políticas do negócio.
* Ocorre a aplicação de métodos de limpeza, validação, deduplication (única versão) e padronização de dados.
* Esta zona permite verificar e validar a qualidade dos dados de forma intensa.
* O catálogo de metadados está disponível para todos que precisem dos dados.
​
#### Refined Zone (Enriched Data)
* Zona refinada ou purificada (última etapa de purificação), os dados são enriquecidos e armazenados nesta zona.
* Osados estão prontos para serem consumidos pelas aplicações LOB’s (Line of Business) e prontos para criar modelos analíticos.
* Precisa garantir que os dados estejam em um formato que possam ser facilmente consumidos de forma ágil.
* A grande maioria dos dados são do tipo Hot Data, dados que são acessados e requisitados com alta frequência. São informações essenciais para os negócios.
* Os dados são frequentemente transformados para atender as necessidades específicas das LOB’s.
* Os dados podem passar por mais etapas de verificações e validações de qualidade, gerenciamento do ciclo de vida e políticas de expurgo.
* Os dados são frequentemente transformados para atender as necessidades dos LOB’s específicos.
* O catálogo de metadados está disponível para todos que necessitam dos dados.
​
#### Sandbox (Machine Learning Exploration Zone)
* Zona destinada a pesquisas e exploração de dados (EDA - Exploratory Data Analysis)
* Consumidas por analistas e cientistas de dados para trabalhar com modelos preditivos de Machine Learning e Deeping Learning.
* Permite que cientistas e analistas de dados criem casos de uso em ambientes prontos, sem necessidade de provisionar novos ambientes para testar os dados.
* Provê segurança aos dados tratados de outras zonas que não podem alterados.
* Os dados podem ser importados a partir de qualquer uma das zonas. As mais comuns são Trusted Zone e Refined Zone onde os dados estão estruturados e devidamente tratados.
* Algumas informações podem ser enviadas de volta à zona bruta, permitindo que dados derivados atuem como dados de origem.
​
## Solução Big Data AWS EMR
​
#### Visão Geral da Solução (Cold Path)
<img src="https://raw.githubusercontent.com/leonardo-jas/data-lake-architectures/master/data-lake-architecture-emr-zones.png" width ="60%" height=60%>
<br>
Esta é uma visão geral da solução para Cold Path (processamento batch). Nesta primeira etapa eu crio subdivisões da solução chamadas de zonas.
<br>
Uma zona se comunica com outra atraves de alguma camada, interligando assim, todo o processo do pipeline de dados.
<br>
Cada camada do Data Lake tem uma responsabilidade única de efetuar seu processamento específico e atingir algum milestone dentro do pipeline de dados.
<br>
Milestones são marcos que delimitam o caminho em que os dados percorrem entre sua fonte e seu alvo, ou seja, os dados de uma fonte devem sempre atingir um alvo.
<br>
A tabela no canto inferior esquerdo mostra provisionamento de serviços AWS.
​
#### Data Stack (Cold and Hot Path)
<img src="https://raw.githubusercontent.com/leonardo-jas/data-lake-architectures/master/data-lake-architecture-emr-stack.png" width ="80%" height=80%>
<br>
Contém todas as tecnologias de computação (EMR) e mecanismos de armazenamento (S3, banco de dados) que constituirão a solução.
​
* Location: define se a solução será on premisse, cloud ou híbrida.
* Compute: são as tecnologias de processamento utilizadas.
* Storage: são os mecanismos de armazenamento utilizados.
* Sandbox: opcionalmente pode se construir uma Sandbox para que analistas e cientistas de dados possam trabalhar em seus experimentos e exploração de dados.
* LOB Applications: são as aplicações que consumirão os dados (Power BI, Tableau, Apps, etc.)
