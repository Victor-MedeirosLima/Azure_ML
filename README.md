# Azure_ML_
Este repositório contém código e instruções para criar um modelo preditivo para aluguel de bicicletas usando Aprendizado de Máquina Automatizado no Azure Machine Learning Studio. O modelo prevê o número de aluguéis de bicicletas com base em dados históricos, considerando características sazonais e meteorológicas.


Conjunto de Dados
O conjunto de dados utilizado neste exercício é derivado do Capital Bikeshare e é utilizado de acordo com o acordo de licença de dados publicado. Os dados estão disponíveis em https://aka.ms/bike-rentals.

Guia Passo a Passo
### 1. Criar Job de Aprendizado de Máquina Automatizado
- No Azure Machine Learning Studio, acesse a página de Aprendizado de Máquina Automatizado (sob Autoria).
- Crie um novo job de Aprendizado de Máquina Automatizado com as seguintes configurações:
	- Configurações Básicas:
	- Nome do Job: msaprendizado-bicicleta-automl
	- Nome do Experimento: msaprendizado-bicicleta-aluguel
	- Descrição: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
	- Tags: nenhum
	- Tipo de Tarefa e Dados:
	- Selecione o tipo de tarefa: Regressão
	- Selecione o conjunto de dados ou crie um novo chamado aluguel-bicicletas a partir do link https://aka.ms/bike-rentals com as configurações apropriadas.
### 2. Configurar Tarefa
- Escolha a tarefa de regressão.
- Selecione o conjunto de dados aluguel-bicicletas.
- Escolha a coluna alvo: 'Rentals' (inteiro).
- Configurações adicionais:
	- Métrica primária: Erro quadrático médio normalizado (Normalized root mean squared error)
	- Explique o melhor modelo: Não selecionado
	- Usar todos os modelos suportados: Não selecionado
	- Modelos permitidos: Selecione apenas RandomForest e LightGBM.
### 3. Configurar Limites
- Expanda a seção de limites.
- Configure os seguintes limites:
	- Máximo de tentativas: 3
	- Máximo de tentativas simultâneas: 3
	- Máximo de nós: 3
	- Limite de pontuação da métrica: 0.085 (se um modelo atingir uma pontuação menor ou igual a 0.085, o job é encerrado)
	- Tempo limite: 15
	- Tempo limite de iteração: 15
	- Ativar término antecipado: Selecionado
### 4. Validação e Teste
- Tipo de validação: Divisão de treinamento-validação
- Percentual de dados de validação: 10
- Conjunto de teste: Nenhum
### 5. Configurar Computação
- Selecione o tipo de computação: Serverless
- Tipo de máquina virtual: CPU
- Nível de máquina virtual: Dedicado
- Tamanho da máquina virtual: Standard_DS3_V2 (ou escolha um tamanho disponível em sua assinatura)
### 6. Submeter o Job de Treinamento
- Submeta o job de treinamento e aguarde a conclusão. Isso pode levar algum tempo.
### 7. Revisar o Melhor Modelo
- Na aba de Visão Geral do job de Aprendizado de Máquina Automatizado, observe o resumo do melhor modelo. Tire uma captura de tela se desejar.
- Selecione o nome do algoritmo do melhor modelo para visualizar os detalhes.
- Na aba de Métricas, selecione os gráficos de resíduos e valores previstos-verdadeiros para revisar o desempenho do modelo.
### 8. Implementar e Testar o Modelo
- Na aba de Modelos, para o melhor modelo treinado, selecione "Implementar" e escolha a opção "Serviço Web".
- Configure o serviço web com as seguintes configurações:
	- Nome: msaprendizado-bicicleta-previsao
	- Descrição: Prever aluguéis de bicicletas
	- Tipo de computação: Azure Container Instance
	- Habilitar autenticação: Selecionado
- Aguarde o início da implantação - isso pode levar alguns segundos.
- Aguarde até que o status da implantação mude para "Concluído". Isso pode levar de 5 a 10 minutos.
### 9. Testar o Serviço Implementado
No Azure Machine Learning Studio, vá para Endpoints, abra o endpoint em tempo real `predict-rentals`.
- Na página do endpoint em tempo real, vá para a guia de Testes.
- Na área de entrada de dados de teste, substitua o JSON de modelo pelo seguinte:
```json
{
  "Inputs": {
    "data": [
      {
        "day": 1,
        "mnth": 1,
        "year": 2022,
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
### 10. Resumo
Você utilizou um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguéis esperado em um determinado dia, com base em características sazonais e meteorológicas. O modelo foi implementado como um serviço web e testado com sucesso.
