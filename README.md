# Deploy-de-Aplica-o-Utilizando-Container-no-Cloud-Run

Vou mostrar como compilar e implantar uma aplicação no **Google Cloud Run**. Primeiro, vamos entender os passos necessários:

1. **Escrever o código da aplicação**:
   - Para este exemplo, vou usar uma aplicação Node.js simples que responde com "Hello World". Podemos adaptar o código para a sua aplicação específica.
   - Crie um novo diretório chamado `helloworld`.
   - Dentro desse diretório, crie um arquivo `package.json` com as seguintes informações:
     ```json
     {
       "name": "helloworld",
       "description": "Simple hello world sample in Node",
       "version": "1.0.0",
       "private": true,
       "main": "index.js",
       "type": "module",
       "scripts": {
         "start": "node index.js"
       },
       "engines": {
         "node": ">=16.0.0"
       },
       "author": "Google LLC",
       "license": "Apache-2.0",
       "dependencies": {
         "express": "^4.17.1"
       }
     }
     ```
   - Crie um arquivo chamado `index.js` no mesmo diretório e adicione o seguinte código:
     ```javascript
     import express from 'express';

     const app = express();

     app.get('/', (req, res) => {
       const name = process.env.NAME || 'World';
       res.send(`Hello ${name}!`);
     });

     const port = parseInt(process.env.PORT) || 8080;
     app.listen(port, () => {
       console.log(`helloworld: listening on port ${port}`);
     });
     ```
   - Este código cria um servidor web básico que escuta na porta definida pela variável de ambiente `PORT`.

2. **Compilar a aplicação em uma imagem de contêiner**:
   - Certifique-se de ter o **Docker** instalado em sua máquina.
   - Abra um terminal e navegue até o diretório `helloworld`.
   - Execute o seguinte comando para criar uma imagem de contêiner:
     ```
     docker build -t gcr.io/PROJECT_ID/helloworld .
     ```
     Substitua `PROJECT_ID` pelo ID do seu projeto no Google Cloud.

3. **Enviar a imagem para o Container Registry**:
   - Faça o login no Google Cloud usando o comando `gcloud auth login`.
   - Execute o seguinte comando para enviar a imagem para o Container Registry:
     ```
     docker push gcr.io/PROJECT_ID/helloworld
     ```

4. **Implantar a aplicação no Cloud Run**:
   - Acesse o [Google Cloud Console](https://console.cloud.google.com/).
   - Clique em "Cloud Run" no menu lateral.
   - Clique em "Criar serviço" e preencha os detalhes:
     - Nome do serviço: `helloworld-service`
     - Região: escolha uma região próxima a você
     - Imagem do contêiner: `gcr.io/PROJECT_ID/helloworld`
     - Configurações avançadas: defina os parâmetros de CPU e memória conforme necessário.
   - Clique em "Criar" para implantar a aplicação.

Agora temos uma aplicação Node.js implantada no Google Cloud Run! 

Vou te explicar com mais detalhes sobre o **Google Cloud Run**.

O **Cloud Run** é uma plataforma gerenciada de computação que permite executar **contêineres** diretamente na infraestrutura escalável do Google. Aqui estão os principais pontos sobre o Cloud Run:

1. **Serverless e Abstração de Infraestrutura**:
   - O Cloud Run é **serverless**, o que significa que ele abstrai toda a gestão de infraestrutura. Você pode se concentrar no que realmente importa: construir ótimas aplicações.
   - Ele permite que você execute código escrito em qualquer linguagem de programação, desde que você possa criar uma **imagem de contêiner** a partir dele. Na verdade, a criação de imagens de contêiner é opcional.
   - Se você estiver usando Go, Node.js, Python, Java, .NET Core ou Ruby, pode usar a opção de **implantação baseada em código-fonte**, que cria o contêiner para você, seguindo as melhores práticas da linguagem que você está usando.

2. **Execução de Serviços e Jobs**:
   - No Cloud Run, seu código pode ser executado continuamente como um **serviço** ou como um **job**.
   - Ambos os serviços e jobs rodam no mesmo ambiente e podem usar as mesmas integrações com outros serviços no Google Cloud.
   - **Serviços**: São usados para executar código que responde a requisições web ou eventos.
   - **Jobs**: São usados para executar código que realiza um trabalho específico e encerra quando o trabalho é concluído.

3. **Principais Recursos dos Serviços no Cloud Run**:
   - **Endpoint HTTPS Único**: Cada serviço do Cloud Run recebe um **endpoint HTTPS** em um subdomínio único do domínio `*.run.app`. Você também pode configurar domínios personalizados.
   - **Autoescalonamento Rápido Baseado em Requisições**: O Cloud Run escala automaticamente para lidar com todas as requisições recebidas ou para lidar com aumento de utilização de CPU fora das requisições, se a alocação de CPU estiver definida como "sempre ligada".
   - **Gerenciamento de Tráfego Integrado**: Cada implantação cria uma nova revisão imutável. Você pode rotear o tráfego para a revisão mais recente, fazer rollback para uma revisão anterior ou dividir o tráfego entre várias revisões para realizar uma implantação gradual.
   - **Serviços Públicos e Privados**: Um serviço pode ser acessível pela internet ou restringido com políticas de acesso usando o **Cloud IAM**.

4. **Benefícios do Cloud Run**:
   - **Execução Totalmente Gerenciada na Nuvem**: Você não precisa criar um cluster ou gerenciar infraestrutura para ser produtivo com o Cloud Run.
   - **Pagamento Apenas pelos Recursos Utilizados**: Você paga apenas pelo que usa.
   - **Testes Mais Consistentes**: Realize testes de forma eficiente.
   - **Solicitações Simultâneas**: O Cloud Run lida com várias solicitações simultâneas.

Em resumo, o Cloud Run permite que os desenvolvedores se concentrem em escrever código e gastem muito pouco tempo operando, configurando e escalando seus serviços. É uma ótima opção para implantar aplicações escaláveis e de alto desempenho.
