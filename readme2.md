# 📊 Kafka Event Stream Dashboard (Painel de Monitoramento)

## 📌 Visão Geral do Software
O **Kafka Event Stream Dashboard** é uma aplicação web *Single Page Application* (SPA) desenvolvida para simular um console de operações e monitoramento em tempo real de uma **Arquitetura Orientada a Eventos (EDA)** baseada em Apache Kafka.

O objetivo principal deste software é demonstrar a capacidade do framework **Angular** em lidar com atualizações de estado de alta frequência (simulando a chegada de milhares de mensagens por segundo) e refletir essas mudanças instantaneamente na interface do usuário (UI) sem causar travamentos de renderização (gargalos no DOM).

---

## 🛠️ Stack Tecnológica
* **Frontend Framework:** Angular (v15+)
* **Linguagem Principal:** TypeScript
* **Estruturação e Estilização:** HTML5 e CSS3 (Design responsivo e componentizado)
* **Ambiente de Execução:** Node.js (utilizado pelo Angular CLI para compilação e servidor de desenvolvimento)
* **Domínio de Negócio (Simulado):** Apache Kafka (Brokers, Tópicos, Throughput, Consumer Lag)

---

## 🚀 Funcionalidades do Dashboard

O painel foi projetado para atuar como uma central de controle para Engenheiros de Dados e SREs (Site Reliability Engineers). Ele possui as seguintes funcionalidades:

1. **Monitoramento de Métricas Globais (Cards Superiores):**
   * **Brokers Ativos:** Quantidade de servidores Kafka rodando no cluster.
   * **Total de Tópicos:** Número de canais de mensagens abertos.
   * **Throughput (MB/s):** Taxa de transferência de dados em tempo real.
   * **Consumer Lag:** Quantidade de mensagens acumuladas que os consumidores ainda não conseguiram processar. (Muda visualmente se o limite de segurança for ultrapassado).

2. **Listagem de Tópicos em Tempo Real (Tabela):**
   * Exibe uma lista de tópicos críticos da arquitetura (ex: `pedidos-vendas-stream`, `logs-autenticacao`).
   * Mostra o número de partições de cada tópico.
   * Atualiza a taxa de mensagens processadas por segundo dinamicamente.
   * Exibe o *Status* de saúde do tópico (Saudável, Alerta, Crítico) com badges coloridos.

3. **Controle de Estado do Cluster:**
   * Botão de ação para **Rebalancear Cluster**, que altera o estado global da aplicação, simulando uma manutenção corretiva que zera o *Consumer Lag* e estabiliza a rede.

---

## 🧠 Conceitos de Angular Aplicados no Código

Este software foi construído especificamente para extrair o máximo das funcionalidades nativas do Angular. Para a equipe entender o que está acontecendo por baixo dos panos, aqui estão os pilares utilizados:

* **Componentização (`@Component`):** A tela inteira é encapsulada no componente `MonitoramentoKafkaComponent`, garantindo que a lógica (TypeScript), o visual (HTML) e o estilo (CSS) operem de forma isolada e previsível.
* **Property Binding (`[ngClass]`, `[class.warning]`):** O CSS da aplicação é injetado dinamicamente pelo Angular. Se o status de um tópico muda para "CRITICO", o Angular automaticamente adiciona a classe CSS correspondente para mudar a cor do texto sem precisarmos usar `document.getElementById()`.
* **Event Binding (`(click)`):** Quando o usuário clica em "Rebalancear Cluster", o Angular intercepta a ação do HTML e dispara o método `reiniciarCluster()` dentro do TypeScript.
* **Diretivas Estruturais (`*ngFor`):** A tabela de tópicos não foi escrita linha por linha. Usamos o `*ngFor` para iterar sobre o array de objetos `topicos`. Se um novo tópico for adicionado no TypeScript, o HTML se redesenha sozinho.
* **Lifecycle Hooks (`ngOnInit`, `ngOnDestroy`):** Usamos o `ngOnInit` para engatilhar a simulação de tráfego de rede no exato milissegundo em que o painel é renderizado na tela. O `ngOnDestroy` garante que a simulação seja interrompida se o usuário fechar o painel, evitando vazamento de memória.
* **Pipes (`| number`):** Usado diretamente no HTML para formatar os números gigantes de mensagens por segundo de `15400` para `15.400`, mantendo o padrão de leitura humano sem poluir a lógica do sistema.

---

## 📖 Dicionário de Negócio: O que é o Kafka?
*(Guia rápido para os membros da equipe que não estão cursando a disciplina de Event-Driven Architecture)*

Para apresentar este software com propriedade, é necessário entender os termos exibidos na tela:
* **Apache Kafka:** É um sistema de mensageria que permite que diferentes programas conversem entre si em altíssima velocidade. 
* **Tópico:** É como se fosse um grupo do WhatsApp. Um sistema envia uma mensagem (ex: "Nova compra realizada") para um tópico, e outros sistemas que estão escutando esse tópico recebem a mensagem instantaneamente.
* **Broker:** É o servidor físico ou máquina virtual onde o Kafka está rodando. Um grupo de Brokers forma um "Cluster".
* **Consumer Lag (Atraso):** Acontece quando o sistema que *produz* mensagens está enviando dados muito mais rápido do que o sistema que *lê* as mensagens consegue processar. Se o Lag subir muito, a arquitetura está sofrendo um gargalo.

---

## 💻 Como Executar o Projeto Localmente

**Pré-requisitos:**
* [Node.js](https://nodejs.org/) instalado na máquina.
* Angular CLI instalado globalmente.

**Passo a passo:**
1. Clone ou baixe este repositório para a sua máquina.
2. Abra o terminal na pasta raiz do projeto (`/dashboard-kafka`).
3. Instale as dependências do projeto executando o comando:
   ```bash
   npm install

   Inicie o servidor de desenvolvimento do Angular:
    Bash

    ng serve

    Abra o seu navegador e acesse a URL:
    Plaintext

    http://localhost:4200

(O Angular possui Hot-Reload nativo. Qualquer alteração feita e salva nos arquivos .ts, .html ou .css recarregará a página automaticamente).