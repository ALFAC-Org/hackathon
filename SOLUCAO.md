# Hackathon - Solução

> [!IMPORTANT]  
> Para melhor entendimento desta documentação, veja o vídeo abaixo:
> **Vídeo de explicação -> TODO**

## Para área de negócios

### Definição de requisitos

#### Requisitos funcionais

| Requisito | Solução |
| --- | --- |
| A nova versão do sistema deve processar mais de um vídeo ao mesmo tempo | A plataforma garantirá o processamento de múltiplos vídeos através da aplicação de filas e _lambdas_ (programa orientado a eventos). Uma vez que o vídeo é enviado para a plataforma, é publicada uma mensagem onde uma nova _lambda_ irá fazer o processamento do vídeo, registrando também uma nova mensagem ao terminar. Com isso, o sistema fica livre de gargalos por múltiplos processamentos. |
| Em caso de picos o sistema não deve perder uma requisição | A plataforma possui configurações dentro do _Kubernetes_ (aplicação para orquestração de _containers_ - aplicações) que fazem automaticamente, o escalonamento de toda a infraestrutura. Sendo necessário atender mais requisições, a plataforma automaticamente provisiona os recursos necessários. Uma vez que não são necessários, os recursos são reduzidos, preservando então o gasto financeiro.  |
| O Sistema deve ser protegido por usuário e senha | A plataforma só é acessível através de um cadastro prévio, com aceitação dos termos de uso, onde os recursos são acessados através de um _token_ (um símbolo de representação sob um conjunto de informações, representando uma chave de acesso com um tempo de expiração). Se algum recurso for acessado sem esse token, automaticamente a plataforma retornará acesso negado.|
| O fluxo deve ter uma listagem de status dos vídeos de um usuário | A plataforma contém um _endpoint_ (um endereço) onde o usuário poderá acessá-lo e ver a lista de vídeos relacionados a ele. É necessário ter realizado anteriormente, o _login_ (o acesso ao sistema através de usuário e senha) na plataforma. |
| Em caso de erro um usuário pode ser notificado (email ou um outro meio de comunicação) | A plataforma possui um sistema de notificação após qualquer erro no processamento do vídeo. O usuário então receberá um e-mail para que ele tente novamente. |

### Fluxo do usuário

#### Domain Driven Design (DDD)

[Documentação no Miro](https://miro.com/app/board/uXjVKZNCxxM=/?moveToWidget=3458764612404718846&cot=14)

## Para área de tecnologia

### Definição de requisitos técnicos

#### Requisitos não funcionais

| Requisito | Solução |
| --- | --- |
| O sistema deve persistir os dados | A plataforma irá persistir os dados em um banco de dados relacional, o `MySQL`. Serão persistidos os dados do usuário cadastrado, dados dos vídeos, dados sobre acesso (_logs_) entre outros.  |
| O sistema deve estar em uma arquitetura que o permita ser escalado | A plataforma possui suas responsabilidades separadas, onde pode-se constar: <ul><li>Código na arquitetura Hexagonal;</li><li>Repositórios independentes para aplicações e infraestrutura;</li><li>Plataforma orientada a eventos e filas de processamento;</li></ul> |
| O projeto deve ser versionado no Github | Links: <ul><li>Hackathon - [https://github.com/ALFAC-Org/hackathon](https://github.com/ALFAC-Org/hackathon)</li><li>Infraestrutura do Video Studio - [https://github.com/ALFAC-Org/video-cloud-infra](https://github.com/ALFAC-Org/video-cloud-infra)</li><li>Video Studio - [https://github.com/ALFAC-Org/video-studio](https://github.com/ALFAC-Org/video-studio)</li></ul> |
| O projeto deve ter testes que garantam a sua qualidade | Para isso, existe uma _Action_ (uma ação no GitHub) para a realização dos testes quem podem ser conferidos em [https://github.com/ALFAC-Org/video-studio/actions/workflows/unit-tests.yaml](https://github.com/ALFAC-Org/video-studio/actions/workflows/unit-tests.yaml) |
| CI/CD da aplicação | É possível verificar o CI/CD da aplicação em [https://github.com/ALFAC-Org/video-studio/actions](https://github.com/ALFAC-Org/video-studio/actions) |

### Arquitetura

#### Visão geral

A aplicação está estruturada no padrão hexagonal, de forma a organizar melhor o código, bem como possibilitar seu escalonamento.

A plataforma pode ser executada via `Docker`, `Kubernetes` ou `Terraform`.

Podendo ser hospedada tanto localmente ou na nuvem, usando serviços como AWS.

A interação da aplicação se dá através de _APIs_, conforme descrito na seção [Acesso e especificações de APIs](#acesso-e-especificações-de-apis).

Veja o [Diagrama no Miro](https://miro.com/app/board/uXjVKZNCxxM=/?moveToWidget=3458764613694478390&cot=14)

#### Infraestrutura em Cloud

A aplicação é provisionada através das configurações de `terraform` disponíveis no repositório [video-cloud-infra](https://github.com/ALFAC-Org/video-cloud-infra).

É provisionado:

- AWS EKS:
  - Services;
  - Deployments;
- Lambdas;
- VPCs;
- Grupos de segurança;
- Banco de dados
- Buckets AWS S3;

Entre outros. Veja o [Diagrama no Miro](https://miro.com/app/board/uXjVKZNCxxM=/?moveToWidget=3458764613706281574&cot=14).

#### Acesso e especificações de APIs

Uma vez provisionado a [infraestrutura](https://github.com/ALFAC-Org/video-cloud-infra) e a [aplicação](https://github.com/ALFAC-Org/video-studio), basta acessar a url fornecida na aplicação. O mesmo é demonstrado no vídeo de explicação.

### Repositórios

- Infraestrutura do Video Studio - [https://github.com/ALFAC-Org/video-cloud-infra](https://github.com/ALFAC-Org/video-cloud-infra);
- Video Studio - [https://github.com/ALFAC-Org/video-studio](https://github.com/ALFAC-Org/video-studio)

## Membros

| Nome | RM | E-mail | GitHub |
| --- | --- | --- | --- |
| Leonardo Fraga | RM354771 | [rm354771@fiap.com.br](mailto:rm354771@fiap.com.br) | [@LeonardoFraga](https://github.com/LeonardoFraga) |
| Carlos Henrique Carvalho de Santana | RM355339 | [rm355339@fiap.com.br](mailto:rm355339@fiap.com.br) | [@carlohcs](https://github.com/carlohcs) |
| Leonardo Alves Campos | RM355568 | [rm355568@fiap.com.br](mailto:rm355568@fiap.com.br) | [@lcalves](https://github.com/lcalves) |
| Andre Musolino | RM355582 | [rm355582@fiap.com.br](mailto:rm355582@fiap.com.br) | [@amusolino](https://github.com/amusolino) |
| Caio Antunes Gonçalves | RM354913 | [rm354913@fiap.com.br](mailto:rm354913@fiap.com.br) | [@caio367](https://github.com/caio367) |