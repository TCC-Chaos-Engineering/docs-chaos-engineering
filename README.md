# CHAOS ENGINEERING

## Definições Básicas

*"É a disciplina de realizar experimentos em um sistema distribuido afim de criar confiança na capacidade desse sistema para suportar condições turbulentas na produção."*

*"É um método de experimentação em infraestrutura que traz os pontos fracos do sistema à luz. Este processo empírico de verificação nos leva a ter sistemas mais resilientes e gera confiança no comportamento operacional desses sistemas."*

*"A Engenharia do Caos se utiliza de experimentos para aprender mais sobre um sistema em particular, descobrindo fraquezas e tornando-o mais resiliente com as correções aplicadas."*

Casey Rosenthal

## Teste e Experimentação

* **Teste** - Sobre condições específicas o sistema irá apresentar um resultado específico, geralmente baseado em verdadeiro ou falso.

* **Experimentação** - Gera um novo conhecimento e frequentemente sugere um novo caminho de exploração.

## Princípios do Caos

* http://principlesofchaos.org

## Simian Army

* **Chaos Monkey**, o primeiro dos integrantes. Tem o objetivo de, aleatoriamente, desabilitar instâncias em produção para que se possa certificar que esse tipo de falha não impacte o usuário. O nome é uma referência bem interessante: Imagine um macaco selvagem com uma arma solto em um datacenter. Pense no potencial destrutivo.

* **Latency Monkey** propositalmente aumenta a latência em respostas dos serviços REST possibilitando testar a recuperação à indisponibilidade dos serviços.

* **Conformity Monkey** limpa tudo aquilo que não está dentro das melhores práticas, como por exemplo uma instância que não tenha o auto-scaling configurado.

* **Doctor Monkey** constantemente faz health checks nas instâncias para detectar problemas de memória, throughput elevado, CPU alto etc.

* **Janitor Monkey** simplesmente procura por recursos não utilizados e os desliga.

* **Security Monkey** é uma extensão do Conformity Monkey. Enquanto este se preocupa com o auto-scaling, por exemplo, o Security Monkey se preocupa com problemas de segurança, como instâncias sem configuração de security group e, ao encontrar, desliga essas instâncias que poderiam ser a origem de problemas. Inclusive checa os certificados SSL e DRM com o objetivo de confirmar se estão válidos e se não estão próximos de expirar.

* **10–18 Monkey** checa configuração e problemas em runtime de instâncias que estejam servindo usuários em diferentes regiões, com diferentes idiomas e charsets.

* **Chaos Gorilla** é uma evolução do Chaos Monkey que, pois dentro de uma região, desabilita zonas para verificar se o sistema continua funcionando sem impacto para o usuário.

* **Chaos Kong** foi o último símio liberado e é uma evolução do Chaos Gorilla. Ele destrói uma região inteira da AWS para se certificar de que haverá um redirecionamento do fluxo para outra evitando assim que o usuário seja impactado.

## Chaos Monkey

O Chaos Monkey na sua versão atual trabalha com uma ferramenta Open-Source de entrega contínua chamada Spinnaker.

As aplicações devem ser gerenciadas pelo Spinnaker para que seja possível 'derrubar' as instâncias através do Chaos Monkey.

## Tecnologias

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

*"Transforme o fracasso em resiliência. O Gremlin oferece a você a estrutura para simular interrupções reais com segurança e com uma crescente biblioteca de ataques. Usando Chaos Engineering para melhorar a resiliência do seu sistema, o “Failure as a Service” do Gremlin torna mais fácil encontrar pontos fracos antes que eles causem problemas aos seus clientes."*

Documentação do Gremlin

## Gremlin na prática

### Configuração

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

CPU ATTACK

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

## Links úteis

* https://www.oreilly.com/library/view/chaos-engineering/9781491988459/

* https://github.com/dastergon/awesome-chaos-engineering

* https://www.goodreads.com/book/show/35516296-chaos-engineerin* 

* https://www.infoq.com/br/news/2012/08/netflix-chaos-monkey

* https://imasters.com.br/cloud/chaos-monkey-da-netflix-e-atualizado-veja-o-que-muda

* https://medium.com/@bfpimenta/o-simian-army-6a7c138460d* 

* https://www.infoq.com/br/articles/engenharia-de-caos

* https://steemit.com/steemit/@bitcoincwb/o-engenharia-do-caos-o-que--isso

* https://jccavalcanti.wordpress.com/2018/05/07/a-engenharia-do-caos/

* https://engenhariae.com.br/editorial/engenharia-em-pauta/engenharia-e-desordem-o-caos

* https://www.nucleodoconhecimento.com.br/engenharia-de-producao/gerenciamento-dos-riscos

* https://gestaltit.com/exclusive/kenwith-gremlin-chaos-engineering-is-not-just-for-the-hyperscalers-anymore/

* https://medium.com/trainingcenter/docker-o-que-%C3%A9-docker-e-como-come%C3%A7ar-58e04bdcb043

* https://www.devmedia.com.br/docker/

* https://www.gremlin.com/docs/

* https://www.gremlin.com/chaos-monkey/

* https://www.gremlin.com/chaos-monkey/chaos-monkey-alternatives/

* https://codecentric.github.io/chaos-monkey-spring-boot/#docs

* http://principlesofchaos.org/

* https://opensource.com/article/17/8/testing-production

* https://blog.codecentric.de/en/2018/07/chaos-engineering/

* https://www.gremlin.com/blog/chaos-engineering-is-not-just-tools-its-culture/

* https://sharpend.io/the-limitations-of-chaos-engineering/

* https://sharpend.io/the-limitations-of-chaos-engineering/


