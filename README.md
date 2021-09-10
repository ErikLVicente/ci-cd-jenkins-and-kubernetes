#instalar jenkins

$ sudo apt-get update

$ sudo apt-get install openjdk-8-jdk --yes

$ sudo wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | apt-key add -

$ sudo sh -c 'echo deb http://pkg.jemkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

$ sudo apt-get update && apt-get install jenkins --yes

#instalar docker

$ sudo curl -fsSL https://get.docker.com | bash


#privilégios para usuário jenkins

$ sudo usermod -aG docker jenkins

#restart jenkins

$ sudo systemctl retart jenkins

#acessar ip do jenkins e pegar chave para setar password

#instalar plugins sugeridos

#preencher usuário, senha, email, etc.

#adicionar plugins que precisará

#gerenciar jenkins > gerenciar plugins
    => docker, docker pipeline, kubernetes e kubernetes continuous deploy.

#instalar (marcar para reiniciar depois)

#criar credenciais
    >gerenciar jenkins>manage credentials
    >domains> add credetials
    => Docker = tipo > username and password - usuario erikdelimavicente, senha ***, ID = dockerhub, description = dockerhub
    => kubernetes = tipo > secretfile - pegar o arquivo do kubectl (.kube\config), ID = kube, description = kube
    => kubeconfig = tipo > kubenetes configuration - ID = kubeconfig, description = kubeconfig, copiar conteudo do kubeconfig para dentro da box content

#configurar cloud providers
    >gerenciar jenkins>configuração de segurança global>
    => mudar "agents" para "Randomico" - salvar

    > configurar sistema
    => Cloud => "a separate configuration page" > select "kubernetes"> cloud details> 
    => colocar a credentials "config(kube)" > test communication
    => jenkins url: "http://ip:8080
    => Pod labels:
        - key: jenkins
        - value: slave
        - pod retention: on-failure
        - pod template: 
            - name: pod-template
            - labels: kubepod
            - containers: container tamplate
                - name: jnlp
                - Docker image: jenkins/jnlp-slave:latest
                - working directory: /home/jenkins
                - restante em branco
        
        Pod retention: On-failure
        salvar

 #preparar projeto
    => docker file
    => yaml's kubernetes
    => testar

#preparar pipeline
    => entrar no jenkins > novo job > "nome"
    => pipeline > 
        - Github project: "inserir url do github"
        - Pipeline - Definition: "pipeline script from SCM" > 
            - SCM: git
            - Repository url: "url github"
            - credentials: "none" - pq repositório está é aberto nesse caso
            - branch specifier: */master
            - scriptpath: jenkinsfile
            salvar

#incluir jenkinsfile no repositório
    => "jenkinsfile" commit
    ###
    
    pipeline {
        agent any

        stages {
            stage('Teste') {
                steps {
                    echo 'Teste'
                }
            }
        }
    }
    
    ###

#agora rodar no jenkins
    => "Construir agora"
    


