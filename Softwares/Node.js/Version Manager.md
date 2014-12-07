# Version Manager

Caso precise rodar mais de uma versão do Node.js, alternando entre verões diferentes será necessário utilizar um software de gerenciamento de verão.

Um dos softwares mais populares para gerenciamento de versão no [Node.js](http://nodejs.org) é o [NVM](https://github.com/creationix/nvm).

## Instalação

### Windows

Faça o download do [NVM para windows](https://github.com/coreybutler/nvm-windows) e execute o instalador. O processo de instalação é simples, basta avançar entre os passos do instalador ate sua conclusão.

### Linux & OS X

Abra seu Terminal e execute o comando abaixo:

```bash
curl https://raw.githubusercontent.com/creationix/nvm/v0.20.0/install.sh | bash
```

## Utilização

Abra seu terminal (*nix) ou prompt de comando (Windows) e digite `nvm -v` e veja se é apresentado a versão instalada do [NVM](https://github.com/creationix/nvm). Agora precisamos instalar o [Node.js](http://nodejs.org), para isso rode o comando abaixo:

```bash
nvm install stable
```

Após os procedimentos automatizados de instalação serem efetuados pelo [NVM](https://github.com/creationix/nvm), precisamos definir a versão estável do [Node.js](http://nodejs.org) a ser utilizada e também definir a versão padrão do ambiente.

```bash
nvm use stable
nvm alias default stable
```

Feito os procedimentos citados acima seu ambiente está pronto para trabalhar com [Node.js](http://nodejs.org).
