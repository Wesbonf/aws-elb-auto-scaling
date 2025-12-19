# Laboratório AWS ELB e Auto Scaling – Arquitetura Escalável

## 📌 Visão Geral
Este laboratório demonstra a criação de uma arquitetura escalável e altamente disponível na AWS utilizando **Elastic Load Balancing (ELB)** e **EC2 Auto Scaling**. O objetivo é distribuir o tráfego de forma eficiente entre múltiplas instâncias EC2 e ajustar automaticamente a capacidade da infraestrutura de acordo com a demanda.

---

## 🎯 Objetivos do Laboratório
Ao final deste laboratório, foi possível:

- Criar uma **Amazon Machine Image (AMI)** a partir de uma instância EC2 existente  
- Configurar um **Application Load Balancer (ELB)** para distribuição de tráfego  
- Criar um **Launch Template** para padronizar a criação de instâncias  
- Implementar um **Auto Scaling Group** com políticas de escalabilidade automática  
- Configurar **alarmes no Amazon CloudWatch** para monitoramento de desempenho  
- Validar o funcionamento do balanceamento de carga e do Auto Scaling  

## 🏗️ Arquitetura Utilizada
A arquitetura do laboratório é composta pelos seguintes componentes:

- **Amazon VPC (Lab VPC)**  
- **Instâncias EC2** distribuídas em sub-redes privadas  
- **Application Load Balancer (ELB)** em sub-redes públicas  
- **Auto Scaling Group** com capacidade mínima, desejada e máxima  
- **Amazon CloudWatch** para métricas e alarmes  

O ELB recebe o tráfego da internet e o distribui entre as instâncias EC2, enquanto o Auto Scaling ajusta automaticamente a quantidade de instâncias conforme a utilização de CPU.


## 1️⃣ Criação da AMI para Auto Scaling

### 📌 Detalhes da AMI
- **Nome da AMI:** WebServerAMI  
- **Descrição:** Lab AMI for Web Server  

### ℹ️ Observação
Essa AMI foi utilizada posteriormente pelo **Auto Scaling Group** para criar novas instâncias automaticamente.

![Criação da AMI](images/1.PNG)

---
## 2️⃣ Criação do Grupo de Destino

Foi criado um **Target Group** para definir o destino do tráfego encaminhado pelo **Load Balancer**.

### 📌 Configurações
- **Nome:** LabGroup  
- **Tipo de destino:** Instâncias  
- **VPC:** Lab VPC  

### ℹ️ Função do Target Group
O grupo de destino é responsável por:
- Realizar **health checks**  
- Encaminhar o tráfego apenas para **instâncias saudáveis**


#### Nome e tipo de destino
![Nome e tipo de instância](images/2.PNG)

#### VPC associada
![VPC](images/3.PNG)

#### Resumo da configuração
![Resumo da configuração](images/4.PNG)


## 3️⃣ Criação do Application Load Balancer (ELB)

Foi configurado um **Application Load Balancer (ALB)** para distribuir o tráfego HTTP entre as instâncias EC2.

### 🖼️ Visão geral
![Application Load Balancer](images/5.PNG)

### 📌 Configurações
- **Nome:** LabELB  
- **Tipo:** Internet-facing  
- **VPC:** Lab VPC  
- **Sub-redes:**  
  - Sub-rede pública 1  
  - Sub-rede pública 2  


![Configurações](images/6.PNG)  
![Configurações](images/7.PNG)

### 🔐 Segurança e Listener
- **Grupo de segurança:** Web Security Group  
- **Listener:** HTTP (porta 80)  
- **Ação padrão:** Encaminhar para o grupo de destino `LabGroup`  

![Segurança](images/8.PNG)


## 4️⃣ Criação do Launch Template

Foi criado um **Launch Template** para padronizar as instâncias do Auto Scaling.

### Configurações
- **Nome:** LabConfig  

![Nome](images/12.PNG)

- **AMI:** WebServerAMI  
- **Tipo de instância:** t2.micro  

![AMI, tipo de instância e par de chaves](images/10.PNG)

- **Par de chaves:** vockey  
- **Grupo de segurança:** Web Security Group  

![Par de chaves e grupo de segurança](images/11.PNG)

- **Monitoramento detalhado do CloudWatch:** habilitado  

![CloudWatch](images/9.PNG)


## 5️⃣ Criação do Auto Scaling Group

O **Auto Scaling Group** foi criado utilizando o Launch Template configurado anteriormente.

### Configurações Gerais

- **Nome:** Lab Auto Scaling Group  
- **VPC:** Lab VPC  
- **Sub-redes:**  
  - Sub-rede privada 1  
  - Sub-rede privada 2  

  ![Nome](images/13.PNG)
  ![VPC e sub-redes](images/14.PNG)

### Capacidade

- **Capacidade desejada:** 2  
- **Capacidade mínima:** 2  
- **Capacidade máxima:** 6  
![Capacidades](images/15.PNG)


### Política de Escalabilidade

- **Nome:** LabScalingPolicy  
- **Métrica:** Utilização média da CPU  
- **Valor alvo:** 60%  

![escalabilidade](images/16.png)

O Auto Scaling ajusta automaticamente o número de instâncias para manter a CPU média próxima ao valor definido.


### Balanceamento e Métricas

- **Load Balancer:** LabELB  
- **Target Group:** LabGroup  
- **Coleta de métricas do grupo:** habilitada  

