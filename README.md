# CHAOS ENGINEERING

## Chaos Monkey
O Chaos Monkey na sua versão atual trabalha com uma ferramenta Open-Source de entrega contínua chamada Spinnaker.

As aplicações devem ser gerenciadas pelo Spinnaker para que seja possível 'derrubar' as instâncias através do Chaos Monkey.

### Spinnaker
* [Site oficial](https://www.spinnaker.io/)
* [Instalação](https://www.spinnaker.io/setup/install/)

### AWS
* [Site oficial](https://aws.amazon.com/pt/)

### Docker
Tecnologia que permite a criação de contêineres para uma camada adicional de virtualização permitindo que um programa rode em qualquer sistema através do contêiner do Docker e sendo mais leve que uma VM.
* [Site oficial](https://www.docker.com/)

### Kubernetes
Sistema de orquestração de contêineres que automatiza a implatação, o dimensionamento e a gestão de aplicações em containers.
* [Site oficial](https://kubernetes.io/)

### Minikube
Ferramenta para rodar o Kurbenetes localmente, executando um único 'node' rodando dentro de uma VM na máquina local.
* [Instalação](https://kubernetes.io/docs/tasks/tools/install-minikube/)

### Chaos Monkey for Spring Boot
Inspirado no Chaos Monkey, roda no Java e possui métodos capazes de executar mais de um tipo de teste no sistema.
* [Repositório no GitHub](https://github.com/codecentric/chaos-monkey-spring-boot)

## Gremlin

### Na prática

O Gremlin não roda nativamente no Windows. Roda somente no Linux, contêineres e na nuvem, mas é possível programar os ataques pelo navegador em qualquer sistema operacional e em qualquer dispositivo com acesso à internet.

Caso necessite, faça a instalação do Oracle VirtualBox no link abaixo:
* [Oracle VirtualBox](https://www.oracle.com/technetwork/pt/server-storage/virtualbox/downloads/index.html)

Abaixo segue o link para download da imagem (.iso) do Linux Ubuntu no Google Docs.
* [Linux Ubuntu](https://drive.google.com/file/d/1G5d0h_sG2dMCz-swc2UZBiIx_j8mqkk8/view)

Após o download e instalação do VirtualBox e Ubuntu, o próximo passo é a instalação do Gremlin.

Antes de qualquer procedimento de instalação, é necessário se cadastrar no site do Gremlin através do link abaixo:

* [Cadastro Gremlin](https://app.gremlin.com/signup)

É necessário executar o comando abaixo para a instalação do Gremlin no Ubuntu.

```
apt-transport-https
```

Execute o comando abaixo colocando o seu usuário cadastrado no Gremlin e o IP do servidor que será atacado.

```
ssh usuario@ip_servidor
```

```
echo "deb https://deb.gremlin.com/ release non-free" | sudo tee /etc/apt/sources.list.d/gremlin.list
```

Execute o comando abaixo para importar a chave GPG.

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C81FC2F43A48B25808F9583BDFF170F324D41134 9CDB294B29A5B1E2E00C24C022E8EF3461A50EF6
```

Execute o comando abaixo para instalar o 'client' e o 'daemon' do Gremlin.

```
sudo apt-get update && sudo apt-get install -y gremlin gremlind
```

Após ter criado a conta no site do Gremlin, você receberá um e-mail de verificação com um link para configuração da sua conta. Nessa configuração você receberá um 'Team ID' e uma 'Secret Key'. Estas duas chaves são necessárias para adicionar novos 'hosts' que serão alvos dos ataques. É importante ressaltar que cada usuário de um mesmo time possui uma 'Secret Key' diferente.

Use o comando abaixo para iniciar o Gremlin:

```
gremlin init
```

Neste momento já é possível criar ataques usando o Gremlin.

### Criando ataques

CPU ATTACK - HELLO WORLD

* Faça o login da sua conta no site Gremlin e clique em [Create Attack.](https://app.gremlin.com/attacks/new)

* Na guia 'Choose the Targets' escolha o host que foi configurado para ser atacado. É possível marcar a opção 'Random' para uma escolha aleatória, caso estejam cadastrados vários hosts.

* Na guia 'Choose a Gremlin' é possível escolher uma de três categorias disponíveis. São elas 'Resource', 'State' e 'Network'. Cada categoria possui uma lista de ataques de acordo com cada perfil. Na versão 'free' (versão que estamos utilizando) estão disponíveis dois ataques: CPU na categoria 'Resource' e 'Shutdown' na categoria 'State'. Vamos escolher CPU da categoria 'Resource'.

* Na opção 'Length' é possível definir quanto tempo (em segundos) a CPU será atacada. Em 'Cores' é possível definir a quantidade de núcleos da CPU que serão atacados. Em 'CPU Capacity' é possível definir a porcentagem de consumo de CPU em cada núcleo. Iremos definir 120 para 'Length' e 1 para 'Cores', visto que a opção 'CPU Capacity' não está disponível na versão free.

* Na guia 'Run the attack' é possível executar o ataque ou agendá-lo. Iremos executar o ataque sem agendar.

* Clique em 'Unleash Gremlin' para executar o ataque.

* Após ter programado o ataque no console do Gremlin no site, é possível verificar o impacto no processador causado pelo Gremlin no Linux Ubuntu usando o comando abaixo.

```
top
```





