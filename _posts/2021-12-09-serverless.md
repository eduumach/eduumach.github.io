---
layout: post
title: Serverless
subtitle: Vamos entender o que é o serverless
tags: [serverless, aws, lambda, faas, paas, caas, iaas]
comments: true
author: Eduardo M. Rezende
---

# O que é o serverless?
Serveless em tradução livre para o português é “Sem servidor”, mas aliás o que isso quer dizer? Sem servidor, onde isso vai ficar? Será que agora estamos na nuvem mesmo, sem um servidor? hahaha

Bom não é bem que assim que funciona, serverless é uma tecnologia que não precisa fazer uma configuração no servidor, como assim, Eduardo? Para isso vamos entender como funciona um servidor.

# Como funciona os servidores na web?
Todos servidores precisam de um Hardware(nada mais do o que você chuta, a parte física do pc). Depois ele precisa de um sistema operacional(a parte que tu xinga, porque não tem como chutar algo que não é físico haha). Com isso ele tem um sistema de virtualização e um servidor web(nginx, apache…) e com isso chegamos na amada aplicação, com códigos, o que todo programador ama.

Nossa que trabalheira configurar isso tudo né? Já é difícil escolher um pc para comprar imagina configurar tudo isso? Aí que entra o maravilho serverless!

# Serverless como funciona?
Vamos eliminar todas essa trabalheira ai atrás de montar e configurar um servidor, como assim? Aqui que entra o tipos de arquiteturas serveless!

# IaaS
Com o IaaS ele elimina a montagem do hardware e a instalação do sistema operacional. Te entrega um sistema sem nada, tendo que manter o ambiente, servidor web, a aplicação e as regras de negócios. Exemplos de IaaS: AWS EC2, Microsoft
Azure, Google Compute Engine.

# CaaS
O CaaS tem quase as mesmas vantagens que o IaaS, a diferença que no lugar de ter um Sistema você vai usar Containers(Docker, kubernetes…), isso quer dizer que seu app pode ter vários, vários containers e rodar eles só quando necessários e sendo auto escaláveis. Mas mesmo assim você vai ter que manter o ambiente, servidor web, a aplicação e as regras de negócios.

# PaaS
Agora com o PaaS ele já te entrega o container e o ambiente configurado, sendo preciso só subir seu código da aplicação e falar para executar aquele comando que é para iniciar a aplicação. Exemplos: Heroku, Google App, Image, AWS Beanstalk, Dokku

# FaaS
Com o FaaS você só precisa criar pequenas funções e focar somente na regra de negocio:

```python
def lambda_handler(event, context):
     return {'message': 'hello world'}
```
<br />
<iframe src="https://giphy.com/embed/TIRlx3Fzi1A7L2d5z7" width="480" height="267" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/TIRlx3Fzi1A7L2d5z7">via GIPHY</a></p>

Isso é uma função do aws lambda, ele mostra na tela essa mensagem, sem precisar configurar e manter um servidor. Você só foca no código e não em como ele vai rodar, aumentando a produção, agilidade e ainda pagando só pelo tempo de execução.

# Fim…
Espero ter ajudado a entender como o serverless funciona e ter tirado algumas duvidas! Qual será a próxima evolução do serveless, vamos ter que falar só o que queremos para o servidor fazer determinada coisa e ele já resolve tudo para gente? Vamos ver kkkkkk