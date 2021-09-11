## Microservices Python e gRPC

### Microservices

1.  Marketplace: Um aplicativo web com `Flask` que exibe uma lista de livros para o usuário.
2.  Recommendations: Um serviço que implementa um servidor `gRPC` que fornece uma lista de livros nos quais o usuário pode estar interessado.

Os usuários interagem com o micro-serviço do `Marketplace` por meio do navegador, e o micro-serviço do `Marketplace` interage com o micro-serviço `Recommendations` via gRPC.

#### Adicionando autenticação e criptografia TLS - Gerando CA (certificate authority)

Isso não se aplica à autenticação de usuários, apenas na comunicação dos micros-serviços. Implementa TLS mútuo em que o cliente e o servidor se validam.

#### Cria um certificado CA que é utilizado para assinar o certificado do servidor e cliente

```
openssl req -x509 -nodes -newkey rsa:4096 -keyout ca.key -out ca.pem -subj /O=me
```

Nos arquivos Dockerfile`s já têm os comandos para criar os certificados que será assinados com o certificado gerado pelo comando acima.

#### Cliente

```
docker build . -f marketplace/Dockerfile  -t marketplace --secret id=ca.key,src=ca.key
```

#### Servidor

```
docker build . -f recommendations/Dockerfile  -t recommendations --secret id=ca.key,src=ca.key
```

**_Obs:_** No momento em que foi realizados os testes o docker-compose não é compatível com as construção `secrets`. Deve fazer o build das imagens Docker manualmente e executar apenas `docker-compose up` sem o `--build`.
