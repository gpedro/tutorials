# Webapp

O [webapp](https://github.com/yeoman/generator-webapp) é um gerador para [Yeoman](http://yeoman.io/) que por padrão disponibiliza o uso do framework [Bootstrap](http://getbootstrap.com/), preprocessador [SASS](http://sass-lang.com/) e bibliotecas como [Modernizr](http://modernizr.com/) e [jQuery](http://jquery.com/) por padrão.

## Instalação

O processo de instalação do gerador [webapp](https://github.com/yeoman/generator-webapp) é simples, basta executar o seguinte comando:

```bash
npm install generator-webapp -g
```

Você também pode fazer a instalação utilizando o próprio [Yeoman](http://yeoman.io/), execute o comando `yo` e selecione a opção `Install a generator`, depois digite o nome do gerador (ex.: webapp) e o selecione na lista que irá retornar com sua busca.

Abra o terminal dentro da pasta onde você costuma criar seus projetos e crie um diretório chamado `myapp` e acesse esse diretório.

```bash
mkdir myapp && cd myapp
```

Agora dentro do diretório `myapp` execute o gerador [webapp](https://github.com/yeoman/generator-webapp).

```bash
yo webapp
```

Teste sua aplicação rodando o comando:

```bash
grunt serve
```

Se tudo correr bem seu navegador ira abrir e apresentar sua aplicação.
