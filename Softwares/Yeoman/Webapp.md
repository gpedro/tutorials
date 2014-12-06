## Gerador Webapp

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
