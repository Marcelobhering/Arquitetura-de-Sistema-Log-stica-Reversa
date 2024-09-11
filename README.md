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

![image](https://github.com/user-attachments/assets/ad037ffa-5a0a-4576-a870-37db150c8545)

---

## Descrição de Cada Componente do Diagrama

 ### ERP/WMS (Sistema de Gestão de Pedidos e Estoque)
- **Função**: Representa o sistema de gestão de pedidos e estoques utilizado pelo e-commerce. Ele gerencia a criação de ordens de compra, controle de estoque e solicitações de devolução.
- **Integração**: O ERP/WMS se comunica com o API Gateway da solução SaaS para fazer solicitações de devolução, receber atualizações de status via Webhooks, e obter informações sobre o ciclo de vida das devoluções.

 ### API Gateway
- **Função**: Atua como o ponto central de comunicação entre o ERP/WMS e os microserviços da solução SaaS. Ele recebe as requisições HTTP (REST APIs) e envia os dados para os microserviços corretos. Também é responsável por disparar Webhooks para atualizar o ERP/WMS sobre o status das devoluções.
- **Segurança**: Gerencia autenticação, rate-limiting, e pode armazenar logs e métricas de requisições.

 ### MicroServiço de Autenticação
- **Função**: Responsável por autenticar e autorizar o acesso dos usuários do sistema (tanto o e-commerce quanto os consumidores), garantindo que apenas usuários válidos possam interagir com os serviços.
- **Banco de Dados**: Armazena informações de usuários, sessões e tokens de autenticação.

 ### MicroServiço de Gestão de Devoluções
- **Função**: Gerencia o ciclo de vida completo de uma devolução, desde o momento em que ela é solicitada até seu processamento e finalização (seja reciclagem, reaproveitamento ou outro).
- **Banco de Dados**: Mantém registros detalhados de cada devolução, incluindo status, datas e motivo da devolução.

 ### MicroServiço de Rastreamento
- **Função**: Rastreia a movimentação dos produtos devolvidos durante o processo logístico. Ele coleta e armazena atualizações de status ("coletado", "em trânsito", "entregue", etc.) e informa tanto o e-commerce quanto o consumidor sobre o progresso.
- **Banco de Dados**: Armazena eventos de rastreamento para consulta histórica e relatórios.

 ### MicroServiço de Logística
- **Função**: Gerencia a logística reversa, organizando a coleta e o transporte de produtos devolvidos em parceria com empresas de transporte. Ele também otimiza as rotas e gerencia o tempo de coleta.
- **Banco de Dados**: Armazena informações de ordens de coleta, rotas e status de transporte.

 ### MicroServiço de Reciclagem
- **Função**: Determina o destino final dos produtos devolvidos, como reaproveitamento, conserto ou reciclagem. Ele também coordena com parceiros de reciclagem para garantir que os produtos sejam processados corretamente.
- **Banco de Dados**: Armazena informações sobre produtos reciclados e o impacto ambiental gerado por cada ciclo.

 ### MicroServiço de Notificações
- **Função**: Envia notificações automáticas para os consumidores e e-commerces informando o status de cada devolução e qualquer mudança importante.
- **Banco de Dados**: Mantém um histórico das notificações enviadas para controle e auditoria.

 ### MicroServiço de Relatórios
- **Função**: Gera relatórios detalhados sobre o desempenho da solução de logística reversa, incluindo impacto ambiental, eficiência das devoluções, e estatísticas de reciclagem.
- **Banco de Dados**: Armazena dados históricos para gerar relatórios e dashboards.
  
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

- **Microserviços**: A arquitetura é baseada no padrão de microserviços, onde cada serviço tem uma responsabilidade única.
- **API Gateway**: Centraliza as comunicações externas (ERP/WMS) por meio de um API Gateway.
- **Webhooks**: O uso de Webhooks para atualizações assíncronas permite que os sistemas externos sejam notificados automaticamente.
  
---

## Existem Padrões Ocultos?

- **Mensageria Assíncrona**: A arquitetura pode incluir mensageria assíncrona para comunicação entre microserviços.
- **Circuit Breaker/Retry**: Um padrão oculto pode ser a implementação de circuit breakers ou tentativas de reenvio nos serviços críticos.
- **public/subscribe**:
- **EDA**:
  
---

## Metamodelo

O metamodelo desta arquitetura é o padrão de microserviços orquestrados por um API Gateway com comunicação assíncrona via Webhooks e APIs. Ele se baseia na separação de responsabilidades, onde cada microserviço cuida de uma função específica do negócio (ex.: devoluções, logística, reciclagem).

---

## Pode ser Discernido no Diagrama/Arquitetura Única?

Sim, o metamodelo pode ser claramente identificado no diagrama, pois vemos a divisão clara de responsabilidades entre os microserviços e o uso de Webhooks para notificação de eventos.

---

## O Diagrama Está Completo?

O diagrama cobre os principais componentes e fluxos da solução SaaS de logística reversa sustentável. No entanto, ele poderia incluir mais detalhes sobre a mensageria assíncrona e segurança, além de especificar ferramentas de monitoramento e logs.

---

## Poderia ser Simplificado e Ainda Assim Ser Eficaz?

Sim, o diagrama poderia ser simplificado, mas isso poderia impactar a visibilidade das interações mais complexas (como Webhooks e notificações).

---

## Discussão Importante na Equipe

Uma discussão importante poderia girar em torno de como garantir a escalabilidade e a resiliência do sistema, especialmente em momentos de pico de devoluções e aumento de volume de dados.

---

## Decisões com Dificuldade

A decisão sobre usar ou não mensageria assíncrona pode ter sido um ponto difícil, já que nem todos os microserviços podem precisar disso, mas a mensageria aumenta a resiliência do sistema.

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
