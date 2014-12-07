## Assemble

Assemble é um gerador de site estático (parecido com [Jekyll](http://jekyllrb.com/)), junto ao [Handlebars.js](http://handlebarsjs.com/) que é um motor de template, podemos trabalhar com inclusão de partials e templates. Iremos utilizar essas ferramentas com o gerador [webapp](https://github.com/yeoman/generator-webapp), porem você pode utilizar este mesmo tutorial com outros geradores do [Yeoman](http://yeoman.io/).

### Estrutura

Partindo do pressuposto que você já tenha uma instalação do [webapp](https://github.com/yeoman/generator-webapp) instalada, agora precisamos adequá-la ao Assemble. Acesse o diretório `app` e crie a seguinte estrutura de diretórios e arquivos:

    app/
      templates/
        data/
          data.json
        layouts/
          master.hbs
          page.hbs
        pages/
          index.hbs
          about.hbs
          contact.hbs
        partials/
          header.jade
          navigation.jade
          footer.jade

Copie o conteúdo do arquivo `app\index.html` e cole dentro do arquivo `app\templates\layouts\master.hbs`. Remova o arquivo `app\index.html`, não será mais necessário. 

### Suporte ao Jade

    ```jade
    grunt.initConfig({
      watch: {
        [...]
        assemble: {
          files: [
            '<%= config.app %>/templates/data/{,*/}*.json',
            '<%= config.app %>/templates/layouts/{,*/}*.hbs',
            '<%= config.app %>/templates/pages/{,*/}*.hbs',
            '<%= config.app %>/templates/partials/{,*/}*.hbs'
          ],
          tasks: [
            'assemble:server',
            'prettify:tmp'
          ]
        }
      },
      [...]
    });
    ```

    ```jade
    grunt.initConfig({
      [...]
      wiredep: {
        app: {
          [...]
          src: ['<%= config.app %>/templates/layouts/master.hbs'],
          [...]
        },
        [...]
      },
    });
    ```

    ```jade
    grunt.initConfig({
      [...]
      useminPrepare: {
        [...]
        html: '<%= config.app %>/templates/layouts/master.hbs'
      },
      [...]
    });
    ```

    ```jade
    grunt.initConfig({
      [...]
      // Convert .hbs files to .html
      assemble: {
        options: {
          data: [
            '<%= config.app %>/templates/data/*.json',
            'package.json'
          ],
          layoutdir: '<%= config.app %>/templates/layouts',
          layout: 'default.hbs',
          partials: ['<%= config.app %>/templates/partials/{,*/}*.hbs'],
          flatten: true
        },
        server: {
          files: [{
            expand: true,
            cwd: '<%= config.app %>/templates/pages/',
            src: ['{,*/}*.hbs'],
            dest: '.tmp',
            ext: '.html'
          }]
        }
      }
    });
    ```

    ```jade
    grunt.loadNpmTasks('assemble');
    ```

    ```jade
    grunt.registerTask('serve', [...], function (target) {
      grunt.task.run([
        [...]
        'wiredep',
        'assemble',
        'concurrent:server',
        [...]
      ]);
    });
    ```

    ```jade
    grunt.registerTask('build', [
      [...]
      'wiredep',
      'assemble',
      'useminPrepare',
      [...]
    ]);
    ```

### Correções
