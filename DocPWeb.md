# **Documentação do PWeb - senior**

## **Introdução**

  PWeb é uma aplicação Web desenvolvida na Senior unidade do contestado, com o intuito de facilitar a geração de relatórios cotidianos para as empresas, esses relatórios são: 
  ```
  > Folha de pagamento
  > Informe de rendimentos
  > Cartão ponto
  ```
  O mesmo contem uma interface muito simples e de fácil entendimento para qualquer usuário, com um design responsívo, o PWeb tambem pode ser acessado de qualquer lugar atráves de um smartphone ou dispositivo móvel com acesso a internet atráves do aplicativo, disponibilizado na Google play.
  
  o PWeb foi desenvolvido por:
  - Gabriel Cândido
  - Matheus Bazzo

  ### Tecnologias e linguagens utilizadas
  
 O *front-end* da aplicação foi desenvolvido utilizando o framework do google [AngularJS 1.5.8](https://code.angularjs.org/1.7.5/docs/guide) para a linguagem JavaScript e o [Bootstrap 3.3](https://getbootstrap.com/docs/3.3/) para o Html e CSS, para o *back-end* a aplicação faz uso do [PHP](http://php.net/manual/pt_BR/index.php) para manipulação do banco de dados e o framework [silex/symphony](https://silex.symfony.com/doc/2.0/) que faz a ligação entre o Angular e PHP com requisições HTTP(POST e GET).
 
 Ainda no *back-end* existe o `Serviço windows` da aplicação desenvolvido com a linguagem C#, que facilita a comunicação com serviços web com o framework .NET, o serviço PWeb é utilizado para acessar o banco de dados no servidor senior, para garantir a integridade dos dados da versão da aplicação e tambem de módulos, o mesmo faz requisição quando o computador é ligado, ou caso o computador fique ligado, o serviço acessa a cada 24 horas.
 
## **Código-fonte**

### Front-end

#### HTML

  O HTML é a view da nossa aplicação, é onde é mostrado todas as informações que chegam do angularJS para gerar os relatórios, o HTML faz um submit ao AngularJS que trás as informações ao mesmo.
  Exemplo de código para um botão:
  ```
  <div class="form-group">
     <button type="submit" ng-disabled="userForm.$invalid" class="btn btn-lg btn-primary btn-block"/>Enviar</button>
  </div>
  ```
  Nesse trecho de código, podemos ver o *Bootstrap*: ```<div class="form-group">``` e o *AngularJS*: ```ng-disabled=``` trabalhando junto com o HTML, o bootstrap nesse trecho é utilizado para colocar o botão em uma div ja formatada pelo framework, e o anjularJS é utilizado para habilitar o botão apenas se todas as informações contidas nos campos submit estejam válidas. Todas essas funções podem se encontradas na documentação de cada framework.
