# PostgreSQL

O Postgres oferece opção de criar e executar instâncias por meio de containers Docker. E por que escolhi o Postgres? Pela possibilidade de execução de sua ferramenta gráfica de administração a partir de um container.

> Você também pode acompanhar o tutorial do docker hub em: [hub.docker.com/](https://hub.docker.com/_/postgres)

Antes de mais nada, precisamos ter uma imagem presente na máquina onde os containers existirão. Para isso vamos executar o seguinte código no terminal:

```bash
docker pull postgres
```

Caso queira listar todas as imagens disponíveis no seu sistema:

```bash
docker images
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuAGWXumWXCNXpPBDd3%2F-LuAGohbAXtyJI2m73DU%2Fimage.png?alt=media&token=fd57c915-fdbd-4d90-a61d-524e22c75cb7)

Para manter o estado à medida que você vai trabalhando com o container, precisa commitar as alterações:

```bash
//docker commit [ID do container] [nome da imagem]
docker commit afd8110f1813 postgres
```

## Trabalhando com containers

Listando todos os seus containers atuais (tantos os running quanto os non-running):

```bash
docker ps -l
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-Lu30Lq-cIrH1Tft0wpO%2F-Lu4-zbVUIcebJKaqH6K%2Fimage.png?alt=media&token=9bd9e9e3-3dfe-435b-9bf3-68ffc54d521d)

## Iniciando uma instância

```bash
//docker run --name [Nome do seu banco de dados] -e POSTGRES_PASSWORD=[Senha do seu banco de dados] -d postgres
docker run --name teste-postgres -e POSTGRES_PASSWORD=Postgres2019! -d postgres
```

E verifica se o docker foi instanciado em sua máquina:
O comando ```docker ps -a``` lista todas as instâncias.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuXtAMxBlvnZzNwsxn7%2Fimage.png?alt=media&token=7190c3a8-69cd-4f32-a8c5-a2c8b8f9888c)

Agora dê um start com ```docker start [CONTAINER ID]``` e ```docker ps``` para conferir de foi:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuXtnjw8mzvLelt9MDl%2Fimage.png?alt=media&token=9ee89cd8-9bd0-49dd-815b-38e7ad9f4b82)

## Plataforma de banco de dados

Eu uso o dbeaver-ce e vou utilizá-lo neste tutorial.
Abra o dbeaver e clique em New database connection.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuAGWXumWXCNXpPBDd3%2F-LuANWKoKYMJ2vBzR9IK%2Fimage.png?alt=media&token=47532206-b940-46c9-b568-670cb86e5b20)

Selecione PostgreSQL

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuAGWXumWXCNXpPBDd3%2F-LuANgoIK8H8l8PLBlRr%2Fimage.png?alt=media&token=7c1bfe25-28f4-4b2a-b50d-18a8054a0805)

E configure sua conexão, nesse caso precisa apenas colocar a senha que colocamos ao dar docker run, lá em iniciando uma instância. 
*Password: Postgres2019!*

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LtBlzfJpOHzLhCvqiJx%2F-LuXs0gdNFW10bTH8ypg%2F-LuXuK_kArmHvJTgP9w-%2Fimage.png?alt=media&token=f2f78f8b-c5bf-4bc8-a852-cd012bff4bea)

Antes de concluir, teste a conexão. Logo após pode dar ok e mãos a obra.