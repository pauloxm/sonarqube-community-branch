# Docker Sonarqube

O objetivo é criar um ambiente com SonarQube e PostgreSQL para uso em qualquer ambiente que tenha o Docker. O projeto foi criado baseado no repositório do projeto [https://github.com/mc1arke/sonarqube-community-branch-plugin]

## Serviços criados

- **Sonarqube-communit-brach-plugin**
- **PostgreSQL**

## Executando a Criação do Projeto

Para criar e iniciar os serviços definidos no arquivo `docker-compose.yaml`, utilize o seguinte comando:

Observação importante: Caso esteja fazendo algum homelab com VM, pode ser que o contianer reclame da alocação de memória. Para contornar isso execute o comando:

```sh
sysctl -w vm.max_map_count=262144
```

Para subir o projeto execute:

```sh
docker compose up -d
```

Eis a sequencia basica para testar o projeto localmente:


1. CRIAR O TOKEN DE USUARIO 
2. CRIAR O PROJETO
3. ATRELAR O TOKEN EXISTENTE
4. CLONE O PROJETO DO SEU REPOSITÓIO PARA O DIRETÓRIO LOCAL. GUARDE O NOME DO DIRETÓRIO QUE SERÁ CRIADO. Segue abaixo um exemplo de repositório fictício, apenas para referência:

```sh
git clone -b develop https://github.com/pauloxm/meu-projeto-dotnet-8.0
```

5. EXECUTAR O COMANDO ABAIXO E VER O RESULTADO


```sh
docker run --rm --network=host \
  -e SONAR_HOST_URL="http://localhost:9000" \
  -e SONAR_LOGIN="SEU TOKEN GERADO NO PASSO 1" \
  -v "./meu-projeto-dotnet-8.0/:/usr/src/" \
  -w /usr/src \
  sonarsource/sonar-scanner-cli \
  -D "sonar.login=SEU TOKEN GERADO NO PASSO 1" \
  -D"sonar.projectKey=NOME DO SEU PROJETO CRIADO NO PASSO 2" \
  -D"sonar.sources=." \
  -D"sonar.dotnet.version=8.0" \
  -D"sonar.cs.dotnetcore.version=8.0" \ # Versão correspondente ao seu projeto dotnet. Em caso de dúvidas você pode consultar o arquivo .sln em 'TargetFramework'
  -D"sonar.verbose=true"
```

Por enquanto é isso. Em breve eu atualizo o projeto com a integração na pipeline do gitlab.
