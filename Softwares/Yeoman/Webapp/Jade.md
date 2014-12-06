## Jade

Jade é um motor de template popular entre os desenvolvedores que trabalham com [Node.js](http://nodejs.org/), sua sintaxe simples e limpa incetiva os desenvolvedores a escrever menos código. Iremos adicionar suporte ao Jade no gerador [webapp](https://github.com/yeoman/generator-webapp) partindo de uma instalação nova, porem você pode executar o mesmo procedimento em um instalação do [webapp](https://github.com/yeoman/generator-webapp) já existente.

### Gerador Webapp

O processo de instalação do gerador [webapp](https://github.com/yeoman/generator-webapp) é simples, basta executar o seguinte comando:

    npm install generator-webapp -g

Você também pode fazer a instalação utilizando o próprio [Yeoman](http://yeoman.io/), execute o comando ```yo``` e selecione a opção ```Install a generator```, depois digite o nome do gerador (ex.: webapp) e o selecione na lista que irá retornar com sua busca.

Abra o terminal dentro da pasta onde você costuma criar seus projetos e crie um diretório chamado ```myapp``` e acesse esse diretório.

    mkdir myapp && cd myapp

Agora dentro do diretório ```myapp``` execute o gerador [webapp](https://github.com/yeoman/generator-webapp).

    yo webapp

Teste sua aplicação rodando o comando:

    grunt serve

Se tudo correr bem seu navegador ira abrir e apresentar sua aplicação.

### Estrutura

Agora que temos instalado a estrutura do nosso projeto, precisamos adequá-la ao Jade. Acesse o diretório ```app``` e crie a seguinte estrutura de diretórios e arquivos:

    app/
      layouts/
        master.jade
        page.jade
      pages/
        index.jade
        about.jade
        contact.jade
      partials/
        header.jade
        navigation.jade
        footer.jade

Abra o arquivo ```app\index.html``` e remova todo HTML dentro da classe ```.container```. Copie o conteúdo do arquivo ```app\index.html``` e cole dentro do arquivo ```app\layouts\master.jade```, em seguida converta este conteúdo para a sintaxe do Jade utilizando algum conversor HTML para Jade (Recomendo [http://html2jade.vida.io/](http://html2jade.vida.io/)). Feito esse procedimento remova o arquivo ```app\index.html```, este arquivo não será mais necessário.

Agora com o arquivo ```app\layouts\master.jade``` convertido para Jade, adicione depois de ```.container``` o código ```block body```:

    [...]
    body
      .container
        block body
    [...]

Abra o arquivo ```app\layouts\page.jade``` e adicione o conteúdo a seguir:

    extends ./master.jade

    block body
      header(role="banner")
        h1 Header

      block content

      footer(role="contentinfo")
        h2 Footer

Abra o arquivo ```app\pages\index.jade``` e adicione o conteúdo a seguir:

    extends ../layouts/page.jade

    block content
      main(role='main')
        | My output.

Feito esses procedimento temos a estrutura correta, porem ainda não temos as configurações corretas em nosso arquivo ```Gruntfile.js``` para funcionar com suporte ao Jade.

### Suporte ao Jade

Primeiro precisamos instalar o pacote Jade para o [Grunt](http://gruntjs.com/):

    npm install grunt-contrib-jade

Feito a instalação do pacote, é necessário abrir o ```Gruntfile.js``` e efetuar as seguintes configurações.

1. Adicionar ao ```livereload``` os arquivos Jade para que quando efetuado alterações ele atualize o servidor:

        grunt.initConfig({
          [...]
          watch: {
            [...]
            livereload: {
              [...]
              files: [
                '<%= config.app %>/**/*.jade',
                [...]
              ]
            }
          }
          [...]
        });

2. Adicionar ao ```watch``` suporte ao Jade para que quando efetuar alterações execute as tarefas do Jade:

        grunt.initConfig({
          [...]
          watch: {
            [...]
            jade: {
              files: ['<%= config.app %>/**/*.jade'],
              tasks: ['jade']
            }
          }
          [...]
        });

3. Alterar no ```wiredep``` o arquivo principal de inclusão automática dos arquivos do bower:

        grunt.initConfig({
          [...]
          wiredep: {
            app: {
              [...]
              src: ['<%= config.app %>/layouts/master.jade'],
              [...]
            }
            [...]
          }
          [...]
        });

4. Alterar no ```wiredep``` através do ```fileTypes``` a forma de inclusão dos arquivos do bower. Isso irá corrigir o endereço dos arquivos adicionado uma barra (/) ao início de cada chamada de arquivo:

        grunt.initConfig({
          [...]
          wiredep: {
            app: {
              [...]
              fileTypes: {
                jade: {
                  replace: {
                    js: 'script(src=\'/{{filePath}}\')',
                    css: 'link(rel=\'stylesheet\', href=\'/{{filePath}}\')'
                  }
                },
              }
            }
          }
          [...]
        });

5. Adicionar a tarefa do Jade:

        grunt.initConfig({
          [...]
          // Convert jade files to html
          jade: {
            dist: {
              options: {
                pretty: true
              },
              files: [{
                expand: true,
                cwd: '<%= config.app %>/pages',
                dest: '.tmp',
                src: ['{,*/}*.jade'],
                ext: '.html'
              }]
            }
          }
        });

6. Fazer o carregamento do modulo do NPM:

        grunt.loadNpmTasks('grunt-contrib-jade');

7. Adicionar a chamada opara a tarefa do Jade nos registros de tarefa:

    grunt.registerTask('serve', [...], function (target) {
      [...]
      grunt.task.run([
        [...]
        'wiredep',
        'jade',
        'concurrent:server',
        [...]
      ]);
    });

    grunt.registerTask('build', [
      [...]
      'wiredep',
      'jade',
      'useminPrepare',
      [...]
    ]);

Feito esses procedimentos agora sua aplicação tem suporte ao Jade. Divirta-se!

### Correções

Caso os links dentro de ```app\layouts\master.jade``` apresentem ```../``` como o exemplo abaixo:

    link(rel='stylesheet', href='/../bower_components/font-awesome/css/font-awesome.css')

A correção é simples, basta alterar ```\.\.\/``` para ```(\.\.\/){1,2}```, como o exemplo abaixo:

    grunt.initConfig({
      wiredep: {
        app: {
          ignorePath: /^\/|(\.\.\/){1,2}/,
          [...]
        }
      }
    });

### Filtros

Filtros podem auxiliar em diversas tarefas, como fazer encoding de código HTML. Para adicionar suporte a filtros acesse a raiz do projeto e crie o arquivo ```filters.js```:

    touch filters.js

Dentro deste arquivo coloque o seguinte conteúdo:

    'use strict';

    var filters = module.exports = {};

Agora para fazer a chamada para esse arquivo, abra o arquivo ```Gruntfile.js``` e adicione a chamada ```filters: require('./filters.js')``` dentro da tarefa do Jade:

    grunt.initConfig({
      [...]
      jade: {
        dist: {
          options: {
            [...]
            filters: require('./filters.js')
          },
          [...]
        }
      }
    });
