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
  Nesse trecho de código, podemos ver o *Bootstrap*: ```<div class="form-group">``` e o *AngularJS*: ```ng-disabled=``` trabalhando junto com o HTML, o bootstrap nesse trecho é utilizado para colocar o botão em uma div ja formatada pelo framework, e o angularJS é utilizado para habilitar o botão apenas se todas as informações contidas nos campos dentro do *form* estejam válidas. Todas essas funções podem se encontradas na documentação de cada framework.
  
#### AngularJS

  Como ja explicado nesse documento, o angularJS é um framework construido pela Google para facilitar a comunicação do HTML com a linguagem JavaScript, deixando o codigo HTML mais limpo e de facil entendimento.
  Começando com **config.js**, ele é utilizado para pegar as informações de configuração da view e enviar ao PHP para o mesmo escrever em um arquivo, para posteriormente utilizar essas mesmas configurações na aplicação, exemplo de código:
  ```
      var setting = {
      headers : {
        'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8;'
      }
    }

    // post para a rota que cria o arquivo
    $http.post('./dbconfig', data, setting)
    .success(function (data, status, headers, setting){
      $scope.PostDataResponse = data;
      $window.location.href = '/pweb';
    })
    .error(function (data, status, header, setting){
      $scope.ResponseDetails = "Erro: " + data ;
    });
  };
  ```
  onde, *setting* é o formato do dado que será enviado e recebido, e o *data* é a variável que pode enviar uma informação ao back-end, nesse caso é nossas variaveis de configuração, e a mesma vai receber a resposta do *response*, essa resposta pode ser informações para serem utilizadas na view, ou apenas um "ok".
  
  quando o *back-end* responder com sucesso, o `.success` é chamado e o mesmo seta a variavel ```$scope.PostDataResponse``` com o *data* e redireciona o usuário para a view principal através do ```$window.location.href = '/pweb'```, caso retorne um erro, o erro é mostrado na tela através do `.error`
  
  ainda nesse sentido temos o **print.js** e o **salvarPDF.js** que são utilizados utilizados para imprimir ou salvar em pdf o *folha de pagamento*.
  
  entrando nos *modules*, temos os arquivos .JS que cuidam da parte de requisição dos relatórios, parte administrativa e da interface do menu da aplicação. Todos seguem a mesma lógica, fazer uma requisição ao back-end(PHP), enviar os dados necessários e esperar um response com as informações que serão utilizadas para um fim.
  
  Essas informações são utilizadas para serem mostradas na view da aplicação, exemplo: ```ng-src="data:image/png;base64,[[logo.logemp]]```, nesse caso é mostrado o logo da empresa na Folha de pagamento, onde a informação da logo é guardada em uma variável como no exemplo abaixo:
  ```
     .success(function (data, status, headers, setting){
      $scope.logo = angular.fromJson(data);
      ngProgress.complete();
    })
  ```
  ou ainda, para fazer uma comparação lógica dentro do próprio JS, como segue abaixo:
  ```
      if($scope.quantidadeContratos > 1){
      $scope.contratos = angular.fromJson(data).contratos;  
	  
    }
 ```
 
 Como ja mostrado no código da configuração, a requisição HTTP é feita através de "." seguido do endereço dessa requisição no back-end que no caso da configuração é */dbconfig*, ficando: ```$http.post('./dbconfig', data, setting)```, no lado do back-end o endereço vai ser apenas */dbconfig*, a requisição pode ser POST ou GET, mas isso deve ser feito nos dois lado da aplicação, no `AngularJS` e no `PHP`, lembrando que a requisição é feita dessa forma na aplicação por causa da forma que o Silex usa as rotas, essa forma pode mudar de acordo com o framework utilizado, mais informações podem ser encontradas na documentação do [Silex 2.0](https://silex.symfony.com/doc/2.0/)
 
 ### Back-end
 
  #### PHP
  
  No back-end existe um arquivo *controller* para cada relatório, assim como para o login,para a administração e para o serviço windows.
  
  ##### LoginController
  no `LoginController.php`, temos as lógicas para o login do usuario, seja pela view do login padrão, ou seja pela plataforma da senior, o SeniorX, assim como *logout*, e *mudarsenha*.
  o PHP recebe a requisição do `front-end(AngularJS)` com as informações que foi informado na view de login, e as utiliza para buscar esse usuário que está pretendendo logar na aplicação,que será mostrado logo abaixo; Todas as informações recebidas pelo front-end através da variavel `data`, são pegas através de: ```$request->get('variavelDesejada')```, sendo `variavelDesejada` o mesmo nome da variavel no angularJS.
  ```
  $sql = "SELECT USU_CODUSU, USU_SENUSU, USU_ALTSEN, USU_QTDTEN, 
                 USU_BLOUSU, USU_EMPUSU
            FROM USU_TUSUPOR, R034FUN
           WHERE USU_CODUSU = ?
             AND USU_EMPUSU = NUMEMP
             AND USU_TIPUSU = TIPCOL
             AND TIPCOL = 1
             AND NUMCPF = USU_CODUSU
             AND SITAFA <> 7";
	
  $stmt = $app['db']->prepare($sql);
  $stmt->bindValue(1, $user);
  $stmt->execute();
  
  $userData = $stmt->fetchAll();
  ```
   > `$sql` é a variavel que contem a linha do sql
   
   > `$stmt = $app['db']->prepare($sql);` atribui o comando à variavel `$stmt`.
   
   > `$stmt->bindValue(1, $user);` substitui o caracter "?" na linha do `$sql`, onde 1 é a posição do caracter e o $user, é a variavel que irá substitui-lo.
   
   > `$stmt->execute();` executa todos esses parametros.
   
   > `$userData = $stmt->fetchAll();` recebe o retorno do SQL e o guarda em uma variável, nesse caso em específico ele recebe todos os parametros do select em um array, por causa do método ` fetchAll();`, se quisessemos apenas uma varíavel do select poderiamos utilizar-lo assim: `fetchAll()[0]['Variavel desejada']` sendo, `variavel desejada` igual ao nome que está no `$sql`.
   
   A senha quando chega é criptografada: ```$password = base64_encode($request->get('password'));```, e enviada para o banco de dados, ou seja, não tem como saber qual é a senha desse usuário, para recuperar a mesma em caso de perda, o mesmo receberá uma senha temporária e logo após logar no PWeb, deverar mudar a mesma.
   
   A questão de campos data, é tratado diferente para cada banco, SQL ou Oracle, como segue nos códigos abaixo:
   ```
     $driver = $app['db']->getDriver();
  if($driver == "oci8"){
    $sql = "ALTER SESSION SET NLS_DATE_FORMAT = 'DD/MM/YYYY'";
    $app['db']->query($sql);
  }
  // se for SQL Server transforma em \Date() e deixa o Doctrine tratar
  else if($driver == "sqlsrv"){
    $accessData = date_create_from_format("d/m/Y",$accessData);
  }
  ```
  
  ##### ConfigController
  
   O configController.php é o responsável para escrever as configurações da aplicação em um arquivo como explicado anteriormente
   ```
     if($dbserver != "" && $dbuser != "" && $db != "" && $driver != ""){
    $file = fopen("../app/config/dbvar.php","w");

    $string = "<?PHP \$dbserver = '$dbserver'; \n \$dbuser = '$dbuser'; \n \$dbpassword = '$dbpassword'; \n \$db = '$db';\n \$driver = '$driver' \n ?>";

    fwrite($file,$string);
    fclose($file);
  }
  ```
  e tambem para receber os módulos habilitados e desabilitados da aplicação para cada usuário.
  
  O C#, que será explicado mais para frente, envia uma string com informações dos módulos a um endereço URL, e o PHP, acessa esse mesmo endereço utilizando o [guzzlehttp](http://docs.guzzlephp.org/en/stable/) e recebe essa informação para utiliza-la para fazer uma condição lógica dentro dele, ele faz a comparação, guarda as informações dentro de um array de string de acordo com cada condição, e envia ao angularJS por meio de [JSON](http://www.json.org/), como pode ser visto no código simplificado:
  ```
  	$curl = curl_init();
	// seta a url para onde será a requisição
	curl_setopt($curl, CURLOPT_URL, 'http://localhost:12934/mod/');
	// seta para receber a resposta em string
	curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
	// Envia a requisição e salva a resposta
	$response = curl_exec($curl);
	// divide a informção em 3 strings novas
	$modulos = explode(";", $response);

	if($modulos[0] == 'sim'){
		$moduloscaminho['folha'] = './holerite.html';
	}else{
		$moduloscaminho['folha'] = '';
	}
		return new Response(json_encode($moduloscaminho), 201);
		// Fecha a requisição e limpa a memória
		curl_close($curl);
```
   
   ##### HoleriteController
   
   O HoleriteController é o responsável pela folha de pagamento, ele a principio é bem simples, ele recebe a requisição do `front-end`,  faz a busca das informações no banco de dados, igual ao exemplo do login, de acordo com o usuario e a empresa guardados na variável de sessão:```$app['session']->get('user')```, ```$app['session']->get('empresa')``` e os reenvia para o angularJS através de JSON para serem mostrados na view da aplicação em forma de divs, como no trecho de exemplo:
   ```
   						<div class="row">
							<div class="col-md-5 col-sm-5">
								<label for="Competencia">Mes/Ano</label>
									<p>
										[[holerite.competencia | date: 'MM/yyyy']]
									</p>
							</div>
							<div class="col-md-5 col-sm-5">
								<label for="Empresa">Empresa</label>
								<p>
									[[holerite.empresa.nomeEmpresa]]
								</p>
							</div>
  ```
  
   a os campos data no Holerite são convertidos no formato que a aplicação possa entender:
   ```
   $calculo['competencia'] = $calculo['competencia']->format('m/Y');
   $calculo['dataini'] = $calculo['dataini']->format('d/m/Y');
   $calculo['datafi'] = $calculo['datafi']->format('d/m/Y');
   ```
   
   ##### CartaoController e InformeController
   
   
