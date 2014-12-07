# Jade

Jade é um motor de template popular entre os desenvolvedores que trabalham com [Node.js](http://nodejs.org/), sua sintaxe simples, limpa e incetiva os desenvolvedores a escrever menos código. Iremos adicionar suporte ao Jade no gerador [webapp](https://github.com/yeoman/generator-webapp) partindo de uma instalação nova, porem você pode executar o mesmo procedimento em um instalação do [webapp](https://github.com/yeoman/generator-webapp) já existente.

## Estrutura

Partindo do pressuposto que você já tenha uma instalação do [webapp](https://github.com/yeoman/generator-webapp) instalada, agora precisamos adequá-la ao Jade. Acesse o diretório `app` e crie a seguinte estrutura de diretórios e arquivos:

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

Abra o arquivo `app\index.html` e remova todo HTML dentro da classe `.container`. Copie o conteúdo do arquivo `app\index.html` e cole dentro do arquivo `app\layouts\master.jade`, em seguida converta este conteúdo para a sintaxe do Jade utilizando algum conversor HTML para Jade (Recomendo [http://html2jade.vida.io/](http://html2jade.vida.io/)). Feito esse procedimento remova o arquivo `app\index.html`, este arquivo não será mais necessário.

Agora com o arquivo `app\layouts\master.jade` convertido para Jade, adicione depois de `.container` o código `block body`:

```jade
[...]
body
  .container
    block body
[...]
```

Abra o arquivo `app\layouts\page.jade` e adicione o conteúdo a seguir:

```jade
extends ./master.jade

block body
  header(role="banner")
    h1 Header

  block content

  footer(role="contentinfo")
    h2 Footer
```

Abra o arquivo `app\pages\index.jade` e adicione o conteúdo a seguir:

```jade
extends ../layouts/page.jade

block content
  main(role='main')
    | My output.
```

Feito esses procedimento temos a estrutura correta, porem ainda não temos as configurações corretas em nosso arquivo `Gruntfile.js` para funcionar com suporte ao Jade.

## Suporte ao Jade

Primeiro precisamos instalar o pacote Jade para o [Grunt](http://gruntjs.com/):

```bash
npm install grunt-contrib-jade
```

Feito a instalação do pacote, é necessário abrir o `Gruntfile.js` e efetuar as seguintes configurações.

1. Adicionar ao `livereload` os arquivos Jade para que quando efetuado alterações ele atualize o servidor:

    ```js
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
    ```

2. Adicionar ao `watch` suporte ao Jade para que quando efetuar alterações execute as tarefas do Jade:

    ```js
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
    ```

3. Alterar no `wiredep` o arquivo principal de inclusão automática dos arquivos do bower:

    ```js
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
    ```

4. Alterar no `wiredep` através do `fileTypes` a forma de inclusão dos arquivos do bower. Isso irá corrigir o endereço dos arquivos adicionado uma barra (/) ao início de cada chamada de arquivo:

    ```js
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
    ```

5. Edite o `useminPrepare` para utilizar o `index.html` da pasta `.tmp`. Essa tarefa é usada pelo `wiredep` na inclusão automática dos arquivos:

    ```js
    [...]
    useminPrepare: {
      html: '.tmp/index.html'
    },
    [...]
    ```

6. Adicionar a tarefa do Jade:

    ```js
    grunt.initConfig({
      [...]
      // Convert .jade files to .html
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
    ```

7. Fazer o carregamento do modulo do NPM:

    ```js
    grunt.loadNpmTasks('grunt-contrib-jade');
    ```

8. Adicionar a chamada opara a tarefa do Jade nos registros de tarefa:

    ```js
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
    ```

Feito esses procedimentos agora sua aplicação tem suporte ao Jade. Divirta-se!

## Correções

Caso os links dentro de `app\layouts\master.jade` apresentem `../` como o exemplo abaixo:

```jade
link(rel='stylesheet', href='/../bower_components/font-awesome/css/font-awesome.css')
```

A correção é simples, basta alterar `\.\.\/` para `(\.\.\/){1,2}`, como o exemplo abaixo:

```js
grunt.initConfig({
  wiredep: {
    app: {
      ignorePath: /^\/|(\.\.\/){1,2}/,
      [...]
    }
  }
});
```

## Filtros

Filtros podem auxiliar em diversas tarefas, como fazer encoding de código HTML. Para adicionar suporte a filtros acesse a raiz do projeto e crie o arquivo `filters.js`:

```bash
touch filters.js
```

Dentro deste arquivo coloque o seguinte conteúdo:

```js
'use strict';

var filters = module.exports = {};
```

Agora para fazer a chamada para esse arquivo, abra o arquivo `Gruntfile.js` e adicione a chamada `filters: require('./filters.js')` dentro da tarefa do Jade:

```js
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
```
