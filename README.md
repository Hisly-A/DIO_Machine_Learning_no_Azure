# Machine Learning no Azure
Trabalhando com Machine Learning na Prática no Azure ML

## 🔎 O que é o Azure Machine Learning?	

>O Azure Machine Learning é um serviço de nuvem para acelerar e gerenciar o ciclo de vida dos projetos de ML (machine learning). Profissionais, cientistas de dados e engenheiros de ML podem usá-lo em seus fluxos de trabalho cotidianos para: treinar e implantar modelos e gerenciar MLOps (operações de aprendizado de máquina).

<sub>Fonte: <https://learn.microsoft.com/pt-br/azure/machine-learning/overview-what-is-azure-machine-learning?view=azureml-api-2></sub>


## Criando o espaço de trabalho no Azure Machine Learning

Realize o cadastro em https://azure.microsoft.com/pt-br/ e depois acesse o Azure em "https://portal.azure.com".

Selecione **+ Create Resource**, pesquise por **"*Machine Learning*"** e clique em **Create** para criar novo recurso do Azure Machine Learning com as seguintes configurações:

- **Subscription:** sua assinatura do Azure.

- **Resource Group:** clique em "Create New" abaixo do campo, informe um nome para seu Resource Group e clique em "OK".

- **Name:** insira um nome exclusivo para seu espaço de trabalho.

- **Region:** selecione a região geográfica mais próxima.

- **Storage account:** será criada automaticamente para seu espaço de trabalho.

- **Key vault:** o cofre de chaves será criado automaticamente para seu espaço de trabalho.

- **Application insights:** será criado automaticamente.

- **Container registry:** deixar na opção "None".

Clique em **Review + Create** revise os dados e clique em **Create**. Aguarde a criação do seu espaço de trabalho (pode demorar alguns minutos) e, em seguida, clique em **Go to resource** ou na "Home" do Azure, escolha o "Workspace" criado na etapa de "Preparação de Ambiente"

Na página do Workspace escolhido, clicar em **Launch Studio** para ser redirecionado à página https://ml.azure.com. Pode ser necessário fazer o login novamente.


## Criação do Automated ML no Machine Learning Studio

No Azure Machine Learning Studio(https://ml.azure.com), acessar **Automated ML**.

Clicar em **+ New Automated ML job** para criar um novo trabalho de ML automatizado com as seguintes configurações:

**Basic settings:** 

- **Job name:** mslearn-bike-automl (de acordo com o trabalho a ser criado)

- **New experiment name:** mslearn-bike-rental (de acordo com o experimento a ser criado)

- **Description:** descrição da trabalho que está sendo criado

- **Tags:** nenhuma

**Task type & Data:**

- **Task type:** Regressão

- **Select data:** crie um novo conjunto de dados com as seguintes configurações:

 - **Data type:** 

  - **Name:** bike-rental (sem espaços)

  - **Description:** colocar uma descrição sobre os dados

  - **Type:** Tabular

 - **Data Source:**

  - Selecione **From Web Files**

 - **Web URL:**

  - **Web URL:** https://aka.ms/bike-rentals

  - **Skip data validation:** não selecionar

 - **Settings:**

  - **File Format:** Delimited

  - **Delimiter:** Comma

  - **Encoding:** UTF-8

  - **Column headers:** Only First file has headers

  - **Skip rows:** None

  - **Dataset contains multi-line data:**  deixar desmarcado

  - Clique em **Next**

 - **Schema**

  - **Path:** Desabilitar

  - E habilitar todas as demais opções nessa tela.

  - Clicar em **Next**

 - **Review:** Revisar os dados preenchidos nas páginas anteriores

 - Clicar em **Create**

- **Task type & Data:** Selecionar o conjunto de dados que acabamos de criar: bike-rentals

- Clicar em **Next**

- **Task Settings**

 - **Target Column:** rentals (integer)

 - Clicar em **View Aditional configuration settings** e preencher conforme imagem abaixo:


- **Limits**

 - **Max trials:** 3

 - **Max concurrent trials:** 3

 - **Max nodes:** 3

 - **Metric score threshold:** 0,085 (se um modelo atingir a pontuação da métrica de 0,085 ou menos, o trabalho termina).

 - **Experiment timeout (minutes):** 15

 - **Iteration timeout (minutes):** 15

 - **Enable early termination:** selecionado

- - **Validate and test:**

- **Validation type:** Divisão de validação de treinamento

- **Percentage validation of data:** 10

- **Test data:** Nenhum

- - **Compute:**

- **Select compute type:** sem servidor

- **Virtual machine type:** CPU

- **Virtual machine tier:** Dedicado

- **Virtual machine size:** Standard_DS3_v2 (4 core(s), 14GB RAM, 28GB storage, $0.23/hr)

- **Number of instances:** 1

- **Review** - Revisar os dados e clicar no botão **Submit Trainning Job**.

Espere o trabalho terminar, pode demorar um pouco.

## Avaliação dos resultados

Na guia **Overview** é indicando qual modelo apresentou melhor resultado.

 ![](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/media/use-automated-machine-learning/complete-run.png)

 Na Seção "Best Model Summary" clique em **Algorithm Name** para visualizar seus detalhes.

 Clique na guia **Metrics** e verifique os gráficos que mostram o desempenho do modelo. 
 
 O gráfico **Residuals Histogram** mostra os resíduos (as diferenças entre os valores previstos e reais). O gráfico **Predict vs. True** compara os valores previstos com os valores verdadeiros.

## Implantação do modelo

Na guia **Model** do melhor modelo treinado, clique em **Deploy** e na opção **Web Service** para implantar o modelo com as seguintes configurações:

- **Name:** predict-rentals

- **Description:** opcional

- **Compute Type:** Azure Container Instance

- **Habilitar autenticação:** selecionado

- Clicar em **Deploy**

Aguarde até que o status da implantação mude para **Succeeded**.

## Teste do modelo

No Azure Machine Learning, selecionar **Endpoints** no menu esquerdo, e selecionar o endpoint criado (**predict-rentals**).

Na página do endpoint clicar na guia **Test**.

No campo **Input data to test endpoint** informe o seguinte JSON:

```
{
   "Inputs": { 
     "data": [
       {
         "day": 23,
         "mnth": 3,   
         "year": 2024,
         "season": 2,
         "holiday": 0,
         "weekday": 1,
         "workingday": 1,
         "weathersit": 2, 
         "temp": 0.3, 
         "atemp": 0.3,
         "hum": 0.3,
         "windspeed": 0.3 
       }
     ]    
   },   
   "GlobalParameters": 1.0
 }
```
Clique no botão **Test** e verifique que o resultado irá trazer um número previsto de aluguéis:

```
{
  "Results": [
    450.6343393128644
  ]
}
```


## Exclusão do espaço de trabalho

**Excluir o endpoint:**

- No estúdio Azure Machine Learning (https://ml.azure.com/?azure-portal=true), na guia **Endpoints**, selecione o ponto de extremidade de **predict-rentals**. Em seguida, clique em **Delete** e confirme que deseja excluir o endpoint.



**Excluir o espaço de trabalho:**

- No portal Azure (https://portal.azure.com/?azure-portal=true), na página **Resource groups**, abra o grupo de recursos que especificou ao criar o seu espaço de trabalho Azure Machine Learning.

- Clique em **Delete resource group**, digite o nome do grupo de recursos para confirmar que deseja excluí-lo e clique em **Delete**.



## Links utilizados

- <https://learn.microsoft.com/pt-br/azure/machine-learning/overview-what-is-azure-machine-learning?view=azureml-api-2>

- https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html

- https://azure.microsoft.com/pt-br/
