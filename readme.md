Abaixo segue detalhamento de minha experiência com AWS Step Functions e Bedrock que aprendi durante o bootcamp:

# AWS Step Functions & Amazon Bedrock - Guia de Introdução

Ambos os serviços da AWS permitem a criação e orquestração de workflows serverless, integrando várias funcionalidades de IA (Inteligência Artificial) e serviços gerenciados da AWS.

## 1. O que é o AWS Step Functions?

O **AWS Step Functions** é um serviço que facilita a coordenação de múltiplos serviços da AWS em um workflow visual. É útil para construir aplicações distribuídas, automatizar tarefas de longa duração e gerenciar workflows complexos. Ele utiliza o **Amazon States Language (ASL)** para definir estados, transições e lógica de controle.

### Principais componentes:
- **Workflow Studio**: Uma interface visual que permite criar, testar e modificar workflows de forma gráfica, sem a necessidade de codificar manualmente o ASL.
- **Amazon States Language (ASL)**: A linguagem de definição de estado utilizada no Step Functions para descrever a máquina de estado que irá orquestrar suas tarefas. Cada estado pode ser definido para executar uma ação específica (chamadas de API, lambda functions, etc.).
- **Integrations**: Step Functions pode orquestrar uma variedade de serviços AWS, como AWS Lambda, S3, DynamoDB, e agora, serviços de IA como o **Amazon Bedrock**.

## 2. O que é o Amazon Bedrock?

O **Amazon Bedrock** oferece uma plataforma para desenvolver e escalar aplicações de IA com **modelos de fundação (Foundation Models)**. Ele permite que desenvolvedores e empresas integrem IA generativa de ponta sem a necessidade de gerenciar infraestrutura complexa. Bedrock oferece acesso a diversos modelos de IA pré-treinados para diferentes casos de uso, como processamento de linguagem natural (NLP), visão computacional e geração de conteúdo.

### Principais características:
- **Modelos de fundação**: Bedrock oferece acesso a modelos pré-treinados, como GPT, Claude (Anthropic) e Stable Diffusion, facilitando a implementação de IA generativa.
- **Integração com Step Functions**: Você pode integrar facilmente os modelos do Bedrock dentro de seus workflows Step Functions para criar soluções de IA automatizadas.

## 3. Criando Workflows com AWS Step Functions

### 3.1. Workflow Studio

O **Workflow Studio** permite criar e gerenciar workflows visualmente, facilitando a definição dos estados e transições no ASL. Com ele, você pode arrastar e soltar componentes, definir os detalhes dos estados e rapidamente testar seu workflow.

### 3.2. Linguagem de Definição de Estados - ASL

O **Amazon States Language (ASL)** define como os estados no Step Functions se comportam. Aqui estão os principais componentes do ASL:
- **States**: Cada estado representa uma etapa no seu workflow.
- **Transitions**: Define a lógica de como o workflow passa de um estado para outro.
- **Parallel**: Para executar estados em paralelo.
- **Catch e Retry**: Para lidar com exceções e tentar novamente ações que falharam.
- **Choice**: Condicionais que permitem diferentes caminhos no workflow.

### Exemplo básico de definição ASL:
```json
{
  "StartAt": "TaskState",
  "States": {
    "TaskState": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:my-function",
      "End": true
    }
  }
}
```

## 4. Permissões e Perfis IAM

Para executar workflows do AWS Step Functions e acessar serviços como Amazon Bedrock, você deve garantir que as permissões corretas estejam configuradas em seus perfis **IAM (Identity and Access Management)**. Algumas práticas recomendadas:

- **Roles**: Crie roles específicas para Step Functions com permissões necessárias para acessar outros serviços (ex.: Lambda, S3, Bedrock).
- **Políticas de Acesso**: Defina políticas de acesso refinadas para evitar permissões excessivas. As permissões mínimas recomendadas para Step Functions incluem:
  - `states:StartExecution`
  - `states:DescribeExecution`
  - `states:StopExecution`

Exemplo de política para Step Functions:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "states:StartExecution",
                "states:StopExecution",
                "states:DescribeExecution"
            ],
            "Resource": "*"
        }
    ]
}
```

## 5. Disponibilidade de Serviços

- O **AWS Step Functions** está amplamente disponível em várias regiões da AWS.
- O **Amazon Bedrock** ainda está em fase de expansão e pode estar disponível apenas em regiões específicas no momento, como **US East (N. Virginia)**.

### Checagem de disponibilidade:
Recomendável verificar a [tabela de regiões da AWS](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) para garantir que o Bedrock está disponível na sua região de interesse.

## 6. Exemplos de Uso Combinado

Aqui está um exemplo de um workflow que chama um modelo de IA no Amazon Bedrock dentro de um Step Functions:

1. **Entrada do usuário**: O usuário faz upload de um arquivo de texto em um bucket S3.
2. **Processamento de linguagem natural (NLP)**: O Step Functions chama um modelo de IA no Amazon Bedrock para analisar o texto e extrair informações relevantes.
3. **Resposta automática**: Um serviço Lambda pode usar o resultado da análise para enviar uma resposta automática ou realizar outra ação, como armazenamento no DynamoDB.

## 7. Conclusão

Tanto o **AWS Step Functions** quanto o **Amazon Bedrock** são poderosas ferramentas para criar workflows escaláveis e baseados em IA. Utilizando o Workflow Studio e integrando com o Amazon Bedrock, é possível criar soluções altamente automatizadas e inteligentes sem a complexidade de gerenciar a infraestrutura manualmente.
