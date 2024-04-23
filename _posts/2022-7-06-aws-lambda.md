---
layout: post
title: AWS Lambda
subtitle: Falaaaa pessoaaal! Hoje vou falar um pouco sobre AWS lambda.
tags: [aws, lambda, serverless, faas]
comments: true
author: Eduardo M. Rezende
---

# O que é AWS?
AWS é um serviço de computação em nuvem feito pela Amazon. Ela foi criada iniciantemente para suprir as necessidades da própria empresa.

# O que é AWS Lambda?
AWS Lamba é uma categoria de serverless do tipo FaaS, ele é um tipo de sistema que não precisa se configurar servidor para usá-lo, só precisa da sua regra de negócio.

Além desta facilidade AWS lambda tem algo bem legal, com ela você paga só o tempo de execução, não precisa pagar por algo que não usa. Vamos supor que você tenha um blog pessoal, você faz posts todas as noites. Como seus seguidores sabem disto, o seu blog tem muitos acessos à noite, mas durante o dia tem bem menos. Com AWS lambda você paga só quando eles fazem acesso, sem precisar pagar a parte da manhã o mesmo preço que pagaria na parte da noite. E também com AWS lambda você garante que seu site não ira cair, pois, ela é escalável, quanto mais pessoas acessam, mais lambdas vai sendo levantadas.

Ta… Mas o que eu preciso para usar essa tecnologia?
Para usar AWS lambda vamos precisar de uma conta na AWS, também vamos precisar de usar o AWS CLI, e por fim o SAM CLI (Como não é o foco deste artigo a instalação e configuração vou ter que pular esta parte, mas na própria documentação explica, é super de boa.).

# Criando um projeto Lambda
Vamos para onde interessa, a como criar um projeto lambda!

```bash
❯ sam init            

You can preselect a particular runtime or package type when using the `sam init` experience.
Call `sam init --help` to learn more.

Which template source would you like to use?
	1 - AWS Quick Start Templates
	2 - Custom Template Location
Choice: 1

Choose an AWS Quick Start application template
	1 - Hello World Example
	2 - Multi-step workflow
	3 - Serverless API
	4 - Scheduled task
	5 - Standalone function
	6 - Data processing
	7 - Infrastructure event management
	8 - Machine Learning
Template: 1

Use the most popular runtime and package type? (Python and zip) [y/N]: y

Would you like to enable X-Ray tracing on the function(s) in your application?  [y/N]: n

Project name [sam-app]: hello

Cloning from https://github.com/aws/aws-sam-cli-app-templates (process may take a moment)

    -----------------------
    Generating application:
    -----------------------
    Name: hello
    Runtime: python3.9
    Architectures: x86_64
    Dependency Manager: pip
    Application Template: hello-world
    Output Directory: .
    
    Next steps can be found in the README file at ./hello/README.md
        

    Commands you can use next
    =========================
    [*] Create pipeline: cd hello && sam pipeline init --bootstrap
    [*] Validate SAM template: sam validate
    [*] Test Function in the Cloud: sam sync --stack-name {stack-name} --watch
    

SAM CLI update available (1.52.0); (1.51.0 installed)
To download: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html
```

Vamos entender o que o rolou.

Primeiro “sam init” com este comando ele cria um projeto lambda para gente, fazendo algumas perguntinhas.

Segundo, ele pergunta qual modelos queremos usar, se é um customizado ou um da AWS pronto, para este artigo eu selecionei “1” sendo um pronto.

Terceiro, ele pergunta qual modelo queremos usar, eu escolhi o “hello word” sendo o “1”.

Quarto, ele pergunta qual categoria de pacote e a linguagem, eu coloquei “y” porque vamos usar o padrão, mas tem como escolher entre outros também.

```bash
Use the most popular runtime and package type? (Python and zip) [y/N]: n

Which runtime would you like to use?
	1 - dotnet6
	2 - dotnet5.0
	3 - dotnetcore3.1
	4 - go1.x
	5 - graalvm.java11 (provided.al2)
	6 - graalvm.java17 (provided.al2)
	7 - java11
	8 - java8.al2
	9 - java8
	10 - nodejs16.x
	11 - nodejs14.x
	12 - nodejs12.x
	13 - python3.9
	14 - python3.8
	15 - python3.7
	16 - python3.6
	17 - ruby2.7
	18 - rust (provided.al2)
Runtime: 14

What package type would you like to use?
	1 - Zip
	2 - Image
Package type: 
```

Quinto, ele pergunta se queremos usar o X-Ray tracing em nosso app, eu coloquei “n” para nosso projeto.

E por último ele nos pergunta qual sera o nome de nosso app.

Com isso ele cria uma base parecida com esta:

```bash
❯ tree                   
.
├── events
│   └── event.json
├── hello_world
│   ├── app.py
│   ├── __init__.py
│   └── requirements.txt
├── __init__.py
├── README.md
├── template.yaml
└── tests
    ├── __init__.py
    ├── integration
    │   ├── __init__.py
    │   └── test_api_gateway.py
    ├── requirements.txt
    └── unit
        ├── __init__.py
        └── test_handler.py
```

# Como rodar o app?
Legal eu sei criar o app! Mas quero ver meu app rodando, para de enrolar ai neh!

Para rodarmos nosso app precisamos de apenas mais dois comandos!

O primeiro é o sam build, com esse comando o sam vai criar um arquivo que ele consegue ler depois para ser executado.

```bash
❯ sam build 
Your template contains a resource with logical ID "ServerlessRestApi", which is a reserved logical ID in AWS SAM. It could result in unexpected behaviors and is not recommended.
Building codeuri: /home/dudu/estudo/hello/hello_world runtime: python3.8 metadata: {} architecture: x86_64 functions: ['HelloWorldFunction']
Running PythonPipBuilder:ResolveDependencies
Running PythonPipBuilder:CopySource

Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Validate SAM template: sam validate
[*] Invoke Function: sam local invoke
[*] Test Function in the Cloud: sam sync --stack-name {stack-name} --watch
[*] Deploy: sam deploy --guided
```

E por último o “sam local start-api”

```bash
sam local start-api 
Mounting HelloWorldFunction at http://127.0.0.1:3000/hello [GET]
You can now browse to the above endpoints to invoke your functions. You do not need to restart/reload SAM CLI while working on your functions, changes will be reflected instantly/automatically. You only need to restart SAM CLI if you update your AWS SAM template
2022-06-16 15:09:15  * Running on http://127.0.0.1:3000/ (Press CTRL+C to quit)
```

Boaaaa! Agora nosso projeto já está rodando na máquina local! Podemos testar a aplicação e desenvolver ela.

Se abrirmos o endereço http://localhost:3000/hello já da para ver nosso hello world!

![Hello World](https://files.catbox.moe/z8rauy.webp)

# Subindo na AWS!
Mas beleza, isso aí eu consigo fazer na minha máquina usando flask de boa… Qual a novidade de usar o AWS Lambda?

Agora que vamos ver a magia! Vamos subir para nuvem! Para fazermos isto precisamos de apenas um comando e seguir o passo a passo do sam!

```bash
❯ sam deploy -g

Configuring SAM deploy
======================

	Looking for config file [samconfig.toml] :  Found
	Reading default arguments  :  Success

	Setting default arguments for 'sam deploy'
	=========================================
	Stack Name [hello]: hello 
	AWS Region [us-east-1]: 
	#Shows you resources changes to be deployed and require a 'Y' to initiate deploy
	Confirm changes before deploy [Y/n]: y
	#SAM needs permission to be able to create roles to connect to the resources in your template
	Allow SAM CLI IAM role creation [Y/n]: y
	#Preserves the state of previously provisioned resources when an operation fails
	Disable rollback [Y/n]: y
	HelloWorldFunction may not have authorization defined, Is this okay? [y/N]: y
	Save arguments to configuration file [Y/n]: n

	Looking for resources needed for deployment:
	 Managed S3 bucket: aws-sam-cli-managed-default-samclisourcebucket-1gd58zudv25f4
	 A different default S3 bucket can be set in samconfig.toml
File with same data already exists at hello/000000000000000000000, skipping upload

	Deploying with following values
	===============================
	Stack name                   : hello
	Region                       : us-east-1
	Confirm changeset            : True
	Disable rollback             : True
	Deployment s3 bucket         : aws-sam-cli-000000000000
	Capabilities                 : ["CAPABILITY_IAM"]
	Parameter overrides          : {}
	Signing Profiles             : {}

Initiating deployment
=====================
File with same data already exists at hello/000000000000000.template, skipping upload

Waiting for changeset to be created..

CloudFormation stack changeset
-------------------------------------------------------------------------------------------------
Operation                LogicalResourceId        ResourceType             Replacement            
-------------------------------------------------------------------------------------------------
+ Add                    HelloWorldFunctionHell   AWS::Lambda::Permissio   N/A                    
                         oWorldPermissionProd     n                                               
+ Add                    HelloWorldFunctionRole   AWS::IAM::Role           N/A                    
+ Add                    HelloWorldFunction       AWS::Lambda::Function    N/A                    
+ Add                    ServerlessRestApiDeplo   AWS::ApiGateway::Deplo   N/A                    
                         aaaaaaaaaa                 aaaaaaaa                                           
+ Add                    ServerlessRestApiProdS   AWS::ApiGateway::Stage   N/A                    
                         tage                                                                     
+ Add                    ServerlessRestApi        AWS::ApiGateway::RestA   N/A                    
                                                  pi                                              
-------------------------------------------------------------------------------------------------

Changeset created successfully. arn:aws:aaaaaaa:us-east-1:-00000000000000


Previewing CloudFormation changeset before deployment
======================================================
Deploy this changeset? [y/N]: y

2022-07-06 18:41:06 - Waiting for stack create/update to complete

CloudFormation events from stack operations
-------------------------------------------------------------------------------------------------
ResourceStatus           ResourceType             LogicalResourceId        ResourceStatusReason   
-------------------------------------------------------------------------------------------------
CREATE_IN_PROGRESS       AWS::IAM::Role           HelloWorldFunctionRole   -                      
CREATE_IN_PROGRESS       AWS::IAM::Role           HelloWorldFunctionRole   Resource creation      
                                                                           Initiated              
CREATE_COMPLETE          AWS::IAM::Role           HelloWorldFunctionRole   -                      
CREATE_IN_PROGRESS       AWS::Lambda::Function    HelloWorldFunction       -                      
CREATE_IN_PROGRESS       AWS::Lambda::Function    HelloWorldFunction       Resource creation      
                                                                           Initiated              
CREATE_COMPLETE          AWS::Lambda::Function    HelloWorldFunction       -                      
CREATE_IN_PROGRESS       AWS::ApiGateway::RestA   ServerlessRestApi        -                      
                         pi                                                                       
CREATE_IN_PROGRESS       AWS::ApiGateway::RestA   ServerlessRestApi        Resource creation      
                         pi                                                Initiated              
CREATE_COMPLETE          AWS::ApiGateway::RestA   ServerlessRestApi        -                      
                         pi                                                                       
CREATE_IN_PROGRESS       AWS::Lambda::Permissio   HelloWorldFunctionHell   Resource creation      
                         n                        oWorldPermissionProd     Initiated              
CREATE_IN_PROGRESS       AWS::ApiGateway::Deplo   ServerlessRestApiDeplo   -                      
                         fasdf                    dasfdafsdf                                 
CREATE_IN_PROGRESS       AWS::Lambda::Permissio   HelloWorldFunctionHell   -                      
                         n                        oWorldPermissionProd                            
CREATE_COMPLETE          AWS::ApiGateway::Deplo   ServerlessRestApiDeplo   -                      
                         asffd                    fsafdsafasd                                 
CREATE_IN_PROGRESS       AWS::ApiGateway::Deplo   ServerlessRestApiDeplo   Resource creation      
                         yment                    fasdfafdfdsafds          Initiated              
CREATE_IN_PROGRESS       AWS::ApiGateway::Stage   ServerlessRestApiProdS   -                      
                                                  tage                                            
CREATE_IN_PROGRESS       AWS::ApiGateway::Stage   ServerlessRestApiProdS   Resource creation      
                                                  tage                     Initiated              
CREATE_COMPLETE          AWS::ApiGateway::Stage   ServerlessRestApiProdS   -                      
                                                  tage                                            
CREATE_COMPLETE          AWS::Lambda::Permissio   HelloWorldFunctionHell   -                      
                         n                        oWorldPermissionProd                            
CREATE_COMPLETE          AWS::CloudFormation::S   hello                    -                      
                         tack                                                                     
-------------------------------------------------------------------------------------------------

CloudFormation outputs from deployed stack
-------------------------------------------------------------------------------------------------
Outputs                                                                                         
-------------------------------------------------------------------------------------------------
Key                 HelloWorldFunctionIamRole                                                   
Description         Implicit IAM Role created for Hello World function                          
Value               arn:aws:iam::000000000000:role/hello-HelloWorldFunctionfasfd-fdsafdasfdfas    

Key                 HelloWorldApi                                                               
Description         API Gateway endpoint URL for Prod stage for Hello World function            
Value               https://fsafdafsdfafds.dfasdf-fdsf.us-east-1.fdsf.com/Prod/hello/          

Key                 HelloWorldFunction                                                          
Description         Hello World Lambda Function ARN                                             
Value               arn:aws:fdsadfasdf:us-fsafd-1:fdsafsadf:function:hello-fdsfasdfdsa-    
KCqA1Uv46PAe                                                                                    
-------------------------------------------------------------------------------------------------

Successfully created/updated stack - hello in us-east-1
```

Vamos passar por cada passo do sam deploy.

O comando “sam deploy -g” quer dizer que vamos fazer um deploy guiado.

“Stack Name” Aqui ele esta perguntando o nome da sua stack que vai estar registrado na aws.

“AWS Region” aqui ele esta pedindo para digitar a região da aws que você quer que fique suas aplicação.

“Confirm changes before deploy” aqui ele quer saber se você vai querer que ele te peça uma confirmação de deploy quando aparecer as alteração. Caso coloque não ele vai pular a confirmação da linha 65.

“Allow SAM CLI IAM role creation” aqui ele pede permissão para criar uma função do SAM CLI IAM.

“Disable rollback” aqui ele esta perguntado que se você quer desativar o rollback, se você não desativar ele vai voltar para a aplicação anterior caso de erro.

“HelloWorldFunction may not have authorization defined, Is this okay” aqui ele esta perguntando se esta tudo bem se a sua função não tiver authorization definido.

“Save arguments to configuration file” aqui ele esta perguntando se você quer salvar um arquivo de configuração.

“Deploy this changeset?” como definimos na linha 14 que queria ter a confirmação antes do deploy, agora ele esta perguntando se pode fazer o deploy.

E pronto! Agora você tem uma aplicação funcionando sem muito custo e sem precisar criar um servidor, mexer com apache, SO… Agora você pode focar somente na sua regra de negócio!

Espero que esse artigo tenha te ajudado a entender mais sobre aws lambda. Obrigado!