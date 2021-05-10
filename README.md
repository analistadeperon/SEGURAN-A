# SEGURAN-A
<div id="app"></div>

<div id="app"></div>

<div id="app"></div>

<div id="app"></div>

  <div class="wrapper" id="app">
    <div class="card-form">
      <div class="card-list">
        <div class="card-item" v-bind:class="{ '-active' : isCardFlipped }">
          <div class="card-item__side -front">
            <div class="card-item__focus" v-bind:class="{'-active' : focusElementStyle }" v-bind:style="focusElementStyle" ref="focusElement"></div>
            <div class="card-item__cover">
              <img
              v-bind:src="'https://www.mobills.com.br/blog/wp-content/uploads/2021/01/cartao-facil-santander-sx.png' + currentCardBackground + '.jpeg'" class="card-item__bg">
            </div>
            
            <div class="card-item__wrapper">
              <div class="card-item__top">
                <img src="https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/chip.png" class="card-item__chip">
                <div class="card-item__type">
                  <transition name="slide-fade-up">
                    <img v-bind:src="'https://www.mobills.com.br/blog/wp-content/uploads/2021/01/cartao-facil-santander-sx.png/' + getCardType + '.png'" v-if="getCardType" v-bind:key="getCardType" alt="" class="card-item__typeImg">
                  </transition>
                </div>
              </div>
              <label for="cardNumber" class="card-item__number" ref="cardNumber">
                <template v-if="getCardType === 'amex'">
                 <span v-for="(n, $index) in amexCardMask" :key="$index">
                  <transition name="slide-fade-up">
                    <div
                      class="card-item__numberItem"
                      v-if="$index > 4 && $index < 14 && cardNumber.length > $index && n.trim() !== ''"
                    >*</div>
                    <div class="card-item__numberItem"
                      :class="{ '-active' : n.trim() === '' }"
                      :key="$index" v-else-if="cardNumber.length > $index">
                      {{cardNumber[$index]}}
                    </div>
                    <div
                      class="card-item__numberItem"
                      :class="{ '-active' : n.trim() === '' }"
                      v-else
                      :key="$index + 1"
                    >{{n}}</div>
                  </transition>
                </span>
                </template>

                <template v-else>
                  <span v-for="(n, $index) in otherCardMask" :key="$index">
                    <transition name="slide-fade-up">
                      <div
                        class="card-item__numberItem"
                        v-if="$index > 4 && $index < 15 && cardNumber.length > $index && n.trim() !== ''"
                      >*</div>
                      <div class="card-item__numberItem"
                        :class="{ '-active' : n.trim() === '' }"
                        :key="$index" v-else-if="cardNumber.length > $index">
                        {{cardNumber[$index]}}
                      </div>
                      <div
                        class="card-item__numberItem"
                        :class="{ '-active' : n.trim() === '' }"
                        v-else
                        :key="$index + 1"
                      >{{n}}</div>
                    </transition>
                  </span>
                </template>
              </label>
              <div class="card-item__content">
                <label for="cardName" class="card-item__info" ref="cardName">
                  <div class="card-item__holder">Titular Do Cartão</div>
                  <transition name="slide-fade-up">
                    <div class="card-item__name" v-if="cardName.length" key="1">
                      <transition-group name="slide-fade-right">
                        <span class="card-item__nameItem" v-for="(n, $index) in cardName.replace(/\s\s+/g, ' ')" v-if="$index === $index" v-bind:key="$index + 1">{{n}}</span>
                      </transition-group>
                    </div>
                    <div class="card-item__name" v-else key="2">Nome Completo</div>
                  </transition>
                </label>
                <div class="card-item__date" ref="cardDate">
                  <label for="cardMonth" class="card-item__dateTitle">Validade</label>
                  <label for="cardMonth" class="card-item__dateItem">
                    <transition name="slide-fade-up">
                      <span v-if="cardMonth" v-bind:key="cardMonth">{{cardMonth}}</span>
                      <span v-else key="2">MM</span>
                    </transition>
                  </label>
                  /
                  <label for="cardYear" class="card-item__dateItem">
                    <transition name="slide-fade-up">
                      <span v-if="cardYear" v-bind:key="cardYear">{{String(cardYear).slice(2,4)}}</span>
                      <span v-else key="2">YY</span>
                    </transition>
                  </label>
                </div>
              </div>
            </div>
          </div>
          <div class="card-item__side -back">
            <div class="card-item__cover">
              <img
              v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + currentCardBackground + '.jpeg'" class="card-item__bg">
            </div>
            <div class="card-item__band"></div>
            <div class="card-item__cvv">
                <div class="card-item__cvvTitle">CVV</div>
                <div class="card-item__cvvBand">
                  <span v-for="(n, $index) in cardCvv" :key="$index">
                    *
                  </span>

              </div>
                <div class="card-item__type">
                    <img v-bind:src="'https://raw.githubusercontent.com/muhammederdem/credit-card-form/master/src/assets/images/' + getCardType + '.png'" v-if="getCardType" class="card-item__typeImg">
                </div>
            </div>
          </div>
        </div>
      </div>
      <div class="card-form__inner">
        <div class="card-input">
          <label for="cardNumber" class="card-input__label"></label>Numero do Cartão
          <input type="text" id="cardNumber" class="card-input__input" v-mask="generateCardNumberMask" v-model="cardNumber" v-on:focus="focusInput" v-on:blur="blurInput" data-ref="cardNumber" autocomplete="off">
        </div>
        <div class="card-input">
          <label for="cardName" class="card-input__label">Nome Impresso no Cartão</label>
          <input type="text" id="cardName" class="card-input__input" v-model="cardName" v-on:focus="focusInput" v-on:blur="blurInput" data-ref="cardName" autocomplete="off">
        </div>
        <div class="card-form__row">
          <div class="card-form__col">
            <div class="card-form__group">
              <label for="cardMonth" class="card-input__label">Data de Validade</label>
              <select class="card-input__input -select" id="cardMonth" v-model="cardMonth" v-on:focus="focusInput" v-on:blur="blurInput" data-ref="cardDate">
                <option value="" disabled selected>Mês</option>
                <option v-bind:value="n < 10 ? '0' + n : n" v-for="n in 12" v-bind:disabled="n < minCardMonth" v-bind:key="n">
                    {{n < 10 ? '0' + n : n}}
                </option>
              </select>
              <select class="card-input__input -select" id="cardYear" v-model="cardYear" v-on:focus="focusInput" v-on:blur="blurInput" data-ref="cardDate">
                <option value="" disabled selected>Ano</option>
                <option v-bind:value="$index + minCardYear" v-for="(n, $index) in 12" v-bind:key="n">
                    {{$index + minCardYear}}
                </option>
              </select>
            </div>
          </div>
          <div class="card-form__col -cvv">
            <div class="card-input">
              <label for="cardCvv" class="card-input__label">CVV</label>
              <input type="text" class="card-input__input" id="cardCvv" v-mask="'####'" maxlength="4" v-model="cardCvv" v-on:focus="flipCard(true)" v-on:blur="flipCard(false)" autocomplete="off">
            </div>
          </div>
        </div>

        <button class="card-form__button">
          Validar
        </button>
      </div>
    </div>
    
    
  </div>


</script>
</HEAD>


<!-- Script Size:  4.97 KB -->

<form action="">
	<label>Número do cartão</label>
	<input type="text" name="numCartao" id="numCartao">
</form>

<script type="text/javascript" src="http://code.jquery.com/jquery-2.1.3.min.js"></script>
<script type="text/javascript" src="https://assets.moip.com.br/v2/bank-account-validator.min.js"></script>
<script type="text/javascript">
  $(document).ready(function() {
    $("#validate_bank_account").click(function() {
      Moip.BankAccount.validate({
        bankNumber         : $("#bank_number").val(),
        agencyNumber       : $("#agency_number").val(),
        agencyCheckNumber  : $("#agency_check_number").val(),
        accountNumber      : $("#account_number").val(),
        accountCheckNumber : $("#account_check_number").val(),
        valid: function() {
          alert("Conta bancária válida")
        },
        invalid: function(data) {
          var errors = "Conta bancária inválida: \n";
          for(i in data.errors){
            errors += data.errors[i].description + "-" + data.errors[i].code + ")\n";
          }
          alert(errors);
        }
      });
    });
  });
</script> 
<form>
  <select id="bank_number">
    <option value="001">BANCO DO BRASIL S.A.</option>
    <option value="237">BANCO BRADESCO S.A.</option>
    <option value="341">BANCO ITAÚ S.A.</option>
    <option value="104">CAIXA ECONOMICA FEDERAL</option>
    <option value="033">BANCO SANTANDER BANESPA S.A.</option>
    <option value="399">HSBC BANK BRASIL S.A.</option>
    <option value="151">BANCO NOSSA CAIXA S.A.</option>
    <option value="745">BANCO CITIBANK S.A.</option>
    <option value="041">BANCO DO ESTADO DO RIO GRANDE DO SUL S.A.</option>
  </select>
 
  <input id="agency_number" placeholder="Agência" type="text"/>
  <input id="agency_check_number" placeholder="Dígito da agência" type="text" />
  <input id="account_number" placeholder="Conta corrente" type="text" />
  <input id="account_check_number" placeholder="Dígito da conta corrente" type="text" />
 
  <input type="button" value="Validar" id="validate_bank_account" />
</form>

<div id="form">
    Número do cartão: <input type="text" id="card_number"/>
    <br/>
    Nome (como escrito no cartão): <input type="text" id="card_holder_name"/>
    <br/>
    Mês de expiração: <input type="text" id="card_expiration_month"/>
    <br/>
    Ano de expiração: <input type="text" id="card_expiration_year"/>
    <br/>
    Código de segurança: <input type="text" id="card_cvv"/>
    <br/>
    <div id="field_errors">
    </div>
    <br/>
</div>
<form id="payment_form" action="https://seusite.com.br/transactions/new" method="POST">
    <input type="submit"></input>
</form>
function testCreditCard() {
  myCardNo = document.getElementById('CardNumber').value;
  myCardType = document.getElementById('CardType').value;
  if (checkCreditCard(myCardNo, myCardType)) {
    alert("Credit card has a valid format")
  } else {
    alert(ccErrors[ccErrorNo])
  };
}
<script src="https://www.braemoor.co.uk/software/_private/creditcard.js"></script>

<!-- COPIED THE DEMO CODE FROM TEH SOURCE WEBSITE (https://www.braemoor.co.uk/software/creditcard.shtml) -->

<table>
  <tbody>
    <tr>
      <td style="padding-right: 30px;">American Express</td>
      <td>3400 0000 0000 009</td>
    </tr>
    <tr>
      <td>Carte Blanche</td>
      <td>3000 0000 0000 04</td>
    </tr>
    <tr>
      <td>Discover</td>
      <td>6011 0000 0000 0004</td>
    </tr>
    <tr>
      <td>Diners Club</td>
      <td>3852 0000 0232 37</td>
    </tr>
    <tr>
      <td>enRoute</td>
      <td>2014 0000 0000 009</td>
    </tr>
    <tr>
      <td>JCB</td>
      <td>3530 111333300000</td>
    </tr>
    <tr>
      <td>MasterCard</td>
      <td>5500 0000 0000 0004</td>
    </tr>
    <tr>
      <td>Solo</td>
      <td>6334 0000 0000 0004</td>
    </tr>
    <tr>
      <td>Switch</td>
      <td>4903 0100 0000 0009</td>
    </tr>
    <tr>
      <td>Visa</td>
      <td>4111 1111 1111 1111</td>
    </tr>
    <tr>
      <td>Laser</td>
      <td>6304 1000 0000 0008</td>
    </tr>
  </tbody>
</table>
<hr /> Card Number:
<select tabindex="11" id="CardType" style="margin-left: 10px;">
  <option value="AmEx">American Express</option>
  <option value="CarteBlanche">Carte Blanche</option>
  <option value="DinersClub">Diners Club</option>
  <option value="Discover">Discover</option>
  <option value="EnRoute">enRoute</option>
  <option value="JCB">JCB</option>
  <option value="Maestro">Maestro</option>
  <option value="MasterCard">MasterCard</option>
  <option value="Solo">Solo</option>
  <option value="Switch">Switch</option>
  <option value="Visa">Visa</option>
  <option value="VisaElectron">Visa Electron</option>
  <option value="LaserCard">Laser</option>
</select> <input type="text" id="CardNumber" maxlength="24" size="24" style="margin-left: 10px;"> <button id="mybutton" type="button" onclick="testCreditCard();" style="margin-left: 10px; color: #f00;">Check</button>

{{$index + minCardYear}}
                </option>
              </select>
            </div>
          </div>
          <div class="card-form__col -cvv">
            <div class="card-input">
              <label for="cardCvv" class="card-input__label">CVV</label>
              <input type="text" class="card-input__input" id="cardCvv" v-mask="'####'" maxlength="4" v-model="cardCvv" v-on:focus="flipCard(true)" v-on:blur="flipCard(false)" autocomplete="off">
            </div>
          </div>
        </div>

        <button class="card-form__button">
          Validar
        </button>
      </div>
    </div>
    
    
  </div>
  <form name="frmTeste" method="post" action="#" onsubmit="return validaForm(this);">
  <p>Validação de formulário</p>
  <p>Nome: <input type="text" name="nome" id="nome" /></p>
  <p>Email: <input type="text" name="email" id="email" /></p>
  <p>Sexo:
    <label><input type="radio" name="sexo" value="M" id="masc" /> Masculino</label>
   <label><input type="radio" name="sexo" value="F" id="masc" /> Feminino</label>
  </p>
  <p>Profissão
    <select name="cargo">
      <option value="">Selecione o cargo</option>
      <option value="programador">Programador</option>
      <option value="designer">Designer</option>
      <option value="tester">Tester</option>
      <option value="todas">Todas</option>
   </select>
  </p><label><input type="submit" name="sbt" value="Enviar" /></label></p>
</form>
<form class="pure-form">
    <fieldset>
        <legend>Confirmação de Senha </legend>

        <input type="password" placeholder="Senha" id="password" required>
        <input type="password" placeholder="Confirme Senha" id="confirm_password" required>

        <button type="submit" class="pure-button pure-button-primary">Confirmar</button>
    </fieldset>
</form>
<script type="text/javascript" src="https://code.jquery.com/jquery-3.2.1.min.js"></script>

<!--Formulário-->
<form>
<label for="cep">CEP</label>
		<input id="cep" type="text" required/>
<label for="logradouro">Logradouro</label>
		<input id="logradouro" type="text" required/>
<label for="numero">Número</label>
		<input id="numero" type="text" />
<label for="complemento">Complemento</label>
		<input id="complemento" type="text"/>
<label for="bairro">Bairro</label>
		<input id="bairro" type="text" required/>
<label for="uf">Estado</label>
		<select id="uf">
			<option value="AC">Acre</option>
			<option value="AL">Alagoas</option>
			<option value="AP">Amapá</option>
			<option value="AM">Amazonas</option>
			<option value="BA">Bahia</option>
			<option value="CE">Ceará</option>
			<option value="DF">Distrito Federal</option>
			<option value="ES">Espírito Santo</option>
			<option value="GO">Goiás</option>
			<option value="MA">Maranhão</option>
			<option value="MT">Mato Grosso</option>
			<option value="MS">Mato Grosso do Sul</option>
			<option value="MG">Minas Gerais</option>
			<option value="PA">Pará</option>
			<option value="PB">Paraíba</option>
			<option value="PR">Paraná</option>
			<option value="PE">Pernambuco</option>
			<option value="PI">Piauí</option>
			<option value="RJ">Rio de Janeiro</option>
			<option value="RN">Rio Grande do Norte</option>
			<option value="RS">Rio Grande do Sul</option>
			<option value="RO">Rondônia</option>
			<option value="RR">Roraima</option>
			<option value="SC">Santa Catarina</option>
			<option value="SP">São Paulo</option>
			<option value="SE">Sergipe</option>
			<option value="TO">Tocantins</option>
		</select>
</form>
<script type="text/javascript">
	$("#cep").focusout(function(){
	//Aqui vai o código		
	});
</script>
<script type="text/javascript">
	$("#cep").focusout(function(){
		//Início do Comando AJAX
		$.ajax({
			//O campo URL diz o caminho de onde virá os dados
			//É importante concatenar o valor digitado no CEP
			url: 'https://viacep.com.br/ws/'+$(this).val()+'/json/unicode/',
			//Aqui você deve preencher o tipo de dados que será lido,
			//no caso, estamos lendo JSON.
			dataType: 'json',
			//SUCESS é referente a função que será executada caso
			//ele consiga ler a fonte de dados com sucesso.
			//O parâmetro dentro da função se refere ao nome da variável
			//que você vai dar para ler esse objeto.
			success: function(resposta){
				//Agora basta definir os valores que você deseja preencher
				//automaticamente nos campos acima.
				$("#logradouro").val(resposta.logradouro);
				$("#complemento").val(resposta.complemento);
				$("#bairro").val(resposta.bairro);
				$("#cidade").val(resposta.localidade);
				$("#uf").val(resposta.uf);
				//Vamos incluir para que o Número seja focado automaticamente
				//melhorando a experiência do usuário
				$("#numero").focus();
			}
		});
	});
</script>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="Criptografia Moip">
    <meta name="author" content="Moip">
    <title>Validação de contas bancárias</title>

    <link href='//fonts.googleapis.com/css?family=Open+Sans:400,700' rel='stylesheet' type='text/css'>
    <link href="https://moip.com.br/docs/stylesheets/screen.css" rel="stylesheet" type="text/css" media="screen" />
    <link href="https://moip.com.br/docs/stylesheets/print.css" rel="stylesheet" type="text/css" media="print" />
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/css/bootstrap-theme.min.css">

    <script type="text/javascript" src="http://code.jquery.com/jquery-2.1.3.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.4/js/bootstrap.min.js"></script>

    <script type="text/javascript" src="build/bank-account-validator.min.js"></script>

  </head>

  <script type="text/javascript">

    $(document).ready(function() {
      $("#validate_bank_account").click(function() {
        Moip.BankAccount.validate({
          bankNumber         : $("#bank_number").val(),
          agencyNumber       : $("#agency_number").val(),
          agencyCheckNumber  : $("#agency_check_number").val(),
          accountNumber      : $("#account_number").val(),
          accountCheckNumber : $("#account_check_number").val(),
          valid: function() {
            $("#success_message").removeClass('hide').fadeIn('slow');
            $("#error_message").fadeOut();
          },
          invalid: function(data) {
            var errors = "Ocorreram os seguintes erros:<br/>";
            for(i in data.errors){
              errors += "- " + data.errors[i].description + "<br/>";
            }
            $("#error_message").removeClass('hide').fadeIn('slow');
            $("#error_message").html(errors);
            $("#success_message").fadeOut();
          }
        });
      });
    });
  </script>

  <body>

    <div class="row">
      <h2 class="form-signin-heading" align="center">Bank Account Validator</h2>
      <hr>

      <div class="col-md-4">&nbsp;</div>

      <div class="col-md-4">

        <div id="success_message" class="alert alert-success hide">Conta Bancária válida</div>
        <div id="error_message" class="alert alert-danger hide"></div>

        <form class="form-signin">

          <div class="row">
            <div class="col-md-12">

              <label>Banco</label>
              <select id="bank_number" class="form-control">
                <option value="">Selecione o banco</option>
                <optgroup label="Principais bancos">
                  <option value="001">BANCO DO BRASIL S.A.</option>
                  <option value="237">BANCO BRADESCO S.A.</option>
                  <option value="341">BANCO ITAÚ S.A.</option>
                  <option value="104">CAIXA ECONOMICA FEDERAL</option>
                  <option value="033">BANCO SANTANDER BANESPA S.A.</option>
                  <option value="399">HSBC BANK BRASIL S.A.</option>
                  <option value="745">BANCO CITIBANK S.A.</option>
                </optgroup>
              </select>
            </div>
          </div>

          <br/>

          <div class="row">
            <div class="col-md-5">
              <label>Agência</label>
              <input id="agency_number" placeholder="Exemplo: 0170" maxlength="5" type="text" class="form-control" />
            </div>
            <div class="col-md-4">
              <label>Dígito</label>
              <input id="agency_check_number" placeholder="Exemplo: 8" maxlength="2" type="text" class="form-control" />
            </div>
          </div>

          <br/>

          <div class="row">
            <div class="col-md-5">
              <label>Conta corrente</label>
              <input id="account_number" placeholder="Exemplo: 97845" maxlength="12" type="text" class="form-control" />
            </div>
            <div class="col-md-4">
              <label>Dígito</label>
              <input id="account_check_number" placeholder="Exemplo: 1" maxlength="2" type="text" class="form-control" />
            </div>
          </div>

          <br/>

          <input type="button" value="Validar conta bancária" id="validate_bank_account" class="btn btn-lg btn-primary btn-block"/>

        </form>

      </div>
    </div>
    <div class="col-md-4"></div>
    <footer class="footer">
      <div class="container">
        <div class="text-muted pull-right">Powered by <b><a href="http://www.moip.com.br">Moip</a></b></div>
      </div>
    </footer>
    <div class="col-md-4"></div>
  <head>
   <title>CheckBoleto - validação de Boletos Bancários</title>
	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8"/>
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<meta name="keywords" content="checar boleto, validação de boletos, boletos maliciosos, vírus do boleto" />
	<meta property="og:title" content="CheckBoleto.com.br"/>
	<meta property="og:image" content="img/logo.jpg"/>
	<meta property="og:site_name" content="checkBoleto.com.br"/>
	<meta property="og:description" content="Validação de boletos bancários"/>
	<link href="img/logo.jpg" rel="shortcut icon">
   <div class="container2">
 
        <div class="navbar-header">
            <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
            <span class="sr-only">CheckBoleto</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#/" title='Validação de boletos bancários'>CheckBoleto</a>
        </div>
 
        <div class="navbar-collapse collapse">
            <ul class="nav navbar-nav">
		        <li><a href="#/sobre">Sobre</a></li>
				<li><a href="#/validar">Validar Boleto</a></li>
				<li><a href="#/virus">Vírus do Boleto</a></li>
				<li><a href="#/contato">Contato</a></li>
            </ul>
        </div><!--/.nav-collapse -->
    </div>
</div>
 </div>
                <div id="banking">
                    <span class="inv">Links para acesso direto a conta</span>
                    <label>Acesse sua conta:</label>
                    <a class="botoesBanking homebanking" href="#homebanking" onclick="ExecBanking('home')" value="Novo Home Banking" title="Home Banking">Home Banking</a>
                    <a class="botoesBanking officebanking" href="#officebanking" onclick="ExecBanking('office')" value="Office Banking" title="Office Banking">Office Banking</a>
                    <a class="botaoAjudaBanking" href="bobw00hn_central_atend.aspx?secao_id=15"></a>

                    <html>
<head>
<title>Tutorial de Alert em JavaScript - Linha de Código</title>
<script>
function funcao1()
{
alert("Eu sou um alert!");
}
</script>
</head>
<body>

<input type="button" onclick="funcao1()" value="Exibir Alert" />

<button onclick="funcao1()">Clique aqui</button>

<p id="demo"></p>

<script>
  <link rel="stylesheet" type="text/css" href="m2br.dialog.css" />
<script src="jquery.js"></script>
<script src="jquery.m2brdialog.pack.js"></script>
(document).ready(function(){
  $('a.link-alerta').m2brDialog({
      largura    : '300',
        altura    : '120',
        tipo    : 'alerta',
        titulo    : 'TESTE',
        texto    : 'Teste m2brDialog'
    });
});
$(document).ready(function(){
    $('.m2brdialog-pergunta').m2brDialog({
        tipo:     'pergunta',
        titulo:    'Confirme',
        texto:    'Tem certeza que deseja executar esta operação?',
        draggable: true,
        botoes: {
            1: {
                label: 'confirmar',
                tipo: 'link',
                endereco: 'https://github.com/analistadeperon'
            },
            2: {
                label: 'cancelar',
                tipo: 'fechar'
            }
        }
    });
});
