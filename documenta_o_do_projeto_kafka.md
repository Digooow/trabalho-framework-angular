# 📊 Kafka Event Stream Dashboard (Painel de Monitoramento EDA)

![Angular](https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![NodeJS](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)

## 📌 1. Visão Geral e Contexto de Negócio
O **Kafka Event Stream Dashboard** é uma *Single Page Application* (SPA) desenvolvida para atuar como um console de operações (NOC) para monitoramento em tempo real de clusters **Apache Kafka** inseridos em uma Arquitetura Orientada a Eventos (EDA).

Este software não interage diretamente com o cluster Kafka em nível de rede nesta versão acadêmica. Em vez disso, ele **simula um barramento de eventos de alta frequência** para demonstrar a eficiência do motor de renderização do **Angular** (Change Detection) e do **TypeScript** no gerenciamento de estados complexos na memória do navegador.

---

## 🏗️ 2. Arquitetura do Frontend

O sistema foi desenhado utilizando o padrão de **Componentização Baseada em Recursos** (Feature-based). Em vez de espalhar lógicas pelo sistema inteiro, toda a regra de negócio do monitoramento vive isolada no seu próprio ecossistema.

### 2.1. Árvore de Diretórios do Módulo
```text
/src/app/monitoramento-kafka/
│
├── monitoramento-kafka.component.ts    # Controller: Lógica de negócio, simulação de eventos e estado.
├── monitoramento-kafka.component.html  # View: Estrutura semântica e data-binding.
├── monitoramento-kafka.component.css   # Estilo: Regras visuais isoladas (Scoped CSS).
└── monitoramento-kafka.component.spec.ts # Testes: Arquivo reservado para testes unitários (Jasmine/Karma).
```

### 2.2. Motor de Reatividade (Change Detection)
Em cenários reais de EDA, o front-end receberia dados via WebSockets ou Server-Sent Events (SSE). Para simular isso sem travar a interface (o temido gargalo de DOM do JavaScript puro), o sistema utiliza a estrutura de *Data Binding* nativa do Angular. Quando a classe TypeScript sofre uma mutação em suas propriedades, a View (HTML) re-renderiza estritamente os nós necessários.

---

## 📄 3. Contratos de Dados (Interfaces e DTOs)

Para garantir previsibilidade e evitar erros em tempo de execução, os dados são fortemente tipados usando as seguintes interfaces (funcionando de forma análoga aos DTOs e *Records* em sistemas back-end tipados):

```typescript
// Contrato que define a estrutura de um Tópico no cluster
export interface TopicoKafka {
  nome: string;
  particoes: number;
  mensagensPorSegundo: number;
  status: 'SAUDAVEL' | 'ALERTA' | 'CRITICO'; // Union Types limitando estados possíveis
}

// Contrato que define as métricas globais do hardware/cluster
export interface MetricaCluster {
  brokersAtivos: number;
  totalTopicos: number;
  throughputMBps: number;
  lagTotal: number;
}
```

---

## 🔄 4. Ciclo de Vida e Fluxo de Eventos

A aplicação utiliza os *Lifecycle Hooks* do Angular para controlar o início e o fim do processamento de dados:

1. **`ngOnInit()`:** Disparado assim que o componente nasce. Ele invoca o método `iniciarSimulacaoTempoReal()`, que abre um *loop* (via `setInterval`) injetando flutuações aleatórias na taxa de mensagens dos tópicos a cada 2000 milissegundos.
2. **Ciclo de Atualização:** A cada iteração, as propriedades matemáticas de `throughput` e `mensagensPorSegundo` são recalculadas. A interface HTML escuta essas variáveis via chaves duplas `{{ variavel }}` e atualiza os números na tela.
3. **Condicionais Visuais (`[ngClass]`, `[class.warning]`):** O CSS é injetado programaticamente. Se o `lagTotal` ultrapassar 50 mensagens, o texto imediatamente se torna vermelho, alertando o operador.
4. **`ngOnDestroy()`:** Fundamental para evitar vazamento de memória (*Memory Leak*). Se o usuário navegar para outra tela, este hook intercepta a destruição do componente e executa um `clearInterval()` para matar a simulação de tráfego que rodava em background.

---

## ⚙️ 5. Guia de Instalação e Execução

### 5.1. Requisitos de Ambiente
* **Node.js** (v18 LTS ou superior) - Motor V8 para execução do tooling.
* **Angular CLI** (`npm install -g @angular/cli`) - Interface de linha de comando.

### 5.2. Executando Localmente (Ambiente de Desenvolvimento)
1. Clone o repositório e navegue até a pasta raiz:
   ```bash
   cd dashboard-kafka
   ```
2. Instale todas as dependências mapeadas no `package.json`:
   ```bash
   npm install
   ```
3. Inicialize o servidor de desenvolvimento:
   ```bash
   ng serve -o
   ```
   > A flag `-o` abrirá o sistema automaticamente no navegador padrão em `http://localhost:4200/`. O sistema conta com *Hot Module Replacement (HMR)* — qualquer arquivo salvo no VS Code refletirá imediatamente na tela.

### 5.3. Build para Produção
Para gerar os artefatos finais compactados para implantação em servidores reais (Nginx, Apache, AWS S3):
```bash
ng build --configuration production
```
*Isso gerará uma pasta `/dist/` contendo os bundles finais de HTML, CSS e JS ofuscados.*

---

## 🔮 6. Roadmap e Próximos Passos (Evolução do Projeto)
* [ ] **Integração Real (RxJS):** Substituir a simulação `setInterval` por conexões WebSockets reais utilizando observables da biblioteca RxJS.
* [ ] **Autenticação:** Implementar *Route Guards* do Angular para proteger a rota do painel, exigindo login administrativo.
* [ ] **Gráficos Complexos:** Integrar bibliotecas como *Chart.js* ou *D3.js* para plotar o histórico de variação do Throughput em gráficos de linha.