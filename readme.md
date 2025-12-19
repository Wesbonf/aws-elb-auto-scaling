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


