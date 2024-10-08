# Arquitetura de Sistema Logistica Reversa
 IT_Architecture_Design-Styles - Apresentação final

# Sistema de Logística Reversa Sustentável

##  Storytelling sobre o problema 

Nos dias atuais, um dos grandes desafios enfrentados pelos e-commerces é a **logística reversa**. Com o aumento do consumo, o retorno de produtos não desejados ou defeituosos tornou-se uma preocupação crescente. O processo de devolução envolve não apenas **custo financeiro**, mas também **impacto ambiental**, devido ao transporte e ao desperdício de materiais.

Nossa solução tem como objetivo criar um sistema de logística reversa **eficiente e sustentável**, que minimize o impacto ambiental e garanta que os produtos retornados sejam **reaproveitados** ou **reciclados**.

---

##  O que esperamos aprender com esse projeto?

Esperamos aprender como:
- **Otimizar** a logística reversa de e-commerces de forma sustentável;
- **Reduzir custos operacionais**;
- **Minimizar o impacto ambiental**;
- Entender como **escalar a solução** para diferentes tamanhos de e-commerces.

---

##  Perguntas que precisam ser respondidas

- Como a solução se **integrará** a diferentes plataformas de e-commerce?
- Quais serão os **custos de implementação e operação** para diferentes tipos de e-commerces?
- Como será o **modelo de precificação baseado no uso** (volume de devoluções)?
- Qual o **impacto ambiental** que essa solução pode gerar a longo prazo?

---

##  Principais Riscos

- **Falta de adesão** por parte dos e-commerces, principalmente os menores.
- **Dificuldade de integração** com sistemas legados de e-commerces mais antigos.
- **Custos operacionais elevados** no início da implementação.
- **Resistência dos clientes** em aceitar soluções de devolução que envolvam processos adicionais de reciclagem.

---

##  Plano para Reduzir Riscos

- Integração **API simplificada** com as plataformas de e-commerce mais comuns, como **Shopify, Magento e WooCommerce**.
- Oferta de um **MVP** (Produto Mínimo Viável) para empresas menores, garantindo um custo inicial mais acessível.
- **Campanhas de conscientização ambiental** para incentivar a adoção da solução.
- **Parcerias** com empresas de logística e reciclagem para otimizar custos.

---

##  Partes Interessadas

- **E-commerces** que buscam uma solução sustentável para suas devoluções.
- **Empresas de logística** que irão coletar e processar os itens devolvidos.
- **Empresas de reciclagem e reaproveitamento** que serão responsáveis por transformar ou reutilizar os itens devolvidos.
- **Consumidores finais** que utilizarão a plataforma de e-commerce e poderão acompanhar o ciclo de vida dos produtos devolvidos.

---

##  O que eles esperam ganhar?

- **E-commerces**: 
  - Redução de **custos logísticos**;
  - Cumprimento de **regulamentações ambientais**;
  - Melhoria da **imagem de marca**;
  - **Fidelização de clientes**.
  
- **Empresas de logística**: 
  - Acesso a novos **contratos e serviços** relacionados ao transporte de devoluções sustentáveis.
  
- **Empresas de reciclagem**: 
  - Maior volume de materiais para reaproveitamento, contribuindo para a **economia circular**.
  
- **Consumidores**: 
  - **Facilidade** no processo de devolução;
  - **Transparência** no impacto ambiental;
  - Potencial participação em programas de fidelidade relacionados à **sustentabilidade**.

---

##  Quem são os usuários?

- **Pequenos e médios e-commerces** que precisam de uma solução de devolução **escalável**.
- **Grandes marketplaces** que buscam uma solução **customizável** e com **relatórios avançados**.
- **Consumidores** que querem devolver produtos de maneira **eficiente** e **ecologicamente correta**.

---

##  O que eles estão tentando realizar?

- **E-commerces**: 
  - **Otimizar** o processo de devolução;
  - **Reduzir custos**;
  - **Melhorar a imagem** perante o público quanto à sustentabilidade.
  
- **Consumidores**: 
  - Um processo de devolução **simples**, **transparente** e com **menor impacto ambiental possível**.

---

##  Qual o pior cenário?

- O **fracasso** do sistema de integração, gerando **insatisfação** dos e-commerces e consumidores.
- Isso pode resultar em **perda de mercado** e aumento de **custos operacionais**.

---

##  Arquitetura (Modelo Freeform - Versão inicial)

![integração entre o ERP_WMS do e-commerce e a solução SaaS de logística reversa sustentávelF](https://github.com/user-attachments/assets/eaf4b065-5154-4202-9bc4-d0ec9ef82616)



---

## Descrição de Cada Componente do Diagrama

### Componentes do Sistema de Gestão de Devoluções

1. **Cliente**: Representa o usuário final que inicia o processo de devolução por meio da plataforma de e-commerce.
2. **Plataforma e-commerce**: Sistema que interage diretamente com o cliente, gerenciando o front-end da operação de vendas e devoluções.
3. **ERP/WMS (Sistema de Gestão de Pedidos e Estoque)**: Sistema que gerencia os pedidos e o estoque da empresa, comunicando-se com o e-commerce para manter as informações atualizadas. Recebe as requisições de devolução (HTTP) e envia atualizações de status via webhook.
4. **Firewall**: Componente de segurança que protege a comunicação entre os sistemas internos e externos, filtrando e controlando o tráfego de dados.
5. **API Gateway**: Responsável por gerenciar as APIs, roteando as solicitações para os serviços apropriados dentro do ambiente de containers. Atua como um ponto de entrada unificado.
6. **MicroServiço Autenticação**: Realiza a autenticação dos usuários e serviços, garantindo que apenas requisições autorizadas possam acessar o sistema.
7. **MicroServiço Gestão de Devoluções**: Gerencia o fluxo de devolução dos produtos, interagindo com o orquestrador de Saga para coordenar os microserviços envolvidos no processo.
8. **Orquestrador Saga**: Gerencia as transações distribuídas de forma assíncrona, garantindo a consistência entre os diferentes microserviços que participam do fluxo de devolução.
9. **Broker (Fila de Mensagens)**: Facilita a comunicação entre os microserviços através do padrão Publish-Subscribe, permitindo a troca de mensagens de forma desacoplada.
10. **Canal Resposta**: Responsável por consolidar e gerenciar as respostas dos diferentes microserviços, retornando o status das operações ao sistema de origem.
11. **MicroServiço Rastreamento**: Realiza o rastreamento dos itens devolvidos, confirmando o status do retorno do produto.
12. **MicroServiço Logística**: Gerencia a logística da devolução, coordenando as atividades de transporte e recebimento dos itens.
13. **MicroServiço Reciclagem**: Administra o processo de reciclagem dos itens devolvidos, avaliando se o item é elegível para reciclagem e gerenciando o fluxo de reciclagem confirmada ou não.
14. **MicroServiço Notificações**: Envia notificações para os usuários e sistemas sobre o status das devoluções e outras atualizações relevantes.
15. **MicroServiço Relatórios**: Coleta dados para geração de relatórios gerenciais e operacionais, apoiando na tomada de decisões.
16. **Serviço de Coleta de Logs**: Coleta logs de operação e eventos dos microserviços para análise e auditoria.
17. **Service Mesh**: Gerencia a comunicação entre os microserviços, oferecendo recursos como balanceamento de carga, autenticação e monitoramento de tráfego.
18. **Serviço de Monitoramento de Infraestruturas**: Monitora a performance e a saúde dos serviços, garantindo que os sistemas estejam operacionais.
19. **Kubernetes**: Escalabilidade horizontal dos microsserviços, onde novas instâncias podem ser adicionadas conforme necessário, garantindo resiliência do sistema.
20. **Bancos de Dados**: Cada microserviço possui seu banco de dados dedicado, garantindo independência e integridade dos dados.

---

## Requisitos Importantes

- **Escalabilidade**: O sistema deve ser capaz de escalar automaticamente com o aumento de volume de devoluções e produtos processados.
- **Resiliência**: O sistema deve ser capaz de lidar com falhas sem afetar todo o processo.
- **Segurança**: A autenticação e autorização são cruciais para proteger os dados dos consumidores e das empresas.
- **Integração via APIs**: A solução precisa de APIs bem documentadas e robustas para se integrar com sistemas de e-commerce e ERP/WMS.
- **Monitoramento e Logs**: É importante ter um sistema de monitoramento e logs centralizado para rastrear cada requisição e identificar falhas.

---

## Sobre o que o Diagrama/Arquitetura Ajuda a Raciocinar/Pensar?

A arquitetura ajuda a pensar sobre como os diferentes microserviços podem operar de forma independente e ao mesmo tempo colaborar para fornecer uma solução integrada. Ela também destaca a importância de centralizar a comunicação via API Gateway, mantendo a segurança e a escalabilidade em mente.

---

## Padrões Essenciais no Diagrama/Arquitetura

### Principais Tecnologias e Padrões

- **Microserviços**: Arquitetura baseada em microserviços permite a escalabilidade, desenvolvimento e manutenção independente de cada componente.
- **Saga Pattern**: Orquestrador Saga gerencia a consistência das transações distribuídas, importante para operações complexas e sequenciais.
- **Publish-Subscribe (Broker)**: Desacopla os microserviços, facilitando a comunicação entre eles de forma assíncrona.
- **Service Mesh**: Introduz um padrão para a gestão de tráfego entre microserviços, adicionando resiliência e segurança.
- **API Gateway**: Centraliza a entrada de requisições, roteando e controlando o acesso aos microserviços.

---

## Existem Padrões Ocultos?

### Padrões de Resiliência

- **Circuit Breaker**:  Implementado dentro do Service Mesh para evitar falhas em cascata quando um serviço falha repetidamente.
- **Retry Pattern**: Está embutido no orquestrador Saga e nos microserviços para tentar novamente uma operação falha, garantindo maior resiliência.

  
---

## Metamodelo

Arquitetura de Aplicações:

Compreende Componentes de Aplicação (microsserviços), Serviços de Aplicação (autenticação, gestão de devoluções), e Interações de Aplicação (comunicação entre microsserviços via APIs e publish-subscribe).

---

## Pode ser Discernido no Diagrama/Arquitetura Única?

Sim, apesar da arquitetura ser modular (com microserviços e diferentes camadas), ela é organizada e orquestrada para funcionar como uma solução única, voltada para um propósito específico.

---

## O Diagrama Está Completo?

O diagrama cobre os principais componentes e fluxos da solução SaaS de logística reversa sustentável.No entanto, ele poderia incluir mais detalhes de segurança, escalabilidade.

---

## Poderia ser Simplificado e Ainda Assim Ser Eficaz?

Sim, o diagrama poderia ser simplificado, mas isso poderia impactar a visibilidade das interações mais complexas (como tópicos do kafka).

---

## Discussão Importante na Equipe

Uma discussão importante foi, como garantir a escalabilidade e a resiliência do sistema? Especialmente em momentos de pico de devoluções e aumento de volume de dados.

---

## Decisões com Dificuldade

A decisão sobre usar ou não mensageria assíncrona, já que nem todos os microserviços podem precisar disso, mas a mensageria aumenta a resiliência do sistema.

---

## Decisões Tomadas Sob Incerteza

A implementação de Webhooks em vez de consultas contínuas

---

## Ponto de Decisão sem Retorno

A escolha de Kubernetes como plataforma de orquestração de microserviços foi um ponto sem retorno, uma vez que o investimento nessa tecnologia torna inviável reverter para opções mais simples como Docker Swarm.

---

## Arquiteturas do projeto em camadas do C4

   ## Nível Contexto

![contextoSaas](https://github.com/user-attachments/assets/4eb8f2d7-d920-4530-a9c6-6a8216193e9e)

---

  ## Nível Container
  
![containerNovo](https://github.com/user-attachments/assets/2dd47823-b312-475b-ab46-52636072584e)

---

 ## Nível Componente
 
 ![componentesSass](https://github.com/user-attachments/assets/e87014c0-9d61-4877-a24e-daf93494dcd3)
