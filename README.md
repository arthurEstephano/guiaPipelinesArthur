# Guia para implementação e desenvolvimeto de Pipelines DevOps

Este guia assim como este repositório foram criados visando instigar e prover os alunos da FeMASS com as informações mínimas necessárias para criação, desenvolvimento e implementação de pipelines nos seus projetos de desenvolvimento de software.

As justificativas para utilização de pipelines está contida no pdf que existe neste repositório que representa o Trabalho de Conclusão de Curso do autor e proprietário deste repositório.

## Sumário de Conteúdos

- [Guia para implementação e desenvolvimeto de Pipelines DevOps](#guia-para-implementação-e-desenvolvimeto-de-pipelines-devops)
  - [Sumário de Conteúdos](#sumário-de-conteúdos)
  - [Criando uma pipeline no GitHub Actions](#criando-uma-pipeline-no-github-actions)
  - [Conceitos Iniciais](#conceitos-iniciais)
  - [Desenvolvimento de uma pipeline para teste e build de um projeto java](#desenvolvimento-de-uma-pipeline-para-teste-e-build-de-um-projeto-java)
  - [Desenvolvendo uma pipeline para deploy de um projeto java em um cluster AKS](#desenvolvendo-uma-pipeline-para-deploy-de-um-projeto-java-em-um-cluster-aks)
  - [Desenvolvendo uma pipeline para integração com SonarCloud](#desenvolvendo-uma-pipeline-para-integração-com-sonarcloud)
  - [Outros serviços de pipelines](#outros-serviços-de-pipelines)
  - [Criador](#criador)

## Criando uma pipeline no GitHub Actions

Para criar uma pipeline GitHub Actions basta abrir no seu repositório a aba "actions", como na imagem abaixo:

![Opção Github Actions](images/actions-op.png)

Uma vez dentro dessa opção é possível procurar qual WorkFlow se deseja utilizar, por exemplo build java com maven:

![Busca no Github Actions](images/actions-search.png)

Uma vez o template selecionado e devidamente alterado, é possível adicionar este ao seu projeto utilizando o botão "Commit changes...", porém é necessário atentar-se a branch a qual esta pipeline está sendo commitada, para alterar a branch basta alterar para a desejada como na imagem abaixo:

![Troca de branches](images/switch-branches.png)

Antes de continuarmos e configurarmos nossa pipeline devidamente devemos nos atentar aos conceitos básicos de qualquer pipeline.

## Conceitos Iniciais

É necessário entender os parâmetros mínimos necessários para criação de uma pipeline, toda pipeline é escrita em arquivo com formato **.yml ou .yaml**, sendo necessário seguir a sintaxe deste formato, recomenda-se a utilização de extensões como a YAML do vscode:

![Yaml extension](images/yaml.png)

Seguindo para os conceitos com o arquivo yaml criado, é recomendado criar um nome (a partir da propriedade "name") para pipeline, a fim de manter controle caso haja mais de 1 como também definir nesta o contexto relacionado.

A utilização da propriedade "on" determina as condições para a qual a pipeline irá rodar.
Esta propriedade é acompanhada das suas condições que engatilham a pipeline, podendo ser "push", que é quando há atualizações diretamente em alguma branch ou resultado do merge de uma pull request ou "pull_request", que é quando há uma pull request aberta para uma determinada branch.

A maneira de determinar quais branches serão utilizadas é a partir da propriedade "branches", definindo a lista de branches a ser utilizada.

É possível ainda utilizar a proprieade "workflow_dispatch" que permite executar essa pipeline de maneira manual pela aba do Actions mostrada anteriormente.

É possível representar um arquivo da seguinte maneira então:

```yaml 
name: Test pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  workflow_dispatch:

```

Cumprindo os requisitos de condições é possível seguir para a propriedade "jobs", que são as instâncias de trabalho de cada pipeline, podendo existir múltiplos jobs sendo esses sequenciais (como serão vistos nos próximos tópicos) ou paralelos.

Cada job deve possuir um nome (nesse caso build) e o tipo de sistema operacional que vai receber o job em questão, representado pela propriedade "runs-on" (neste caso a última versão disponível de um Ubunto).

Definidos o job e a o SO que receberá o job podemos seguir para os passsos do job, que são representados pela propriedade "steps", que representa uma sequência de comandos ou tarefas que serão executadas como parte do job.

É importante ressaltar a necessidade da utilização do step de checkout do git, que permite o job de acessar o repositório diretamente.
Para esse é utilizada a seguinte anotação:

```yaml 
- uses: actions/checkout@v3"
```

A propriedade "uses" permite utilizar um trecho de código já gerado em algum repositório. Neste caso é utilizado um trecho do próprio GitHub Actions, o qual não necessita de muitos mais detalhes ou de propriedades auxiliares, veremos casos diferentes nos próximos tópicos.

A partir do checkout é possível executar steps dentro do código, podendo ser um script ou a utilização de outro trecho de código para instalar java no seu job por exemplo. É possível exemplificar com um hello world da maneira abaixo:

```yaml 
- name: Run a one-line script
  run: echo Hello, world!
```

Utilizando um pipe após o run é possível escrever scripts de múltiplas linhas (run: |).

A partir do entendimento destes componentes podemos seguir para o desenvolvimento de pipelines, é válido ressaltar que todos esses detalhes estão presentes na documentação do GitHub Actions no seguinte link: [Aprenda Git Actions](https://docs.github.com/pt/actions/learn-github-actions/understanding-github-actions)

Exemplo completo da pipeline inicial:

```yaml
name: Test pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Run a one-line script
        run: echo Hello, world!

      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.

```


## [Desenvolvimento de uma pipeline para teste e build de um projeto java](pipelines-examples\mavenyml)

## [Desenvolvendo uma pipeline para deploy de um projeto java em um cluster AKS](pipelines-examples\deployaks.yml)

## [Desenvolvendo uma pipeline para integração com SonarCloud](pipelines-examples\sonarcloud.yml)

## Outros serviços de pipelines

## Criador

**Arthur Moura**:

- <https://github.com/arthurEstephano>
