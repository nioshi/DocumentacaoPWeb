# **Documentação do PWeb - senior**

## **Sumário**
<ul>
<li>
1. [Introdução](#id1)
<li> 1.1. [Tecnologias e linguagens utilizadas](#id1.1) </li>
</li>
</ul>
2. [Código-fonte](#id2)
 - 2.1. [Front-end](#id2.1)
   - 2.1.1. [HTML](#id2.1.1)
   - 2.1.2. [AngularJS](#2.1.2)
 - 2.2. [Back-end](#id2.2)
    - 2.2.1 [PHP](#id2.2.1)
     - 2.2.1.1 [LoginController](#id2.2.1.1)
      - 2.2.1.2 [ConfigController](#id2.2.1.2)
      - 2.2.1.3 [HoleriteController](#id2.2.1.3)
      - 2.2.1.4 [CartaoController e InformeController](#id2.2.1.4)
    - 2.2.1.5 [AdminController](#id2.2.1.5)
    - 2.2.1.6 [AtualizadorController](#id2.2.1.6)
   - 2.2.2 [Serviço windows](#id2.2.2)
    - 2.2.2.1 [C#](#id2.2.2.1)
3. [Instalações](#id3)
 - 3.1 [Front-end](#id3.1)
   - 3.1.1 [AngularJS](#id3.1.1)
   - 3.1.2 [Bootstrap](#id3.1.2)
 - 3.2 [Back-end](#3.2)
   - 3.2.1 [PHP](#id3.2.1)
   - 3.2.2 [C#](#id3.2.2)
   - 3.2.3 [SQLite](#id3.2.3)
   - 3.2.4 [Silex](#id3.2.4)
4. [Local de testes](#id4)
## **Introdução** 
<div id='id1'

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
  <div id='id1.1'
 O *front-end* da aplicação foi desenvolvido utilizando o framework do google [AngularJS 1.5.8](https://code.angularjs.org/1.7.5/docs/guide) para a linguagem JavaScript e o [Bootstrap 3.3](https://getbootstrap.com/docs/3.3/) para o Html e CSS, para o *back-end* a aplicação faz uso do [PHP](http://php.net/manual/pt_BR/index.php) para manipulação do banco de dados e o framework [silex/symphony](https://silex.symfony.com/doc/2.0/) que faz a ligação entre o Angular e PHP com requisições HTTP(POST e GET).
 
 Ainda no *back-end* existe o `Serviço windows` da aplicação desenvolvido com a linguagem C#, que facilita a comunicação com serviços web com o framework .NET, o serviço PWeb é utilizado para acessar o banco de dados no servidor senior, para garantir a integridade dos dados da versão da aplicação e tambem de módulos, o mesmo faz requisição quando o computador é ligado, ou caso o computador fique ligado, o serviço acessa a cada 24 horas.
/>
/> 

## **Código-fonte**
<div id='id2'

### Front-end
<div id='id2.1'

#### HTML
<div id='id2.1.1'

  O HTML é a view da nossa aplicação, é onde é mostrado todas as informações que chegam do angularJS para gerar os relatórios, o HTML faz um submit ao AngularJS que trás as informações ao mesmo.
  Exemplo de código para um botão:
  ```
  <div class="form-group">
     <button type="submit" ng-disabled="userForm.$invalid" class="btn btn-lg btn-primary btn-block"/>Enviar</button>
  </div>
  ```
  Nesse trecho de código, podemos ver o *Bootstrap*: ```<div class="form-group">``` e o *AngularJS*: ```ng-disabled=``` trabalhando junto com o HTML, o bootstrap nesse trecho é utilizado para colocar o botão em uma div ja formatada pelo framework, e o angularJS é utilizado para habilitar o botão apenas se todas as informações contidas nos campos dentro do *form* estejam válidas. Todas essas funções podem se encontradas na documentação de cada framework.
  
  Para os relatórios, o usuário terá uma view, que o mesmo escolherá a empresa e a competência para qual deseja gerar o relátorio.
  
  Existe tambem a view de Administrador, onde o mesmo pode:
  
  - Adicionar um novo usuário.
  - Remover um usuário.
  - Ver informações do usuário.
  - Alterar módulos de cada usuário.
/>  
  
#### AngularJS
<div id='id2.1.2'

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
/>
/>
 
 ### Back-end
 <div id='id2.2'
 
  #### PHP
  <div id='id2.2.1'
  
  No back-end existe um arquivo *controller* para cada relatório, assim como para o login,para a administração e para o serviço windows.
  
  ##### LoginController
  <div id='id2.2.1.1
  
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
   
   > Oracle
   
   ```
     $driver = $app['db']->getDriver();
  if($driver == "oci8"){
    $sql = "ALTER SESSION SET NLS_DATE_FORMAT = 'DD/MM/YYYY'";
    $app['db']->query($sql);
  }
  ```
  > SQL
  
  ```
  // se for SQL Server transforma em \Date() e deixa o Doctrine tratar
  else if($driver == "sqlsrv"){
    $accessData = date_create_from_format("d/m/Y",$accessData);
  }
  ```
  />
  
  ##### ConfigController
  <div id='id2.2.1.2
  
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
/>
   
   ##### HoleriteController
   <div id='id.2.2.1.3
   
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
  
   Os campos data no Holerite são convertidos no formato que a aplicação possa entender:
   ```
   $calculo['competencia'] = $calculo['competencia']->format('m/Y');
   $calculo['dataini'] = $calculo['dataini']->format('d/m/Y');
   $calculo['datafi'] = $calculo['datafi']->format('d/m/Y');
   ```
   />
   
   ##### CartaoController e InformeController
   <div id='id2.2.1.4
   
   O controller do cartao e do informe tem funções bem parecidas, o que diferencia um do outro é a URL de gestão que será acessada através do comando: ```$client = new SoapClient($urlGestao . "/g5-senior-services/ronda_Synccom_senior_g5_rh_hr_relatorios?wsdl");```, para o cartão ponto, e para o informe: ```$client = new SoapClient($urlGestao . "/g5-senior-services/ronda_Synccom_senior_g5_rh_hr_relatorios?wsdl");```, através de webServices, e a variável `$arguments`, para cada um tambem tem seu próprio array com as informações que irão aparecer no PDF gerado:
   > Cartão
   
   ```
   "prEntrada" => "<EDatInR=$dataini>"
        .  "<EDatFiR=$datafim>"
        .  "<EMarAfa='S'>"
	.  "<EMarFol='S'>"
	.  "<ELisDem='N'>"
        .  "<EAbrGPe='N'>"
	.  "<EAbrEmp=$empresa>"
        .  "<EAbrCad=$user>",
   ```
   
   > informe
   
   ```
    "prEntrada" => "<ENumCpf=$user>"
       .  "<EAnoBase=$ano>"
       .  "<ELisAut=1>",
   ```
					    
  O PHP retorna as informações ao AngularJS, que cria um PDF para visualização das informações:
  ```
      .success(function (data, status, headers, setting){
      // Decodifica string em base64 retira o espaço para compatibilidade
      var binary = atob(data.replace(/\s/g, ''));
      var len = binary.length;
      var buffer = new ArrayBuffer(len);
      var view = new Uint8Array(buffer);
      for (var i = 0; i < len; i++) {
          view[i] = binary.charCodeAt(i);
      }
    
      // Cria o blob especificando o mime-type, caso contrário somente o 
      // Chrome trata corretamente
      var newBlob = new Blob([view], {type: "application/pdf"})
    
      // Compatibilidade com o I.E. Não permite que utilizar um blob como href
      if (window.navigator && window.navigator.msSaveOrOpenBlob) {
        window.navigator.msSaveOrOpenBlob(newBlob);
        return;
      } 
      
      // Outros browsers: 
      // Cria o blob como URL
      var data_blob = window.URL.createObjectURL(newBlob);
      
      //Abre a janela

	  //foi usado o parametro _self, pois era o unico que abria janela no google chrome.
      newWindow.open(data_blob,"_self");
 ```
 Como segue no código acima, ele recebe as informações do back-end, e os converte para pdf, e abre uma janela com o PDF gerado do relatório, tanto para o cartão tanto para o informe.
 />
 
 ##### AdminController
 <div id='id2.2.1.5
 
   Esse controller é utlizado para buscar as informações do usuario no banco para mostrar na tabela da view, e tambem para alterar os modulos conforme escolha do administrador, a uma pequena diferença no acesso ao banco, ja que o mesmo não é SQL e sim [SQLite](https://www.sqlite.org/docs.html), apesar de parecerem diferentes, os comandos se tornam parecidos mudando apenas a sintaxe:
   ```
  $newdados = json_decode($_POST["x"], false);
  $db = new PDO('sqlite:C://Users//User//Documents//Projetos//pweb//Atualizador.db');
  $i = 0;
  while($newdados[$i] != null){
  $consulta = $db->prepare("UPDATE VersaoClientes SET modFolha = :modF, modInforme = :modI, modCartao = :modC WHERE nome = :names ");
  $consulta->bindParam(':modF', $newdados[$i]->modFolha, PDO::PARAM_STR, 50);
  $consulta->bindParam(':modI', $newdados[$i]->modInforme, PDO::PARAM_STR, 50);
  $consulta->bindParam(':modC', $newdados[$i]->modCartao, PDO::PARAM_STR, 50);
  $consulta->bindParam(':names', $newdados[$i]->nome, PDO::PARAM_STR, 50);
  $consulta->execute();
  $i = $i + 1;
  }
  ```
  A diferença do recebimento desse código dos demais ja mostrados, é que em vez do PHP enviar JSON, ele está recebendo, assim a forma de recebimento é um pouco diferente, como é visto na linha: `$newdados = json_decode($_POST["x"], false);`, sendo o envio do angularJS tambem diferente, nesse caso:
  ```
  $scope.GravarDados = function(){
      usuarios = JSON.stringify($scope.usuario);
      xmlhttp = new XMLHttpRequest();
      xmlhttp.open("POST", "gravardados", true);
      xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
      xmlhttp.send("x=" + usuarios);
    };
 ```
  onde:
  > `usuarios = JSON.stringify($scope.usuario);` transforma a informação em JSON
  
  > `xmlhttp.open("POST", "gravardados", true);` abre a requisição informando a forma de envio(POST), e a informação enviada.
  
  > `xmlhttp.send("x=" + usuarios);` envia a informação, com um caractere de detecção de pacote o `x=`, para o PHP saber onde começa a informação do JSON no pacote recebido.
  />
  
  ##### AtualizadorController
  <div id='id2.2.1.6
  
   esse controllador trabalha exclusivamente com o C# e o serviço windows e fica no servidor Senior, nele temos:
   - Verificar a versão do cliente e guardar a mesma no banco SQLite
   - Enviar o .zip com a versão nova caso esteja desatualizado
   - Enviar os modulos do usuario para o C#
   
   A forma de verificar a versão é bem simples, ele recebe as informações do usuario:
   ```
    $nome = $_POST['user'];
    $versao = $_POST['version'];
    $datas = $_POST['datas'];
   ```
   Ele compara o nome do mesmo que no banco é `unique`, se ja existir ele apenas faz um `Update` para atualizar a versão e data do acesso, caso o nome não existir, ele faz um `Insert` desse novo usuário:
   ```
     // CRIA O BD DO SQLite obs: precisa ser o caminho completo
  $db = new PDO('sqlite:C://Users//User//Documents//Projetos//pweb//Atualizador.db'); 
  //CONSULTA O BANCO PARA VER SE JA EXISTE O CLIENTE
  $consulta = $db->prepare("SELECT nome,versao FROM VersaoClientes WHERE nome = :nomes");
  $consulta->bindParam(':nomes', $nome, PDO::PARAM_STR, 50);
  $consulta->execute();
  $result = $consulta->fetchAll();

  foreach ($result as $retorno) {
  $retorno['nome'];
  $retorno['versao'];
  }
  ```
  Se a versão estiver desatualizada, o mesmo chama a rota `/baixar` o qual retorna um .zip com a versão atualizada para o C# que está no computador do cliente, o C# ja faz o trabalho de descompactar os arquivos na pasta da aplicação PWeb:
  ```
  $app->post('/baixar', function (Request $request) use ($app) {

  if(file_exists("C:/Users/User/Documents/Projetos/pweb/web.zip")){
	  //LIMPA O BUFFER DO PHP PARA NÂO ENVIAR O ARQUIVO CORROMPIDO
	  ob_clean();
    flush();

	  return $app->sendFile('C:/Users/User/Documents/Projetos/pweb/web.zip', 200, array('Content-Type' => "application/x-zip"));
  }
  else{
    return new Response("Arquivo version não existe!", 500);
  }
});
```
 Atenção para as linhas `ob_clean();` e `flush();`, pois sem elas, o PHP enviará lixo de mémoria com o .zip, e quando o pacote chegar no cliente o binário do arquivo está corrompido, pois terá informações no binário que não deveria estar junto.

 Lembrando que para o PHP conseguir enviar ou receber um arquivo pela rede dois parametros devem ser mudados logo no inicio do arquivo .PHP:
 ```
 ini_set('upload_max_filesize', '20M');
 ini_set('post_max_size', '20M');
 ```
 E ainda temos a rota do `modulose`, o qual pega os modulos contidos no banco de dados do servidor da Senior, e os envia para o serviço windows do cliente:
 ```
  //CRIA O BD DO SQLite obs: precisa ser o caminho completo
  $db = new PDO('sqlite:C://Users//User//Documents//Projetos//pweb//Atualizador.db'); 
  //CONSULTA O BANCO PARA VER OS MODULOS DO CLIENTE
  $nome = $_POST['user'];
  $consulta = $db->prepare("SELECT modFolha,modInforme,modCartao FROM VersaoClientes WHERE nome = :nomes");
  $consulta->bindParam(':nomes', $nome, PDO::PARAM_STR, 50);
  $consulta->execute();
  $result = $consulta->fetchAll();

  foreach ($result as $retorno) {
  $retorno['modFolha'];
  $retorno['modInforme'];
  $retorno['modCartao'];
  }
  return new Response(json_encode($retorno), 201);
  ```
  />
  
 #### Serviço windows
 <div id='id2.2.2'
 
 ##### C#
 <div id='id2.2.2.1'
 
   O serviço windows foi escrito com C# pois a linguagem facilita muito quando a questão é comunicação WEB e HTTP, graças a o framework da microsoft o .NET.
   
   A comunicação com o PHP é utilizando [webClient](https://docs.microsoft.com/pt-br/dotnet/api/system.net.webclient?view=netframework-4.7.2), o qual possibilita o envio e recebimento de informações por URL.
   
   É o C# que faz as requisições para o AtualizadorController ja explicado acima, ele envia as informações do cliente que estão em um arquivo, atraves de `NameValueCollection` para o PHP:
   ```
    NameValueCollection version = new NameValueCollection();
                version["user"] = user[0];
                version["version"] = versions[2];
                version["datas"] = date;
                versions[2] = versions[2].Trim();

                using (var client = new WebClient())
                {
                    result = client.UploadValues(URLEnviar, version);
                    resultasd = Encoding.UTF8.GetString(result);
                    resultasd = resultasd.Substring(3);
                }
  ```
  o PHP retorna se a versão está atualizada ou desatualizada, se estiver atualizada nada acontece e apenas um LOG é gerado, se estiver desatualizada o C# torna a fazer uma requisição mas agora para o `/baixar`, que o retornará com um arquivo .zip, o c# irá receber o mesmo, deletar a pasta da aplicação antiga, e instalar os arquivos da nova versão.
  
  O C# recebe o .zip através de binário:
  ```
  result = client.UploadValues(URLBaixar, mensagem);
     //LOCAL ONDE ARMAZENA O .ZIP
     FileStream arquivoBaixado = new FileStream("C://Users//User//Documents//Projetos//novaversao.zip", FileMode.OpenOrCreate);
      BinaryWriter binarioRecebido = new BinaryWriter(arquivoBaixado);
      binarioRecebido.Write(result);
 ```
  no método `sendResponse` existe a variável `acessarbd` o qual controla se o C# irá buscar no banco de dados do servidor da Senior ou apenas de um arquivo no computador do cliente, para não ficar fazendo requisições desnecessárias para o banco de dados. O arquivo é alimentado quando o C# faz requisição no banco de dados 1 vez por dia.
  
  > acessarbd = 0
  
  ```
  using (var client = new WebClient())
                    {
                        result = client.UploadValues(URLreceber, version);
                        resultado = Encoding.UTF8.GetString(result);
                        resultado = resultado.Substring(3);
                        JObject json = JObject.Parse(resultado);
                        dynamic results = JsonConvert.DeserializeObject<dynamic>(resultado);
                        modulos["modulos"] = results.modFolha + ";" + results.modInforme + ";" + results.modCartao;
                        //ESCREVE NO ARQUIVO DPS DE RECEBER DO BANCO
                        using (StreamWriter writer = new StreamWriter("C://Users//User//Documents//Projetos//teste//modulos.txt"))
                        {
                            writer.WriteLine(results.modFolha + ";" + results.modInforme + ";" + results.modCartao);
                        }
                        acessarbd = 1;
                        return modulos["modulos"];
              }
```

> acessarbd = 1

```
                  // LE O ARQUIVO E ENVIA PARA O PHP
                    using (StreamReader ler = new StreamReader("C://Users//User//Documents//Projetos//teste//modulos.txt"))
                    {
                        modulos["modulos"] = ler.ReadLine();
                    }

                    return modulos["modulos"];
```

para baixar o .zip segue a mesma lógica de variável, e as duas variáveis são resetadas quando:
- O timer de 24 horas estoura
- O computador é desligado

```
        public void OnTimer(object sender, System.Timers.ElapsedEventArgs args)
        {
            baixarArquivo();
            acessarbd = 0;
            baixarversao = 0;
        }

        protected override void OnShutdown()
        {
            base.OnShutdown();
            baixarversao = 0;
            acessarbd = 0;
        }
```

Dentro do método `OnStart(string[] args)` é onde fica o loop do serviço, enquanto o serviço estiver rodando no computador do cliente, esse método estará sendo executado, dentro dele é escrito o LOG de start do serviço, assim como é criado o [WebServer](https://developer.mozilla.org/pt-BR/docs/Learn/Common_questions/o_que_e_um_web_server) e iniciado o mesmo: `var ws = new WebServer(SendResponse, "http://localhost:12934/mod/");`, com a função que o webServer irá utilizar, e a URL onde o mesmo irá acessar.

Nesse método tambem temos o timer, o qual é utilizado para caso o computador não seja desligado, quando o mesmo estoura, as flags são resetadas, e a requisição de versão é feita, o código completo pode ser visto logo abaixo:
```
            var ws = new WebServer(SendResponse, "http://localhost:12934/mod/");
            ws.Run();
            mainT = new Thread(start: ws.Run);
            mainT.Start();
            System.Timers.Timer timer = new System.Timers.Timer();
            timer.Elapsed += new ElapsedEventHandler(OnTimer);
            timer.Interval = 86436000;
            timer.Enabled = true;
            timer.Start();
```
Tambem é utilizado thread para não ocorrer conflito em um evento muito raro onde vários usuários fazem requisição ao WebServer, se tiver apenas uma Thread, apenas 1 cliente por vez poderia acessar o banco de dados.
/>
/>
/>
/>

## Instalações
<div id='id3

### Front-end
<div id='id3.1'

#### AngularJS
<div id='id3.1.1'

 Para utilizar o AngularJS é bem simples, deve-se entrar no site oficial do [angular](https://angularjs.org/) e fazer o download da versão desejada; o Angular redirecionará para uma página como [essa](https://code.angularjs.org/1.5.8/angular.min.js), que conterá o código-fonte para ser utilizado no projeto, deve-se copiar esse codigo-fonte e colar em um arquivo chamado `angular.min.js`, o arquivo deve ser adicionado no diretório da aplicação - uma boa prática é deixar todos os arquivos .JS em uma única pasta - para chamar o script no projeto, deve ser utilizar:
 ```
 <script src="js/angular.min.js"></script>
 ```
 esse código segue para os outros script javascript.
 />
 
 #### Bootstrap
 <div id='id3.1.2'
 
  o Bootstrap oferece três formas de download e uma chamada CDN(essa em questão não precisa de download), para utilizar o CDN basta usar o seguinte código nos arquivos html:
  ```
<!-- Última versão CSS compilada e minificada -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

<!-- Tema opcional -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">

<!-- Última versão JavaScript compilada e minificada -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
  ```
  
  No PWeb é utilizada a 1ª opção de download, quando clicar em download irá baixar um .zip no computador, dentro desse .zip está compactados os arquivos do `.js`, `css` e do `fonts`, basta pegar esses arquivos e colocar no diretório da aplicação, novamente, uma boa prática é usar pastas com o mesmo nome(js,css,fonts).
  
  Para chamar esses arquivos na aplicação:
  > js
  ```
  <script src="js/bootstrap.min.js"></script>
  ```
  > CSS
  ```
  <link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
  ```
  />
  />
  
  ### back-end
  <div id='id3.2'
  
  #### PHP
  <div id='id3.2.1'
  
   O PHP para Windows pode ser baixado nesse [link](https://windows.php.net/download/), após baixar o .zip do mesmo, descompacta-se os arquivos em uma pasta no computador, exemplo: `C:\Program Files\php`, depois devemos colocar essa pasta em "PATH", para isso: ` botão windows + Pause Break` -> `Configurações avançadas do sistema` -> `Variáveis de ambiente`, selecione `Path` e clique em `Editar...` clique em `novo` e adicione o caminho do php, neste caso: `C:\Program Files\php`. depois é necessário reiniciar o computador para a variavel ser gravada.
   
   Pode-se ver o php entrado no prompt de comando(CMD) e digitando `php -v`.
  />
  
  #### C#
  <div id='id3.2.2'
  
  Para o C# foi utilizado o visual studio IDE da Microsoft pois com ele é possivel adicionar plugins à aplicação, ele pode ser baixado nesse [link](https://visualstudio.microsoft.com/).
  
  Com o Visual studio, podemos escolher quais pacotes de desenvolvimento queremos de acordo com nossa aplicação; No PWeb necessítamos dos seguintes pacotes de desenvolvimento:
   - Desenvolvimento para desktop com .NET
   - ASP.NET e desenvolvimento Web
   - Processamento e armazenamentos de dados.
   
  Depois de instalado o visual Studio, cria-se um novo projeto "Serviço windows", para entender como funcionam acesse [Dcoumentação serviços windows](https://docs.microsoft.com/pt-br/dotnet/framework/windows-services/walkthrough-creating-a-windows-service-application-in-the-component-designer), nesse projeto se deve importar 2 pacotes NuGet:
  - JSON
  - Newtonsoft.json
  
  Esses pacotes são usados para o C# receber e entender pacotes `JSON`, ja que o mesmo não entende nativamente.
  />
  
  #### SQLite
  <div id='id3.2.3'
  
  O SQLite do PWeb utiliza o SQLiteStudio, essa IDE facilita muito a criação do banco de dados SQLite, abre-se o programa, na barra de menu seleciona-se `Database` e depois `Add a database`, seleciona-se uma pasta onde ficará o arquivo.db e pronto; Com isso ja é possivel alimentar esse banco, manualmente, ou com scripts.
  />
  
  #### Silex
  <div id='id3.2.4'
  
  o Silex foi baixado usando o composer com o comando:
  ```
  composer require silex/silex "^2.0"
  ```
  
  Depois é só chamar o mesmo no arquivo PHP que utilizará ele, com o código:
  ```
  use Symfony\Component\HttpFoundation\Response;
  use Symfony\Component\HttpFoundation\Request;
  ```
  
   > OBS: O Silex 2.0 utilizado no PWeb foi descontinuado.
  />
  />
  />
  
  ## Local de testes
  <div id='id4'
  
  Os testes do PWeb são feitos na maquina local utilizando o Gerenciador de serviços da internet(IIS) do Windows.
  
  Com o IIS ja devidamente configurado, devemos clicar com o botão direito em `Sites > Default Web Site` e ir em `Adicionar diretório virtual`.
  
   - O campo "alias" é o nome do diretório, o qual será usado no endereço do navegador web para acessar à aplicação.
   - O caminho físico deverá ser `..\pweb\web`, que é a pasta na qual o endereço irá acessar, ou seja a pasta da aplicação PWeb.
   - O "Conectar como..." deve ser o usuário da máquina, selecione o check-box `Usuário específico`, depois `Definir...`, insira: Nome de usuário, e a senha da máquina e `Ok`, clique em: `Testar Configurações` se estiver tudo certo clique em `Ok`.
   
  Agora a aplicação web está devidamente configurada, para acessar a página abra o navegador web, e digite `http://localhost/nome do diretório virtual`.
  
  > OBS: Na documentação é levado em consideração que o Banco de dados, SQL ou Oracle, ja está configurado e rodando na máquina, assim como as configurações do IIS.
  />
