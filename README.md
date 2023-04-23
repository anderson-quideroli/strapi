<h1 align="center"> PROJETO STRAPI CMS - v4.9.1 </h1>
<h4 align="center"> 
    :white_check_mark:  Projeto concluído  :white_check_mark:
</h4>

### Tópicos 

- [Strapi](#strapi)

- [Ferramentas utilizadas](#ferramentas-utilizadas)

- [Pré-requisitos](#pré-requisitos)

- [Arquitetura](#arquitetura)

- [Topologia Kubernetes Local](#topologia-kubernetes-local)

- [Topologia Strapi](#topologia-strapi)

- [Repositorios](#repositórios)

- [Execução](#execução)

- [Acesso ao projeto](#acesso-ao-projeto)
 
- [Autor](#autor)


## 💽 Strapi

Strapi é um CMS headless de código aberto que oferece aos desenvolvedores a liberdade de escolher suas ferramentas e estruturas favoritas e permite que os editores gerenciem e distribuam seu conteúdo usando o painel de administração do aplicativo. Baseado em um sistema de plug-in, Strapi é um CMS flexível cujo painel de administração e API são extensíveis - e cada parte é personalizável para corresponder a qualquer caso de uso. O Strapi também possui um sistema de usuário integrado para gerenciar em detalhes o que os administradores e usuários finais têm acesso.

Documentação: [Strapi DOCv4](https://docs.strapi.io/dev-docs/intro)

## 🛠 Ferramentas utilizadas

1. Docker - v4.18.0
2. Node.js - v16
3. Yarn - v1.22.19
4. Strapi - v4.9.1
5. Kubernetes - v1.27.1

## 🧩 Pré-requisitos

1. Criar um cluster Kubernetes de preferencia com 2 nodes worker e 1 master executando a control-plane.
2. Instalar o balanceador Metallb no cluster.
3. Configurar o ConfigMap para o balanceador poder atribuir endereços IP aos serviços que irão utilizar o balanceador Metallb.
4. Instalar o Nginx-ingress e alterar de NodePort para LoadBalancer.

## 📏 Arquitetura

Para esse projeto, foi criado um cluster Kubernetes contendos 2 nodes worker e 1 master executando a control-plane.
A principio o projeto seria realizado utilizando o Minikube, mas devido algumas limitações da ferramenta com o load balancer e Nginx Ingress, foi decidido criar um cluster kubernetes, pois simularia melhor um ambiente produtivo.

O Cluster foi instalado e configurado com o balanceador Metallb e alterado o Nginx-ingress para trabalhar com o tipo LoadBalancer ao invés de Nodeport, desta forma ele receberia um IP atribuido pelo balanceador e se tornaria acessivel na rede.

A aplicação Strapi trabalhar na porta 1337/TCP, ela será exposta como serviço ClusterIP para que ela só possa ser acessada de dentro do cluster na namespace strapi, O Nginx-ingress ira efetuar o trabalho de proxy reverso e rotear o trafego para o serviço exposto da aplicação na porta 1337/TCP.

O ingress está configurado para efetuar o redirect da porta 80/TCP para 443/TCP utilizando certificado SSL/TLS com a URL - https://strapi.quideroli.local.

## 📐 Topologia Kubernetes Local:

<div allign="center">
<img src="https://user-images.githubusercontent.com/127318593/233867632-e272b9b5-f885-4989-9eef-139c86040e59.JPG" width="700px" />
</div>

## 📐Topologia Strapi:

<div allign="center">
<img src="https://user-images.githubusercontent.com/127318593/233867656-dcc74cc7-6dd0-4b4e-83a2-ddb35dae34be.JPG" width="700px" />
</div>

## 📁 Repositórios

Docker Hub: [quideroli/strapi](https://hub.docker.com/r/quideroli/strapi)

## 🛠️ Execução

Para executar esse projeto, efetua o clone do repositorio no cluster ou na estação de trabalho que posssua acesso ao cluster kubernetes e execute os manifestos kubernetes localizado no diretório:

`kubectl apply -f manifestos_kubernetes/`
 
Para deletar esse projeto efetue o comando abaixo:

`kubectl delete -f manifestos_kubernetes/`

Após executar os manifestos será criado o serviço do tipo ingress, colete o ip atribuido pelo load balancer Metallb para o serviço da controller do ingress, e crie uma nova entrada no arquivo hosts do windows ou Linux:

Execute o comando abaixo e copie o endereço IP do campo External-IP:
`kubectl get svc -n ingress-nginx`

"IP da controller do ingress" strapi.quideroli.local

- Windows:

Path: **C:\Windows\System32\drivers\etc\hosts**

- Linux:

Path: **/etc/hosts**

## 📁 Acesso ao projeto

Você pode [acessar o código fonte do projeto inicial](https://github.com/strapi/strapi) ou [baixá-lo](https://github.com/strapi/strapi/archive/refs/heads/main.zip).

## 👦 Autor
Anderson Dias de Jesus Quideroli

LinkedIn: https://www.linkedin.com/in/anderson-dias-de-jesus-quideroli-625049b2/
