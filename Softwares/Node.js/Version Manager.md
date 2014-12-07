# Version Manager

Caso precise rodar mais de uma versão do Node.js, alternando entre verões diferentes será necessário utilizar um software de gerenciamento de verão.

Um dos softwares mais populares para gerenciamento de versão no Node.js é o [NVM](https://github.com/creationix/nvm).

## Instalação

### Windows

Faça o download do [NVM para windows](https://github.com/coreybutler/nvm-windows) e execute o instalador. O processo de instalação é simples, basta avançar entre os passos do instalador.

### Linux & OS X

Abra seu Terminal e execute o comando abaixo:

```bash
curl https://raw.githubusercontent.com/creationix/nvm/v0.20.0/install.sh | bash
```

## Utilização

Abra seu Terminal (*nix) ou Prompt de Comando (Windows) e digite `nvm -v` e veja se mostrado a versão instalada do nvm. Caso tenha mostrado a versão, iremos instalar o node utilizando o seguinte comando. 

Agora vamos instalar a versão estável do Node.JS

```bash
nvm install stable
```

Após os procedimentos automatizados de instalação serem efetuados pelo nvm, vamos definir o node a ser utilizado e também temos que definir o Node.JS Estável como padrão do ambiente .

```bash
nvm use stable
nvm alias default stable
```
