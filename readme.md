1. Entendendo os Conceitos Básicos (Para vocês)

O que é um Framework?
Imagine que você vai construir uma casa. Fazer isso usando apenas Javascript puro seria como fabricar seus próprios tijolos, cimento e ferramentas antes de começar. Usar um framework é como comprar um terreno que já vem com a fundação pronta, paredes levantadas e a tubulação passada. Você só precisa se preocupar em mobiliar e definir a função de cada cômodo. Ele te dá uma estrutura e dita as regras de como a construção deve ser feita.

O que é o Angular?
É um framework completo mantido pelo Google para construir aplicações web (sites que funcionam como programas de computador). Ele utiliza o TypeScript, uma linguagem fortemente tipada. Se vocês já estudaram linguagens orientadas a objetos como C#, a transição será muito natural, pois conceitos como injeção de dependências e classes estão no coração do Angular.
2. Divisão de Tarefas (3 Membros)

Para otimizar o tempo, dividam as responsabilidades da seguinte forma:

    Membro 1 (Você - O Contador de Histórias): Responsável por dominar a teoria. Você vai escrever o roteiro, definir a ordem da apresentação, comparar o Angular com outros frameworks (React e Vue) e explicar o porquê de usá-lo.

    Membro 2 (O Desenvolvedor): Responsável exclusivo por criar a pequena aplicação prática. Não precisa fazer slides, o foco é 100% em fazer o código rodar e entender como explicar o código na hora.

    Membro 3 (O Designer/Revisor): Responsável por montar a apresentação visual, diagramar a arquitetura no quadro ou em diagramas, e testar o software junto com o desenvolvedor para caçar bugs.

3. Tutorial do Software de Demonstração

Para extrair as capacidades do Angular sem gastar semanas de desenvolvimento, o Membro 2 deve criar um Painel de Controle (Dashboard) de Tarefas. Isso mostrará os três pilares do Angular: Componentes, Serviços e Roteamento.

Passo a passo da configuração (Nível Professor):

    Instalação dos pré-requisitos: Baixe e instale o Node.js no computador.

    Instalação do Angular CLI: Abra o terminal e digite: npm install -g @angular/cli. Isso instala a ferramenta de linha de comando do Angular.

    Criação do Projeto: No terminal, digite ng new painel-seminario. Quando perguntar sobre routing, digite y (yes). Escolha CSS normal.

    Abrindo o ambiente: Abra o projeto gerado no Visual Studio Code para começar a editar. O terminal integrado do VS Code será o seu melhor amigo.

    Rodando o servidor: No terminal do VS Code, digite ng serve -o. Isso vai abrir o navegador e atualizar a tela automaticamente a cada vez que o código for salvo.

    Criando os componentes (O poder do Angular):

        Digite ng generate component cabecalho

        Digite ng generate component lista-tarefas
        O Angular cria automaticamente os arquivos HTML, CSS e TypeScript separadinhos para cada pedaço da tela.

Na apresentação, o Membro 2 vai abrir o VS Code, mostrar como o HTML está separado da lógica (TypeScript) e como um componente se comunica com o outro na tela.
4. Estrutura da Apresentação Visual (Slides)

Na hora de montar a apresentação, estruturem o material com um layout profissional e apliquem um tema escuro para reduzir o cansaço visual e destacar trechos de código. Lembrem-se de exportar o arquivo final em formato PDF, garantindo que o escalonamento visual fique perfeito e a formatação não quebre em nenhum projetor ou computador da universidade.

Estrutura dos Slides:

    Slide 1: Capa (Tema, Nome do Framework, Nomes dos Alunos).

    Slide 2: O que é um Framework? (Metáfora da construção).

    Slide 3: O que é o Angular? (História, criado pelo Google, focado em grandes projetos).

    Slide 4: Angular vs Javascript Puro (Por que não usar apenas JS? Falar sobre manutenibilidade e produtividade).

    Slide 5: Angular vs Outros (Comparação rápida com React: Angular já vem com tudo pronto de fábrica, React você precisa montar as peças).

    Slide 6: Pontos Fortes e Fracos (Curva de aprendizado alta vs Organização impecável para equipes grandes).

    Slide 7: Quando usar? (Sistemas empresariais, dashboards, plataformas bancárias).

    Slide 8: A Arquitetura (Mostrar um diagrama simples de Componentes e Serviços).

    Slide 9: Demonstração Prática (Hora de mostrar o software rodando).

    Slide 10: Conclusão e Dúvidas.

5. TODO List Completa (Checklist da Semana)

Dias 1 e 2: Estruturação

    [ ] Membro 1: Pesquisar a fundo a diferença entre Angular, React e Vue.

    [ ] Membro 1: Escrever o primeiro rascunho do roteiro da história (usando os tópicos do slide).

    [ ] Membro 2: Instalar Node.js, Angular CLI e rodar o projeto inicial vazio no VS Code.

    [ ] Membro 3: Criar o esqueleto da apresentação visual, aplicar a identidade visual escura e compartilhar com o grupo.

Dias 3 e 4: Desenvolvimento e Refinamento

    [ ] Membro 2: Criar os componentes do "Dashboard" (Cabeçalho e Lista).

    [ ] Membro 2: Fazer a lógica em TypeScript para adicionar e remover itens falsos da lista.

    [ ] Membro 1: Revisar o roteiro e definir exatamente em qual slide cada frase será dita.

    [ ] Membro 3: Preencher os slides com as informações resumidas do Membro 1.

Dia 5: Integração

    [ ] Membro 3: Exportar a apresentação finalizada para PDF e colocar em um pendrive/nuvem.

    [ ] Grupo: Juntar a teoria com a prática. Definir a transição da sua fala (história) para a parte técnica do Membro 2 (código).

Dia 6 e 7: Ensaios

    [ ] Grupo: Ensaiar a apresentação cronometrando o tempo.

    [ ] Membro 2: Testar se o código roda perfeitamente do zero (simular a hora da apresentação).

    [ ] Grupo: Preparar respostas para perguntas difíceis que o professor possa fazer (ex: "Qual a desvantagem do Angular?" - R: "Curva de aprendizado íngreme e tamanho inicial do projeto maior").