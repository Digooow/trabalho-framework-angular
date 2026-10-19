Guia Completo do Seminário: Framework Angular
1. Roteiro da Apresentação (Meta: 30 Minutos)
Parte 1 — Conhecendo a tecnologia (Seu Papel: Contador de Histórias | ~10 minutos)

    O que é? Um framework web completo (não apenas uma biblioteca ou ferramenta avulsa). Ele dita as regras e a arquitetura de como o código deve ser organizado, funcionando como a fundação completa de uma casa.

    Para que serve e problemas que resolve? Serve para criar Single Page Applications (SPAs). Resolve o problema do "código espaguete" e da manipulação manual exaustiva do DOM encontrada no desenvolvimento web tradicional, automatizando a sincronização entre os dados e a tela.

    Quem criou e quando? Criado pelo Google. A primeira versão (AngularJS) surgiu em 2010. Em 2016, foi totalmente reescrito e modernizado para o Angular atual.

    Onde é utilizado? Em sistemas que exigem alta escalabilidade, segurança e padronização corporativa. Exemplos: Gmail, internet banking, painéis administrativos e plataformas corporativas robustas.

    Vantagens: Arquitetura consistente (MVC/Componentes), injeção de dependência nativa, tipagem forte com TypeScript e ecossistema "tudo incluso" (roteamento, formulários e requisições HTTP já vêm de fábrica).

    Limitações: Curva de aprendizado inicial íngreme e tamanho do pacote inicial maior em comparação com bibliotecas mais leves.

    Por que escolher Angular em vez de React/Vue? Pela padronização. Em equipes grandes, o Angular força todos a programarem no mesmo padrão arquitetural, facilitando a entrada de novos desenvolvedores no projeto.

    Por que não usar JavaScript puro? O TypeScript traz o rigor da tipagem forte, autocompletar e detecção de erros em tempo de compilação — características indispensáveis para quem desenvolve sistemas complexos e já está acostumado com back-ends orientados a objetos, como C#.

Parte 2 — Preparando o ambiente (Membro 3 | ~5 minutos)

    O que é necessário instalar? Apenas o Node.js (que inclui o gerenciador de pacotes NPM) e uma IDE, como o Visual Studio Code.

    Como criar um projeto do zero?

        Comando 1: npm install -g @angular/cli (instala o motor do Angular no computador).

        Comando 2: ng new nome-do-projeto (gera toda a estrutura base automaticamente).

    Principais arquivos e pastas (Dissecando a Arquitetura):

        src/: A pasta raiz onde o código-fonte da aplicação vive.

        src/app/: O coração do sistema, onde ficam os Componentes (HTML, CSS e TypeScript).

        angular.json: O arquivo mestre que gerencia as configurações de compilação do projeto.

        package.json: A lista de dependências externas.

    Como executar no navegador? O comando ng serve compila a aplicação e levanta um servidor local (geralmente em localhost:4200).

Parte 3 — Colocando a mão na massa (Membro 2 | ~10 minutos)

    Apresentação do Exemplo: Mostrar o "Painel de Tarefas" (Dashboard) rodando limpo no navegador.

    Explicação do Código:

        Abrir o Visual Studio Code.

        Explicar a anatomia de um Componente Angular: mostrar como ele isola a lógica em uma classe TypeScript (.ts), a estrutura visual no .html e o estilo no .css.

        Mostrar o Data Binding: Como uma variável declarada na classe TypeScript é impressa na tela HTML usando chaves duplas {{ variavel }}.

    Modificação ao Vivo:

        Alterar uma cor no CSS ou um texto na variável do TypeScript.

        Salvar o arquivo e mostrar o navegador aplicando a alteração instantaneamente, sem necessidade de atualizar a página (Hot Reload).

Parte 4 — Conclusão (Todo o Grupo | ~5 minutos)

    O que muda em relação a HTML, CSS e JavaScript puros? A mudança do paradigma imperativo (dizer exatamente como a tela deve mudar) para o declarativo (dizer o que a tela deve mostrar com base no estado dos dados).

    Em que tipo de projeto seria útil? Dashboards complexos, sistemas integradores, ERPs e aplicações que exigem manutenção a longo prazo por times rotativos.

    Maior facilidade encontrada: A geração automática de arquivos estruturados através dos comandos do Angular CLI (ng generate).

    Maior dificuldade encontrada: Compreender o fluxo inicial de interligação dos módulos e a adaptação à sintaxe do TypeScript.

2. Documentação e Tutorial (Desenvolvimento do Software)

Este é o guia técnico para o Membro 2 desenvolver o Painel de Tarefas antes da apresentação.

    Geração do Projeto:

        Abra o terminal do VS Code e execute: ng new painel-seminario

        Entre na pasta: cd painel-seminario

    Criação dos Componentes:

        Execute: ng generate component lista-tarefas (Isso cria automaticamente os arquivos de lógica, visual e testes).

    Lógica do Componente (lista-tarefas.component.ts):

        Dentro da classe, crie um array tipado: tarefas: string[] = ['Estudar Angular', 'Fazer os slides', 'Apresentar seminário'];

        Crie uma função simples: adicionarTarefa(novaTarefa: string) { this.tarefas.push(novaTarefa); }

    Integração Visual (lista-tarefas.component.html):

        Use a diretiva estrutural *ngFor (o laço de repetição do Angular) para renderizar a lista:
        <li *ngFor="let tarefa of tarefas">{{ tarefa }}</li>

    Exibição Principal:

        Vá no arquivo app.component.html, apague tudo que vem como padrão e coloque apenas a tag do novo componente: <app-lista-tarefas></app-lista-tarefas>

3. Divisão Ágil de Tarefas

Como o prazo é curto, façam sincronizações diárias de 10 minutos (Daily Standup) para garantir que ninguém está travado.

    Membro 1 (Você - Teoria e Narrativa): Responsável por dominar as respostas da Parte 1 e 4. Deve elaborar o discurso de transição entre a sua contação de história e a entrada da demonstração técnica.

    Membro 2 (Desenvolvimento): Focado exclusivamente em criar, entender e explicar o código do Painel de Tarefas no VS Code (Parte 3).

    Membro 3 (Design e Setup): Responsável por criar os slides, estruturar as informações da Parte 2 e documentar o processo de instalação para apresentar.

4. TODO List da Semana

    Dia 1-2: Preparação Base

        [ ] Membro 1: Roteirizar o texto exato da Parte 1, focando em analogias claras.

        [ ] Membro 2: Instalar dependências (Node, Angular CLI) e gerar o projeto base.

        [ ] Membro 3: Definir a identidade visual dos slides (Priorizar tema escuro para conforto visual e destacar trechos de código).

    Dia 3-4: Produção

        [ ] Membro 1: Roteirizar a Parte 4 (Conclusão) junto com a equipe.

        [ ] Membro 2: Finalizar o código do Painel de Tarefas e testar a modificação ao vivo que será feita na aula.

        [ ] Membro 3: Preencher os slides com as informações resumidas (sem textos longos na tela).

    Dia 5-6: Integração e Refinamento

        [ ] Membro 3: Exportar a apresentação finalizada obrigatoriamente em formato PDF para evitar problemas de escalonamento visual ou fontes quebradas no computador da universidade.

        [ ] Todos: Passagem de bastão (ensaiar o momento exato em que um passa a fala para o outro).

    Dia 7: Ensaio Final

        [ ] Todos: Ensaiar com cronômetro para cravar os 30 minutos de forma confortável.

        [ ] Membro 2: Testar se o ambiente local do Angular levanta corretamente offline ou na rede da universidade.