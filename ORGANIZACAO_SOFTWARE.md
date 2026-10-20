# 📊 Documentação do Software: Kafka Event Stream Dashboard

Este documento contém a especificação técnica completa, contratos de dados, arquitetura de componentes, código-fonte e guia de execução da aplicação prática desenvolvida para o seminário de Angular: o **Kafka Event Stream Dashboard**.

---

## 📌 1. Visão Geral e Contexto de Negócio

O **Kafka Event Stream Dashboard** é uma *Single Page Application* (SPA) corporativa que atua como um console de operações (NOC / SRE) para monitoramento em tempo real de clusters **Apache Kafka** em uma **Arquitetura Orientada a Eventos (EDA)**.

### Propósito Técnico da Demonstração
Em vez de implementar um exemplo trivial (como uma lista de tarefas comum), o grupo desenvolveu um painel de alta frequência de eventos. A aplicação simula um fluxo dinâmico de telemetria para evidenciar as principais forças do Angular:
1. **Change Detection Eficiente:** Atualização seletiva da árvore do DOM sem travamentos de interface (*DOM bottleneck* comum em scripts imperativos com JavaScript puro).
2. **Ciclo de Vida Previsível (*Lifecycle Hooks*):** Inicialização e descarte seguro de fluxos assíncronos para evitar vazamentos de memória (*memory leaks*).
3. **Tipagem Estática Forte com TypeScript:** Prevenção de erros de modelagem de dados através de interfaces contratuais.

---

## 📖 2. Dicionário de Negócio: Conceitos de Apache Kafka

Para capacitar qualquer membro da equipe a explicar a aplicação durante a apresentação, aqui estão os conceitos do domínio simulado:

* **Apache Kafka:** Plataforma distribuída de mensageria e transmissão de fluxos de eventos (*event streaming*) de altíssimo rendimento.
* **Broker:** Um servidor que compõe o cluster Kafka, responsável por armazenar mensagens e atender produtores e consumidores.
* **Cluster:** O conjunto articulado de múltiplos brokers que operam de forma redundante e distribuída.
* **Tópico:** Um canal categorizado de mensagens (análogo a uma tabela de banco de dados orientada a eventos ou a um feed de notícias).
* **Partições:** Divisões físicas de um tópico que permitem paralelismo na escrita e leitura dos eventos.
* **Throughput (MB/s):** Volume de tráfego de dados transmitido por segundo no barramento.
* **Consumer Lag:** Quantidade de mensagens acumuladas na fila que os consumidores ainda não conseguiram processar. Quando o *lag* sobe excessivamente, indica gargalo operacional no sistema consumidor.

---

## 🛠️ 3. Stack Tecnológica

* **Framework Frontend:** Angular (v15+ / Standalone & Módulos)
* **Linguagem:** TypeScript
* **Marcação e Estilização:** HTML5 semântico e CSS3 encapsulado (*Component Scoped Styling*)
* **Ambiente de Tooling:** Node.js (LTS) e Angular CLI
* **Mecanismo Reativo:** Change Detection do Angular com simulação assíncrona controlada

---

## 🏗️ 4. Arquitetura do Componente e Diretórios

O software foi desenvolvido seguindo o padrão de **Componentização Baseada em Recursos** (*Feature-based*), mantendo a regra de negócio do monitoramento isolada em seu próprio diretório:

```text
projeto-seminario/
├── src/
│   ├── app/
│   │   ├── monitoramento-kafka/
│   │   │   ├── monitoramento-kafka.component.ts    # Lógica, estados e simulação de eventos
│   │   │   ├── monitoramento-kafka.component.html  # Estrutura visual e data-binding declarativo
│   │   │   ├── monitoramento-kafka.component.css   # Estilos isolados do dashboard
│   │   │   └── monitoramento-kafka.component.spec.ts # Testes unitários (Jasmine/Karma/Vitest)
│   │   ├── app.component.ts                        # Componente raiz da aplicação
│   │   └── app.component.html                      # Template que renderiza o painel
│   ├── styles.css                                  # Estilos globais da aplicação
│   └── index.html                                  # Página Single Page mestra
├── angular.json                                    # Configurações de compilação da CLI
└── package.json                                    # Manifesto de dependências e scripts
```

---

## 📄 5. Contratos de Dados (Interfaces TypeScript)

Para garantir previsibilidade nos dados manipulados pelo dashboard, foram definidos os seguintes contratos de tipagem forte:

```typescript
// Contrato que define o estado e as propriedades de um Tópico monitorado
export interface TopicoKafka {
  nome: string;
  particoes: number;
  mensagensPorSegundo: number;
  status: 'SAUDAVEL' | 'ALERTA' | 'CRITICO'; // Union Type limitando os estados válidos
}

// Contrato que resume o estado operacional do cluster de hardware
export interface MetricaCluster {
  brokersAtivos: number;
  totalTopicos: number;
  throughputMBps: number;
  lagTotal: number;
}
```

---

## 💻 6. Código-Fonte Completo da Aplicação

### 6.1. Componente Lógico: `monitoramento-kafka.component.ts`

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';

export interface TopicoKafka {
  nome: string;
  particoes: number;
  mensagensPorSegundo: number;
  status: 'SAUDAVEL' | 'ALERTA' | 'CRITICO';
}

export interface MetricaCluster {
  brokersAtivos: number;
  totalTopicos: number;
  throughputMBps: number;
  lagTotal: number;
}

@Component({
  selector: 'app-monitoramento-kafka',
  templateUrl: './monitoramento-kafka.component.html',
  styleUrls: ['./monitoramento-kafka.component.css']
})
export class MonitoramentoKafkaComponent implements OnInit, OnDestroy {
  public clusterStatus: 'ONLINE' | 'REINICIANDO' = 'ONLINE';
  private timerId: any;

  public metricas: MetricaCluster = {
    brokersAtivos: 5,
    totalTopicos: 4,
    throughputMBps: 142.8,
    lagTotal: 45
  };

  public topicos: TopicoKafka[] = [
    { nome: 'pedidos-vendas-stream', particoes: 6, mensagensPorSegundo: 1250, status: 'SAUDAVEL' },
    { nome: 'logs-autenticacao-raw', particoes: 12, mensagensPorSegundo: 4300, status: 'SAUDAVEL' },
    { nome: 'notificacoes-push-queue', particoes: 3, mensagensPorSegundo: 890, status: 'ALERTA' },
    { nome: 'telemetria-dispositivos', particoes: 24, mensagensPorSegundo: 15400, status: 'CRITICO' }
  ];

  ngOnInit(): void {
    this.iniciarSimulacaoTempoReal();
  }

  ngOnDestroy(): void {
    if (this.timerId) {
      clearInterval(this.timerId);
    }
  }

  private iniciarSimulacaoTempoReal(): void {
    this.timerId = setInterval(() => {
      // Simula flutuação dinâmica na vazão dos tópicos
      this.topicos.forEach(topico => {
        const variacao = Math.floor(Math.random() * 100) - 45;
        topico.mensagensPorSegundo = Math.max(100, topico.mensagensPorSegundo + variacao);
      });

      // Recalcula métricas globais do cluster
      this.metricas.throughputMBps = parseFloat((120 + Math.random() * 50).toFixed(1));
      this.metricas.lagTotal = Math.floor(Math.random() * 100);
    }, 2000);
  }

  public reiniciarCluster(): void {
    this.clusterStatus = 'REINICIANDO';
    setTimeout(() => {
      this.clusterStatus = 'ONLINE';
      this.metricas.lagTotal = 0;
    }, 3000);
  }
}
```

### 6.2. Template de Apresentação: `monitoramento-kafka.component.html`

```html
<div class="dashboard-container">
  <header class="dashboard-header">
    <div>
      <h1>Painel de Monitoramento Apache Kafka</h1>
      <p class="subtitle">Console de Operações de Event Stream em Tempo Real</p>
    </div>
    <div class="header-actions">
      <span class="status-badge" [ngClass]="clusterStatus.toLowerCase()">
        Cluster Status: {{ clusterStatus }}
      </span>
      <button class="btn-primary" (click)="reiniciarCluster()" [disabled]="clusterStatus === 'REINICIANDO'">
        Rebalancear Cluster
      </button>
    </div>
  </header>

  <!-- Grade de Métricas Globais -->
  <section class="metrics-grid">
    <div class="metric-card">
      <span class="metric-title">Brokers Ativos</span>
      <span class="metric-value">{{ metricas.brokersAtivos }}</span>
    </div>
    <div class="metric-card">
      <span class="metric-title">Total de Tópicos</span>
      <span class="metric-value">{{ metricas.totalTopicos }}</span>
    </div>
    <div class="metric-card">
      <span class="metric-title">Throughput</span>
      <span class="metric-value">{{ metricas.throughputMBps }} <small>MB/s</small></span>
    </div>
    <div class="metric-card">
      <span class="metric-title">Consumer Lag</span>
      <span class="metric-value" [class.warning]="metricas.lagTotal > 50">
        {{ metricas.lagTotal }} <small>msgs</small>
      </span>
    </div>
  </section>

  <!-- Tabela de Tópicos em Tempo Real -->
  <section class="table-section">
    <h2>Tópicos Monitorados</h2>
    <table class="data-table">
      <thead>
        <tr>
          <th>Nome do Tópico</th>
          <th>Partições</th>
          <th>Taxa de Eventos</th>
          <th>Status</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let topico of topicos">
          <td class="font-bold">{{ topico.nome }}</td>
          <td>{{ topico.particoes }}</td>
          <td>{{ topico.mensagensPorSegundo | number }} msg/s</td>
          <td>
            <span class="tag" [ngClass]="topico.status.toLowerCase()">
              {{ topico.status }}
            </span>
          </td>
        </tr>
      </tbody>
    </table>
  </section>
</div>
```

### 6.3. Estilização Isolada: `monitoramento-kafka.component.css`

```css
.dashboard-container {
  padding: 24px;
  background-color: #f8fafc;
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  color: #334155;
}

.dashboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  border-bottom: 1px solid #e2e8f0;
  padding-bottom: 16px;
}

.dashboard-header h1 {
  margin: 0;
  font-size: 20px;
  color: #0f172a;
}

.subtitle {
  margin: 4px 0 0 0;
  font-size: 13px;
  color: #64748b;
}

.header-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}

.status-badge {
  padding: 6px 12px;
  border-radius: 16px;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.5px;
}

.status-badge.online {
  background-color: #dcfce7;
  color: #166534;
}

.status-badge.reiniciando {
  background-color: #fef3c7;
  color: #92400e;
}

.btn-primary {
  background-color: #2563eb;
  color: #ffffff;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s;
}

.btn-primary:hover {
  background-color: #1d4ed8;
}

.btn-primary:disabled {
  background-color: #94a3b8;
  cursor: not-allowed;
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}

.metric-card {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  flex-direction: column;
}

.metric-title {
  font-size: 12px;
  color: #64748b;
  font-weight: 500;
}

.metric-value {
  font-size: 24px;
  font-weight: 700;
  color: #0f172a;
  margin-top: 8px;
}

.metric-value.warning {
  color: #dc2626;
}

.metric-value small {
  font-size: 12px;
  font-weight: 400;
  color: #64748b;
}

.table-section {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 20px;
}

.table-section h2 {
  font-size: 14px;
  margin: 0 0 16px 0;
  color: #0f172a;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  font-size: 13px;
}

.data-table th {
  background-color: #f8fafc;
  color: #475569;
  padding: 10px 12px;
  font-weight: 600;
  border-bottom: 1px solid #e2e8f0;
}

.data-table td {
  padding: 12px;
  border-bottom: 1px solid #f1f5f9;
}

.font-bold {
  font-weight: 600;
}

.tag {
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 10px;
  font-weight: 700;
}

.tag.saudavel {
  background-color: #f0fdf4;
  color: #15803d;
}

.tag.alerta {
  background-color: #fffbeb;
  color: #b45309;
}

.tag.critico {
  background-color: #fef2f2;
  color: #b91c1c;
}
```

---

## 🧠 7. Conceitos do Angular Demonstrados no Código

Este guia conecta diretamente os recursos do Angular ao código escrito, servindo de consulta rápida para responder a perguntas técnicas:

1. **Decorador `@Component`:** Agrupa lógica (`.ts`), apresentação (`.html`) e estilo (`.css`), conferindo escopo fechado (*encapsulation*).
2. **Interpolação (`{{ expressao }}`):** Permite imprimir o valor de propriedades TypeScript na tela de forma reativa (`{{ metricas.throughputMBps }}`).
3. **Property Binding (`[ngClass]`, `[class.warning]`):** Vincula dinamicamente classes CSS a expressões booleanas do TypeScript (ex: aplica vermelho se `metricas.lagTotal > 50`).
4. **Event Binding (`(click)`):** Intercepta interações do usuário no DOM e dispara métodos na classe TypeScript (`(click)="reiniciarCluster()"`).
5. **Diretiva Estrutural (`*ngFor`):** Itera sobre listas de objetos sem manipular nós do DOM manualmente (`*ngFor="let topico of topicos"`).
6. **Pipes de Formatação (`| number`):** Transforma números brutos para exibição legível com separadores de milhar (`15400` vira `15,400` ou `15.400`) sem alterar o valor original na memória.
7. **Lifecycle Hooks:**
   * `ngOnInit`: Executado assim que o componente é instanciado. Usado para iniciar o timer da simulação em tempo real.
   * `ngOnDestroy`: Executado antes do componente ser destruído. Limpa o `setInterval` com `clearInterval()`, impedindo vazamento de memória (*Memory Leak*).

---

## 🚀 8. Guia de Execução e Roteiro de Live Demo

### 8.1. Pré-Requisitos
* **Node.js** (v18 ou superior)
* **Angular CLI** instalado globalmente:
  ```bash
  npm install -g @angular/cli
  ```

### 8.2. Como Executar Localmente
1. Abra o terminal na pasta do projeto Angular:
   ```bash
   cd projeto-seminario
   ```
2. Instale as dependências:
   ```bash
   npm install
   ```
3. Inicialize o servidor de desenvolvimento:
   ```bash
   ng serve -o
   ```
   > A flag `-o` abre o navegador automaticamente em `http://localhost:4200`.

### 8.3. Roteiro Sugerido para o Live Demo na Apresentação (Membro 2)
1. **Mostrar o painel em execução:** Apontar as métricas e tópicos atualizando sozinhos a cada 2 segundos.
2. **Explicar a reatividade:** Mostrar que a página não recarrega e apenas os números mudam no DOM.
3. **Interação ao vivo:** Clicar em *"Rebalancear Cluster"*, observando o botão ficar desabilitado temporariamente e o *Consumer Lag* ser zerado após 3 segundos.
4. **Modificação ao vivo (Hot Reload):**
   * Abra o arquivo `monitoramento-kafka.component.css` ou `.ts` no VS Code.
   * Altere a cor do botão `.btn-primary` (ex: de azul para roxo) ou aumente a velocidade do `setInterval` de `2000` para `1000`.
   * Salve o arquivo (`Ctrl + S`) e mostre o navegador atualizando instantaneamente sem perder a sessão.

---

## 📦 9. Build de Produção e Roadmap de Evolução

### Compilação de Produção
Para compilar a aplicação com minificação, *tree-shaking* e otimizações AOT:
```bash
ng build --configuration production
```
Os artefatos finais prontos para publicação são gerados no diretório `/dist/`.

### Roadmap para Próximos Passos
* [ ] **Substituição de Mock por RxJS:** Trocar a simulação `setInterval` por fluxos reativos do `RxJS` conectados a um WebSocket ou Server-Sent Events (SSE).
* [ ] **Segurança com Route Guards:** Implementar `CanActivate` para proteger a rota do painel com autenticação administrativa.
* [ ] **Visualização Gráfica:** Integrar biblioteca como *Chart.js* ou *ngx-charts* para exibir gráficos históricos da flutuação de *throughput* e *lag*.

