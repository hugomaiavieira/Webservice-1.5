---
title: Integração Webservice 1.5

language_tabs:
  - xml: XML

toc_footers:
  - <a href='/Webservice-1.5-FAQ'>Perguntas frequentes</a>
  - <a href='/Webservice-1.5-Processamento-em-lote/'>Processamento em lote</a>
  - <a href='/Webservice-1.5-Pagamento-com-celular/'>Pagamento com celular</a>

search: true
---

# Integração Webservice 1.5

O objetivo desta documentação é orientar o desenvolvedor sobre como integrar com o Webservice da Cielo, descrevendo as funcionalidades, os métodos a serem utilizados, listando informações a serem enviadas e recebidas, e provendo exemplos.

O mecanismo de integração com o Cielo E-commerce é simples, de modo que apenas conhecimentos intermediários em linguagem de programação para Web, requisições HTTP/HTTPS e manipulação de arquivos XML, são necessários para implantar a solução Cielo E-commerce com sucesso.

Para realizar testes no ambiente de homologação não há a necessidade de se credenciar na Cielo, porém para vender pela internet, é necessário se credenciar na Cielo no seguinte endereço: [https://www.cielo.com.br/sitecielo/e-commerce/credenciamento-ecommerce.html](https://www.cielo.com.br/sitecielo/e-commerce/credenciamento-ecommerce.html).

Após a conclusão do credenciamento e recebimento das instruções é preciso desenvolver a integração utilizando como guia este manual. Assim que a integração estiver concluída, é necessário preencher completamente o formulário de homologação e enviá-lo para o Suporte Web do Cielo E-commerce que informará ao estabelecimento a chave de segurança.

Por fim, após o término do desenvolvimento, é preciso dar início à homologação junto à Cielo
para iniciar a operação no ambiente de produção.

<aside class="notice">Veja a seção [Homologação](#Homologacao) para instruções sobre o processo de homologação</aside>

## Suporte Cielo

Após a leitura deste manual, caso ainda persistam dúvidas (técnicas ou não), a Cielo disponibiliza o suporte técnico 24 horas por dia, 7 dias por semana em idiomas (Português e Inglês), nos seguintes contatos:

* +55 4002-9700 – *Capitais e Regiões Metropolitanas*
* +55 0800-570-1700 – *Demais Localidades*
* +55 11 2860-1348 – *Internacionais*
  * Opção 1 – *Suporte técnico;*
  * Opção 2 – *Credenciamento E-commerce.*
* Email: [cieloecommerce@cielo.com.br](mailto:cieloecommerce@cielo.com.br)

## Glossário

Para facilitar o entendimento, listamos abaixo um pequeno glossário com os principais termos relacionados ao E-commerce, ao mercado de cartões e adquirencia:

* **Autenticação**: processo para assegurar que o comprador é realmente aquele quem diz ser (portador legítimo), geralmente ocorre no banco emissor com uso de um token digital ou cartão com chaves de segurança.
* **Autorização**: processo para verificar se uma compra pode ou não ser realizada com um cartão. Nesse momento, são feitas diversas verificações com o cartão e com o portador (ex.: adimplência, bloqueios, etc.) É também neste momento que o limite do cartão é sensibilizado com o valor da transação.
* **Cancelamento**: processo para cancelar uma compra realizada com cartão.
* **Captura**: processo que confirma uma autorização que foi realizada previamente. Somente após a captura, é que o portador do cartão poderá visualizá-la em seu extrato ou fatura.
* **Chave de acesso**: é um código de segurança específico de cada loja, gerado pela Cielo, usada para realizar a autenticação e comunicação em todas as mensagens trocadas com a Cielo. Também conhecido como chave de produção e key data.
* **Comprador**: é o aquele que efetua compra na loja virtual.
* **Emissor (ou banco emissor)**: É a instituição financeira que emite o cartão de crédito, débito ou voucher.
* **Estabelecimento comercial ou EC**: Entidade que responde pela loja virtual.
* **Gateway de pagamentos**: Empresa responsável pelo integração técnica e processamento das transações.
* **Número de credenciamento**: é um número identificador que o lojista recebe após seu credenciamento junto à Cielo.
* **Portador**: é a pessoa que tem o porte do cartão no momento da venda.
* **SecureCode**: programa internacional da Mastercard para possibilitar a autenticação do comprador no momento de uma compra em ambiente E-commerce.
* **TID (Transaction Identifier)**: código composto por 20 caracteres que identificada unicamente uma transação Cielo E-commerce.
* **Transação**: é o pedido de compra do portador do cartão na Cielo.
* **VBV (Verified by Visa)**: Programa internacional da Visa que possibilita a autenticação do comprador no momento de uma compra em ambiente E-commerce.

<aside class="notice">Acesse http://www.mastercard.com.br/securecode para mais detalhes sobre o SecureCode.</aside>

<aside class="notice">Acesse http://www.verifiedbyvisa.com.br para mais detalhes sobre o VBV.</aside>

## Produtos e Bandeiras suportadas

A versão atual do Webservice Cielo possui suporte às seguintes bandeiras e produtos:

|Bandeira|Crédito à vista|Crédito parcelado Loja|Débito|Voucher|
|--------|---------------|----------------------|------|-------|
|Visa|Sim|Sim|Sim|*Não*|
|Master Card|Sim|Sim|Sim|*Não*|
|American Express|Sim|Sim|*Não*|*Não*|
|Elo|Sim|Sim|*Não*|*Não*|
|Diners Club|Sim|Sim|*Não*|*Não*|
|Discover|Sim|*Não*|*Não*|*Não*|
|JCB|Sim|Sim|*Não*|*Não*|
|Aura|Sim|Sim|*Não*|*Não*|

# Visão Geral

Neste manual será apresentado uma visão geral do Cielo E-commerce e o mecanismo tecnológico no formato de integração Webservice (chamado nas versões anteriores de Buy Page Loja).

Para informações sobre a integração no formato do Checkout Cielo (chamado nas versões anteriores de Buy Page Cielo ou Solução Integrada) acesse: [https://www.cielo.com.br/ecommerce](https://www.cielo.com.br/ecommerce).

Para todo pedido de compra, a meta é efetivá-la em uma venda. Uma venda com cartão pode ser caracterizado em uma transação autorizada e capturada.

<aside class="warning">Uma transação autorizada somente gera o crédito para o lojista se ela for capturada (ou confirmada).</aside>

## Características da solução

A solução Webservice da plataforma Cielo E-commerce foi desenvolvida com tecnologia XML, que é padrão de mercado e independe da tecnologia utilizada por nossos clientes. Dessa forma, é possível integrar-se utilizando as mais variadas linguagens de programação, tais como: ASP, ASP. Net, Java, PHP, Ruby, Python, etc.

Entre outras características, os atributos que mais se destacam na plataforma Cielo E-commerce:

* **Ausência de aplicativos proprietários**: não é necessário instalar aplicativos no ambiente da loja virtual em nenhuma hipótese.
* **Simplicidade**: o protocolo utilizado é puramente o HTTPS, sem necessidade do uso de SOAP.
* **Facilidade de credenciamento**: o tratamento das credenciais do cliente (número de afiliação e chave de acesso) trafega na mensagem, em campos comuns do XML, sem necessidade de atributos especiais, como por exemplo, SOAP Header.
* **Segurança**: a troca de informações se dá sempre entre o Servidor da Loja e da Cielo, ou seja, sem o browser do comprador.
* **Multiplataforma**: a integração é realizada através de Web Service, em um único Endpoint.

## Considerações sobre a integração

* Todas as requisições a Web Service da Cielo devem conter o nó de autenticação do lojista, composto pelo número de credenciamento e chave de acesso.
* O cadastro da loja deve estar ativo junto à Cielo.
* Deve-se definir um timeout adequado nas requisições HTTP à Cielo; recomendamos 30 segundos.
* O certificado Root da entidade certificadora (CA) de nosso Web Service deve estar cadastrado na Truststore a ser utilizada. Como nossa certificadora é de ampla aceitação no mercado, é provável que ela já esteja registrada na Truststore do próprio sistema operacional.
* Disponibilizamos no kit de integração o arquivo ecommerce.xsd para facilitar a validação das restrições de formato, tamanho dos campos, tipos e domínios de dados.
* Em todas as mensagens a data/hora deverá seguir o formato: `aaaa-MM-ddTHH24:mm:ss`. Exemplo: 2011-12-21T11:32:45.
* Os valores monetários são sempre tratados como valores inteiros, sem representação das casas decimais, sendo que os dois últimos dígitos são considerados como os centavos. Exemplo: R$ 1.286,87 é representado como 128687; R$ 1,00 é representado como 100.

<aside class="notice">Veja a seção Certificado Digital para informações sobre os certificados Cielo</aside>

## Arquitetura

A integração é realizada através de serviços disponibilizados como Web Services. O modelo empregado é bastante simples: há uma única URL (endpoint) que recebe os POSTs via HTTPs e, dependendo do formato do XML enviado, uma determinada operação é realizada.

A chamada ao Web Service é resumida por:

* A mensagem em formato XML, definida de acordo com a funcionalidade.
* O destino (ambiente de teste ou de produção).
* O retorno em formato XML, que pode ser: `<transacao/>`, `<retorno-token>` ou `<erro/>`.

```
POST /servicos/ecommwsec.do HTTP/1.1
Host: ecommerce.cielo.com.br
Content-Type: application/x-www-form-urlencoded
Content-Length: length
mensagem=<?xml version="1.0" encoding="ISO-8859-1"?><requisicao-captura id="3e22bdd0-2017-4756-80b7-35a532e6c973" versao="1.2.1"><tid>10069930690101012005</tid><dados-ec><numero>1006993069</numero><chave>25fbb99741c739dd84d7b06ec78c9bac718838630f30b112d033ce2e621b34f3</chave></dados-ec><valor>3880</valor></requisicao-captura>
```

## Transação

O elemento central do Cielo E-commerce é a transação, criada a partir de uma requisição HTTP ao Webservice da Cielo. A identificação única de uma transação na Cielo é feita através do campo TID, que está presente no retorno das mensagens de autorização. Esse campo é essencial para realizar consultas, capturas e cancelamentos.

A partir da criação de uma transação, ela pode assumir os seguintes status:

![status transações](/images/status.png)

As transições de status podem ser realizadas através da troca de mensagens entre a loja e a Cielo, ou de forma automática, por exemplo, quando o prazo para a captura de transação autorizada expirar.

# Criando transações 

Todas as transações no Cielo E-commerce iniciam-se através de um POST (HTTPS) ao Web Service da Cielo com uma mensagem XML `<requisicao-transacao>`, cujo conjunto de TAGS determinam as configurações de uma transação.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb997438630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <dados-portador>
    <numero>4012001038443335</numero>
    <validade>201508</validade>
    <indicador>1</indicador>
    <codigo-seguranca>973</codigo-seguranca>
    <token/>
  </dados-portador>
  <dados-pedido>
    <numero>178148599</numero>
    <valor>1000</valor>
    <moeda>986</moeda>
    <data-hora>2011-12-07T11:43:37</data-hora>
    <descricao>[origem:10.50.54.156]</descricao>
    <idioma>PT</idioma>
    <soft-descriptor/>
    <taxa-embarque/>
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>A</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <url-retorno>http://localhost/lojaexemplo/retorno.jsp</url-retorno>
  <autorizar>1</autorizar>
  <capturar>false</capturar>
  <campo-livre>Informações extras</campo-livre>
  <bin>455187</bin>
  <gerar-token>false</gerar-token>
  <avs>
  <![CDATA[
    <dados-avs>
      <endereco>Rua Teste AVS</endereco>
      <complemento>Casa</complemento>
      <numero>123</numero>
      <bairro>Vila AVS</bairro>
      <cep>12345-123</cep>
    </dados-avs>
  ]]>
  </avs>
</requisicao-transacao>
```

### raiz

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|[dados-ec](#dados-ec)|n/a|Sim|n/a|Dados do estabelecimento comercial|
|[dados-portador](#dados-portador)|n/a|Sim|n/a|Dados do cartão|
|[dados-pedido](#dados-pedido)|n/a|Sim|n/a|Dados do pedido|
|[forma-pagamento](#forma-pagamento)|n/a|Sim|n/a|Forma de pagamento|
|url-retorno|Alfanumérico|Sim|1..1024|URL da página de retorno. É para essa página que a Cielo vai direcionar o browser ao fim da autenticação ou da autorização. Não é obrigatório apenas para autorização direta, porém o campo dever ser inserido como `null`.|
|capturar|Boolean|Sim|n/a|`true` ou `false`. Define se a transação será automaticamente capturada caso seja autorizada.|
|campo-livre|Alfanumérico|Opcional|0..128|Campo livre disponível para o Estabelecimento.|
|bin|Numérico|Opcional|6|Seis primeiros números do cartão.|
|gerar-token|Boolean|Opcional|n/a|`true` ou `false`. Define se a transação atual deve gerar um token associado ao cartão.|
|avs#avs|Alfanumérico|Opcional|n/a|String contendo um bloco XML, encapsulado pelo `CDATA`, contendo as informações necessárias para realizar a consulta ao serviço.|

### dados-ec

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|numero|Numérico|Sim|1..20|Número de afiliação da loja com a Cielo.|
|chave|AlfaNumérico|Sim|1..100|Chave de acesso da loja atribuída pela Cielo.|

### dados-portador

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|numero|Numérico|Sim|19|Número do cartão.|
|validade|Numérico|Sim|6|Validade do cartão no formato aaaamm. Exemplo: 201212 (dez/2012).|
|indicador|Numérico|Sim|1|Indicador sobre o envio do Código de segurança: **0** – não informado, **1** – informado, **2** – ilegível, **9** – inexistente|
|codigo-seguranca|Numérico|Condicional|3..4|Obrigatório se o indicador for **1**|
|nome-portador|Alfanumérico|Opcional|0..50|Nome como impresso no cartão|
|token|Alfanumérico|Condicional|0..100|Token que deve ser utilizado em substituição aos dados do cartão para uma autorização direta ou uma transação recorrente. Não é permitido o envio do token junto com os dados do cartão na mesma transação.|

### dados-pedido

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|numero|Alfanumérico|Sim|1..20|Número do pedido da loja. **Recomenda-se que seja um valor único por pedido.**|
|valor|Numérico|Sim|1..12|Valor a ser cobrado pelo pedido (já deve incluir valoresde frete, embrulho, custos extras, taxa de embarque, etc). Esse valor é o que será debitado do consumidor.|
|moeda|Numérico|Sim|3|Código numérico da moeda na norma ISO 4217. **Para o Real, o código é 986**.|
|data-hora|Alfanumérico|Sim|19|Data hora do pedido. **Formato**: `aaaa-MM-ddTHH24:mm:ss`|
|descricao|Alfanumérico|Opcional|0..1024|Descrição do pedido|
|idioma|Alfanumérico|Opcional|2|Idioma do pedido: PT (português), EN (inglês) ou ES (espanhol). Com base nessa informação é definida a língua a ser utilizada nas telas da Cielo. **Caso não seja enviado, o sistema assumirá “PT”**.|
|taxa-embarque|Numérico|Opcional|1..9|Montante do valor da autorização que deve ser destinado à taxa de embarque.|
|soft-descriptor|Alfanumérico|Opcional|0..13|Texto de até 13 caracteres que será exibido na fatura do portador, após o nome do Estabelecimento Comercial.|

<aside class="notice">O cadastro do cliente está habilitado para transacionar apenas com a moeda REAL, caso necessite de mais informações, contate a central de relacionamento, seu gerente comercial ou o Suporte Web Cielo E-commerce.</aside>

### forma-pagamento

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|bandeira|Alfanumérico|Sim|n/a|Nome da bandeira (minúsculo): “visa”, “mastercard”, “diners”, “discover”, “elo”, “amex”, “jcb”, “aura”|
|produto|Alfanumérico|Sim|1|Código do produto: **1** – Crédito à Vista, **2** – Parcelado loja, **A** – Débito.|
|parcelas|Numérico|Sim|1..2|Número de parcelas. **Para crédito à vista ou débito, utilizar 1.**|

<aside class="warning">O valor do resultante da divisão do valor do pedido pelo número de parcelas não deve ser
inferior a R$5,00. Caso contrário a transação será negada.</aside>

## Fluxos de integração e redirecionamentos

Após a transação ter sido criada, o fluxo de navegação pode ser direcionado ao ambiente da Cielo caso o lojista solicite a autenticação na mensagem XML.

Nessa situação, o sistema do lojista deve obter o valor da TAG <url-autenticacao> do XML de retorno para realizar um redirect no browser do cliente e dar continuidade ao processo. O redirecionamento deve ser realizado em modo Full Screen. Ou seja, não há mais suporte a abertura de pop up. Dessa forma, a partir da tela de checkout deve ser realizado um redirecionamento à URL retornada na criação da transação.

<aside class="notice">Esse redirecionamento pode ser através de um Http Redirect (como no código da Loja Exemplo) ou através de um Javascript.</aside>

Após o processo de autenticação, o fluxo é devolvido ao lojista através da informação presente na TAG <url-retorno>, enviada na primeira requisição para a Cielo.

O diagrama abaixo facilita a visualização do fluxo completo de navegação:

![fluxo](/images/fluxo.png)

<aside class="notice">Geralmente, a URL de retorno segue o seguinte formato: https://minhaloja.com.br/pedido?id=12345678. Essa página deve utilizar o número do pedido para buscar internamente o TID que foi retornado pela Cielo. Com esta informação, a página deve realizar uma requisição de Consulta via TID ao Web Service da Cielo e interpretar o resultado para exibir ao cliente</aside>

Por outro lado, quando não há autenticação, não existe troca de contextos ou redirects, e a integração é mais simples:

![fluxo-simples](/images/fluxo-simples.png)

## Tipos de retorno

Há três tipos de retorno que podem ser gerados na resposta do Web Service:

1. `<transacao>`
2. `<retorno-token>`
3. `<erro>`

Para as operações relacionadas a uma transação (consultas, autorização, captura e cancelamento), a resposta, em caso de sucesso, é sempre um XML do tipo `<transacao>`. No caso de uma requisição exclusiva para criação de token, a resposta esperada é `<retorno-token>`.

O exemplo ao lado ilustra a forma mais reduzida de uma mensagem de retorno tipo `<transacao>`. Basicamente, ela é composta pelos dados do pedido e dados da configuração da transação.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="6-e7762cbf8856">
  <tid>10017348980735271001</tid>
  <dados-pedido>
    <numero>1130006436</numero>
    <valor>1000</valor>
    <moeda>986</moeda>
    <data-hora>2011-12-05T16:01:28.655-02:00</data-hora>
    <descricao>[origem:10.50.54.156]</descricao>
    <idioma>PT</idioma>
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>1</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <status>0</status>
  <url-autenticacao>https://ecommerce.cielo.com.br/web/index.cbmp?id=a783251</url-autenticacao>
</transacao>
```

As informações mais importantes são:

* **TID**: é o elo entre o pedido de compra da loja virtual e a transação na Cielo.
* **URL de autenticação**: aponta à página que dá início à autenticação (quando solicitada).
* **Status**: é a informação base para a loja controlar a transação.

A tabela abaixo detalha as TAGS do XML básico de retorno, identificado pelo nó raiz `<transação>`:

|Elemento|Tipo|Tamanho|Descrição|
|--------|----|-------|---------|
|tid|Alfanumérico|1..40|Identificador da transação|
|dados-pedido|Idêntico ao nó enviado pela loja na criação da transação.|
|forma-pagamento|Idêntico ao nó enviado pela loja na criação da transação.|
|status|Numérico|12|Código de status da transação. Veja o apêndice para a lista de status|
|url-autenticacao|Alfanumérico|1..256|URL de redirecionamento à Cielo.|

Por fim, há outro tipo de retorno que é empregado toda vez que uma requisição não pode ser executada, seja porque era inválida ou por ter ocorrido falha no seu processamento. Nesse cenário o nó raiz do XML de resposta é do tipo `<erro>`.

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<erro xmlns="http://ecommerce.cbmp.com.br">
  <codigo>001</codigo>
  <mensagem><![CDATA[O XML informado nao e valido:- string value '' does not match pattern for type of valor element in DadosPedido in namespace http://ecommerce.cbmp. com.br: '<xml-fragment/>]]>
  </mensagem>
</erro>
```

Quando a transação é inválida, podemos classificar os erros em dois tipos:

* **Erros sintáticos**: ocorrem quando a mensagem XML não respeita as regras definidas no arquivo ecommerce.xsd. Por exemplo, uma letra em um campo numérico, ou a ausência de um valor obrigatório;
* **Erros semânticos**: ocorrem quando uma requisição solicita uma operação não suportada para determinada transação. Por exemplo, tentar capturar uma transação não autorizada, ou ainda, cancelar uma transação já cancelada.

<aside class="notice">As mensagens de erro sempre trazem informações adicionais que facilitam o troubleshooting. A tabela que consta no item “Anexos - 6.2. Catálogo de Erros” possui a lista completa com os códigos de erros e suas descrições que devem ser consideradas no desenvolvimento da integração.</aside>

## Autenticação e nível de segurança

Dependendo da bandeira escolhida, as transações na plataforma Cielo E-commerce podem ser configuradas para serem autenticadas no banco emissor do cartão (portador), a fim de garantir o nível maior de segurança ao lojista. A autenticação não é feita automaticamente entre sistemas, deste modo é necessário que o comprador interaja no processo, conforme será visto a seguir.

Ela acontece sempre no site do banco (Internet Banking), utilizando mecanismos e tecnologias independentes da Cielo. Dessa forma, é possível que o banco utilize token eletrônico e senha, enquanto outro utilize os cartões de senhas ou CPF para autenticar uma transação.

Conforme mostrado anteriormente, a mecânica do redirecionamento é obtida através da tag `<url-autenticacao>` que é retornada pela Cielo no XML <transação> no momento da solicitação de autorização ao Web Service.

A autenticação é obrigatória para transações de débito e opcional para o crédito. Atualmente somente Visa e MasterCard suportam essa funcionalidade e consequentemente, somente essas duas bandeiras possuem o produto débito.

<aside class="notice">Consulte os produtos e bandeiras suportadas no item 1.6 Produtos e Bandeiras suportadas.</aside>

Quando há autenticação, o fluxo de execução da autorização acaba sendo feito em duas etapas, conforme mostrado no diagrama abaixo:

![fluxo-autenticacao](/images/fluxo-autenticacao.png]

1. fecharPedido() – acontece quando o portador do cartão finaliza o pedido e dá início ao pagamento da compra
  1. criarTransacao(autenticada) – o sistema do lojista envia uma requisição XML `<requisicao-transacao>` solicitando uma transação autenticada, ou seja, a TAG <autorizar> será 0, 1 ou 2. Em seguida, a Cielo informará no XML de retorno o campo <url-autenticacao> com o endereço que o portador deverá ser redirecionado.
2. acessar(url-atenticacao) – o browser do portador é redirecionado ao ambiente da Cielo. Assim que a página da Cielo é acessada, automaticamente ela já é direcionada para o banco emissor (3.1). Esse redirect é tão rápido que é praticamente imperceptível.
3. autenticar(token, cpf) – o portador estará no ambiente do banco e utilizará algum mecanismo provido pelo próprio emissor para realizar a autenticação da transação (geralmente token, cartão de bingo, cpf, assinatura eletrônica, etc).
  1. resultadoAutenticacao() – o banco emissor redireciona o fluxo para a Cielo com o resultado da autenticação. A partir daí, o fluxo volta ao normal, conforme disposto no item “2.3 Arquitetura de integração”.
    1.processar() – o sistema da Cielo processa o retorno da autenticação e submete á autorização e, opcionalmente, à captura automática.
    2. enviarRedirect(url-retorno) – o sistema da Cielo envia um redirect ao browser do cliente para o endereço especificado na URL de retorno, fornecida na primeira requisição (`<requisicao-transacao>`)
      1. acessar(url-retorno) – o browser do portador acessar a URL no ambiente da loja, onde recomendamos que exista uma requisição de consulta via TID ao Web Service da Cielo.

### Observações

* Somente o primeiro redirecionamento (1.2: enviarRedirect()) é de responsabilidade da loja virtual.
* O comprador é redirecionado ao site do Banco Emissor somente se a autenticação estiver disponível. Caso contrário, a transação prosseguirá à autorização automaticamente (exceto se foi apenas solicitada autenticação).

<aside class="notice">Consulte os produtos e bandeiras suportadas no item 1.6 Produtos e Bandeiras suportadas.</aside>

Os pré-requisitos para que uma transação seja autenticada estão relacionados abaixo:

* Banco e Bandeira devem ser participantes do programa de autenticação;
* O BIN do cartão deve ser participante do programa de autenticação;
* A configuração da <requisicao-transacao>//<autorizar> deve ser 0, 1 ou 2. 


Quando há autenticação, o fluxo de execução da autorização acaba sendo feito em duas etapas, conforme mostrado no diagrama abaixo:

![fluxo-autorizacao](/images/fluxo-autorizacao.png)

1. fecharPedido() – acontece quando o portador do cartão finaliza o pedido e dá início ao pagamento da compra
  1. criarTransacao(autenticada) – o sistema do lojista envia uma requisição XML <requisicao-transacao> solicitando uma transação autenticada, ou seja, a TAG <autorizar> será 0, 1 ou 2. Em seguida, a Cielo informará no XML de retorno o campo <url-autenticacao> com o endereço que o portador deverá ser redirecionado.
  2. acessar(url-atenticacao) – o browser do portador é redirecionado ao ambiente da Cielo. Assim que a página da Cielo é acessada, automaticamente ela já é direcionada para o banco emissor (3.1). Esse redirect é tão rápido que é praticamente imperceptível.
  3. autenticar(token, cpf) – o portador estará no ambiente do banco e utilizará algum mecanismo provido pelo próprio emissor para realizar a autenticação da transação (geralmente token, cartão de bingo, cpf, assinatura eletrônica, etc).
    1. resultadoAutenticacao() – o banco emissor redireciona o fluxo para a Cielo com o resultado da autenticação. A partir daí, o fluxo volta ao normal, conforme disposto no item “2.3 Arquitetura de integração”.
      1. processar() – o sistema da Cielo processa o retorno da autenticação e submete á autorização e, opcionalmente, à captura automática.
      2. enviarRedirect(url-retorno) – o sistema da Cielo envia um redirect ao browser do cliente para o endereço especificado na URL de retorno, fornecida na primeira requisição (`<requisicao-transacao>`)
    2. acessar(url-retorno) – o browser do portador acessar a URL no ambiente da loja, onde recomendamos que exista uma requisição de consulta via TID ao Web Service da Cielo.

### Observações:

* Somente o primeiro redirecionamento (1.2: enviarRedirect()) é de responsabilidade da loja virtual.
* O comprador é redirecionado ao site do Banco Emissor somente se a autenticação estiver disponível. Caso contrário, a transação prosseguirá à autorização automaticamente (exceto se foi apenas solicitada autenticação).

Observando o diagrama do item “2.4 Transação”, é possível observar que todas as transações
passarão pelo status “Autenticada” ou “Não autenticada”. Por consequência, todas receberão o nó
<autenticacao> no XML de resposta ao lojista. Abaixo, o XML com o nó de autenticação:

Observando o diagrama da seção [Transação](#transação), é possível observar que todas as transações passarão pelo status “Autenticada” ou “Não autenticada”. Por consequência, todas receberão o nó `<autenticacao>` no XML de resposta ao lojista. Abaixo, o XML com o nó de autenticação:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="add7d51e-f7a1-41b1-b224-4ffbd724730c">
  <tid>1001734898073E931001</tid>
  <pan>IqVz7P9zaIgTYdU41HaW/OB/d7Idwttqwb2vaTt8MT0=</pan>
  <dados-pedido>
    <numero>1196683550</numero>
    <valor>1000</valor>
    <moeda>986</moeda>
    <data-hora>2011-12-08T10:44:24.244-02:00</data-hora>
    <descricao>[origem:10.50.54.156]</descricao>
    <idioma>PT</idioma>
    <taxa-embarque>1000</taxa-embarque>
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>1</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <status>2</status>
  <autenticacao>
    <codigo>2</codigo>
    <mensagem>Autenticada com sucesso</mensagem>
    <data-hora>2011-12-08T10:44:47.311-02:00</data-hora>
    <valor>1000</valor>
    <eci>5</eci>
  </autenticacao>
</transacao>
```

Os campos apenas do nó `<autenticacao>` estão listados na tabela abaixo:

|Elemento|Tipo|Tamanho|Descrição|
|--------|----|-------|---------|
|codigo|Numérico|1.2|Código do processamento|
|mensagem|Alfanumérico|1..100|Detalhe do processamento|
|data-hora|Alfanumérico|19|Data e hora do processamento|
|valor|Numérico|1..12|Valor do processamento sem pontuação. Os dois últimos dígitos são os centavos.|
|eci|Numérico|2|Nível de segurança.|

O campo ECI (Eletronic Commerce Indicator) representa o quão segura é uma transação. Esse valor deve ser levado em consideração pelo lojista para decidir sobre a captura da transação.

<aside class="warning">O indicador ECI é muito importante, pois é ele que determina as regras de Chargeback.</aside>

# Operações e configurações

## Criação da Transação de Autorização

A requisição de autorização é a principal operação do Cielo E-commerce, pois é através dela que uma venda pode ser concretizada e finalizar o processo de venda. A autorização possui uma série de configurações que podem ser customizadas, além de funcionalidades que agregam valor ao lojista e seus consumidores.

<aside class="notice">Para os códigos de resposta da autorização consulte o Catálogo de Códigos de Resposta da Autorização (LR)</aside>

## Autorização Direta

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb997438630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <dados-portador>
    <numero>4012001038443335</numero>
    <validade>201508</validade>
    <indicador>1</indicador>
    <codigo-seguranca>973</codigo-seguranca>
  </dados-portador>
  <dados-pedido>
    <!-- ... -->
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>1</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <url-retorno>http://localhost/lojaexemplo/retorno.jsp</url-retorno>
  <autorizar>3</autorizar>
  <capturar>false</capturar>
</requisicao-transacao>
```

A autorização direta caracteriza-se por ser uma transação onde não há a autenticação do portador, seja por opção (e risco) do lojista, seja porque a bandeira ou emissor não tem suporte. A autorização direta pode ser feita de duas formas: tradicional (com os dados do cartão) ou através de um token.

### Tradicional

* **Objetivo** - Submeter uma transação direta com o uso de um cartão de crédito.
* **Regras**
  * O cadastro da loja virtual deve estar habilitado para envio dos dados do cartão.
  * Enviar a TAG `<autorizar>` com o valor “3”.
  * Somente válido para Crédito.
  * O lojista deve estar atento às regras para envio do cartão.
  * Na autorização direta, o nível de segurança da transação (ECI) é definido como:
    * “7” para Visa, Diners, Discover, Elo e JCB.
    * “0” para Mastercard e Aura

## Autorização Recorrente

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb997438630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <dados-portador>
    <numero>4012001038443335</numero>
    <validade>201508</validade>
    <indicador>1</indicador>
    <codigo-seguranca>973</codigo-seguranca>
  </dados-portador>
  <dados-pedido>
    <!-- ... -->
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>1</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <url-retorno>http://localhost/lojaexemplo/retorno.jsp</url-retorno>
  <autorizar>4</autorizar>
  <capturar>false</capturar>
</requisicao-transacao>
```

A autorização recorrente pode ser feita de duas formas: através do envio de um token previamente cadastrado, ou através de um cartão. A transação recorrente é praticamente igual à transação tradicional, as mudanças consistem nas regras que o emissor e a bandeira utilizam para autorizar ou negar uma transação. Outra diferença está relacionada ao Renova Fácil.

<aside class="notice">Para saber se sua loja é elegível a utilizar a autorização recorrente, consulte nossa central de relacionamento ou o Suporte Web Cielo E-commerce.</aside>

### Autorização recorrente com Cartão

* **Objetivo** - Submeter uma transação recorrente com o uso de um cartão de crédito.
* **Regras**
  * Enviar a TAG `<autorizar>` com o valor “4”.
  * Somente válido para crédito à vista.

### Renova Fácil

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="d35b67189442">
  <tid>10069930690362461001</tid>
  <pan>uv9yI5tkhX9jpuCt+dfrtoSVM4U3gIjvrcwMBfZcadE=</pan>
  <dados-pedido>
    <!-- ... -->
  </dados-pedido>
  <forma-pagamento>
    <!-- ... -->
  </forma-pagamento>
  <status>5</status>
  <!-- ... -->
  <autorizacao>
    <codigo>5</codigo>
    <mensagem>Autorização negada</mensagem>
    <data-hora>2011-12-09T10:58:45.847-02:00</data-hora>
    <valor>1000</valor>
    <lr>57</lr>
    <nsu>221766</nsu>
  </autorizacao>
  <dados-portador>
    <numero>4012001038443335</numero>
    <validade>201508</validade>
    <codigo-seguranca/>
  </dados-portador>
</transacao>
```

Essa funcionalidade facilita a identificação de um cartão que tenha sido substituído por outro no banco emissor. Dessa forma, quando uma transação recorrente é submetida ao Web Service e a Cielo identifica que o cartão utilizado está desatualizado, o sistema terá o seguinte comportamento:

1. Caso a transação recorrente tenha sido enviada através de um cartão, sua autorização será negada e serão retornados os dados do novo cartão, conforme o diagrama abaixo:

![remova fácil](/images/remova-facil.png)

<aside class="notice">O Renova Fácil só está disponível para transações recorrentes. A efetividade do Renova Fácil depende do uso correto das transações recorrentes devidamente sinalizadas como tal. Consulte os bancos e bandeiras participantes com o Suporte Web Cielo E-commerce.</aside>

### Autorização de uma transação previamente gerada

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-autorizacao-tid id="0000000000" versao="1.2.0">
  <tid>1006993069990BCAA001</tid>
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb997438630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
</requisicao-autorizacao-tid>
```

Para os estabelecimentos utilizando o processo de autenticação, é possível solicitar a autorização
das transações que pararam após a execução deste processo. A mensagem para performar tal operação
é a “requisicao-autorizacao-tid” como descrita abaixo:

<aside class="notice">Requisições para transações que não foram submetidas ao processo de autenticação ou foram interrompidas, pois o portador errou a senha não serão aceitas.</aside>

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|tid|Alfanumérico|Sim|20|Identificador da transação|
|dados-ec.numero|Numérico|Sim|1..20|Número de afiliação da loja com a Cielo.|
|dados-ec.chave|Alfanumérico|Sim|1..100|Chave de acesso da loja atribuída pela Cielo.|

## Transação com Token

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-token id="8fc889c7-004f-42f1-963a-31aa26f75e5c" versao="1.2.1">
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb99741c739dd84d7b06ec78c9bac718838630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <dados-portador>
    <numero>4012001038443335</numero>
    <validade>201508</validade>
    <nome-portador>FULANO DA SILVA</nome-portador>
  </dados-portador>
</requisicao-token>
```

* **Objetivo** - Solicitar a criação de um token associada a um cartão de crédito, para viabilizar o envio de transações sem o cartão.
* **Regras**
  * O Token é único para um determinado [Cartão + Estabelecimento Comercial]. Dessa forma, um cartão pode estar “tokenizado” em mais de uma loja e em cada uma possuirá códigos diferentes.
  * Caso seja enviada mais de uma solicitação com os mesmos dados, o token retornado será sempre o mesmo.
  * A criação do token é independente do resultado da autorização (aprovada/negada).

<aside class="warning">A transação feita via token não isenta o lojista do envio da informação de bandeira, portanto é necessário que o sistema do lojista (ou gateway) que armazenará os tokens também armazene a bandeira do cartão que foi tokenizado.</aside>

<aside class="notice">Um token não utilizado por mais de um ano será automaticamente removido do banco de dados da Cielo. É possível realizar o bloqueio de um token específico para que este não seja mais usado. O bloqueio do token deve ser solicitado ao Suporte Web Cielo E-commerce.</aside>

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|dados-ec.numero|Numérico|Sim|1..20|Número de afiliação da loja com a Cielo.|
|dados-ec.chave|Alfanumérico|Sim|1..100|Chave de acesso da loja atribuída pela Cielo.|
|daods-portador|n/a|Opcional|n/a|Nó com os dados do cartão.|
|dados-portador.numero|Numérico|Sim|19|Número do cartão|
|dados-portador.validade|Numérico|Sim|6|Validade do cartão no formato aaaamm. Exemplo: 201212 (dez/2012).|
|dados-portador.nome-portador|Alfanumérico|Opcional|0..50|Nome impresso no cartão|

### Retorno

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<retorno-token xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="57239017">
  <token>
    <dados-token>
      <codigo-token>TuS6LeBHWjqFFtE7S3zR052Jl/KUlD+tYJFpAdlA87E=</codigo-token>
      <status>1</status>
      <numero-cartao-truncado>455187******0183</numero-cartao-truncado>
    </dados-token>
  </token>
</retorno-token>
```

O retorno será do tipo <retorno-token> quando a solicitação tenha sido concluída com sucesso, ou <erro> em caso de fracasso.

|Elemento|Tipo|Tamanho|Descrição|
|--------|----|-------|---------|
|codigo-token|Alfanumérico|100|Código do token gerado|
|status|Numérico|1|Status do Token: **0** – Bloqueado, **1** – Desbloqueado|
|numero-cartao-truncado|Alfanumérico|1..16|Número do cartão truncado.|

### Autorização Direta via Token

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb997438630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <dados-portador>
    <token>TuS6LeBHWjqFFtE7S3zR052Jl/KUlD+tYJFpAdlA87E=</token>
  </dados-portador>
  <dados-pedido>
    <!-- ... -->
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>1</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <url-retorno>http://localhost/lojaexemplo/retorno.jsp</url-retorno>
  <autorizar>3</autorizar>
  <capturar>true</capturar>
</requisicao-transacao>
```

* **Objetico** - Submeter uma transação direta (sem autenticação) com o uso de um token previamente cadastrado
* **Regras**
  * Enviar a TAG `<autorizar>` com o valor “3”.
  * O token deve estar desbloqueado.
  * Válido somente para crédito.

### Autorização recorrente com Token

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb997438630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <dados-portador>
    <token>TuS6LeBHWjqFFtE7S3zR052Jl/KUlD+tYJFpAdlA87E=</token>
  </dados-portador>
  <dados-pedido>
    <!-- ... -->
  </dados-pedido>
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>1</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <url-retorno>http://localhost/lojaexemplo/retorno.jsp</url-retorno>
  <autorizar>4</autorizar>
  <capturar>true</capturar>
</requisicao-transacao>

```

* **Objetivo** - Submeter uma transação recorrente com o uso de um token previamente cadastrado.
* **Regras**
  * Enviar a TAG `<autorizar>` com o valor “4”.
  * O token deve estar desbloqueado.
  * Somente válido para crédito à vista.

### Renova Fácil com Token

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="d35b67189442">
  <tid>10069930690362461001</tid>
  <pan>uv9yI5tkhX9jpuCt+dfrtoSVM4U3gIjvrcwMBfZcadE=</pan>
  <dados-pedido>
    <!-- ... -->
  </dados-pedido>
  <forma-pagamento>
    <!-- ... -->
  </forma-pagamento>
  <status>5</status>
  <!-- ... -->
  <autorizacao>
    <codigo>5</codigo>
    <mensagem>Autorização negada</mensagem>
    <data-hora>2011-12-09T10:58:45.847-02:00</data-hora>
    <valor>1000</valor>
    <lr>57</lr>
    <nsu>221766</nsu>
  </autorizacao>
  <token>
    <dados-token>
      <codigo-token>TuS6LeBHWjqFFtE7S3zR052Jl/KUlD+tYJFpAdlA87E=</codigo-token>
      <status>1</status>
      <numero-cartao-truncado>455187******0183</numero-cartao-truncado>
    </dados-token>
  </token>
</transacao>
```

Essa funcionalidade facilita a identificação de um cartão que tenha sido substituído por outro no banco emissor. Dessa forma, quando uma transação recorrente é submetida ao Web Service e a Cielo identifica que o cartão utilizado está desatualizado, o sistema terá o seguinte comportamento:

1. Caso a transação recorrente tenha sido enviada através de um token, sua autorização será negada e será retornado um novo token para ser utilizado, conforme o diagrama abaixo:

![remova fácil com token](/images/remova-facil-token.png)

### Geração de Token

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.0">
  <!-- ... -->
  <gerar-token>true</gerar-token>
</requisicao-transacao>
```

<aside class="warning">este serviço adicional está sujeito a cobrança a partir do momento em que a geração de token é solicitada.</aside>

* **Objetivo** - Além da mensagem específica para criação de um token, descrita em Transação com Token, é possível aproveitar uma requisição de autorização para solicitar a geração do token, acrescentando a informação `<gerar-token>` no nó de requisição de transação.
* **Regras**
  * Caso um cartão seja submetido mais de uma vez pelo mesmo lojista, o Token gerado será sempre o mesmo.

## Funcionalidades Agregadas

### Autenticação e Transações com Cartões de Débito

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <!-- ... -->
  <forma-pagamento>
    <bandeira>visa</bandeira>
    <produto>A</produto>
    <parcelas>1</parcelas>
  </forma-pagamento>
  <!-- ... -->
  <autorizar>1</autorizar>
</requisicao-transacao>
```

A autenticação da transação garantirá uma segurança extra ao lojista contra Chargebacks, porém, conforme apresentando no capítulo “2.5 – Autenticação e nível de segurança”, nem todas as bandeiras e emissores disponibilizam esse tipo de serviço.

O produto débito obrigatoriamente exige uma transação autenticada, caso contrário, a transação não é autorizada.

* **Objetivo** - Tornar elegível uma transação para autenticação.
* **Regras**
  * Enviar a flag <autorizar> de acordo com o domínio abaixo, para tentar:
    * 0 – Somente autenticar a transação.
    * 1 – Submeter à autorização somente se a transação for autenticada.
    * 2 – Submeter à autorização se a transação for autenticada ou não.
  * Para débito, enviar o produto “A” no XML.
  * A solicitação da autorização de uma transação que foi somente autenticada pode ser feita em até 90 dias após a data inicial.

<aside class="notice">Tendo em vista que a autenticação não depende exclusivamente desta flag, recomendamos sempre verificar o campo <eci> para verificar o resultado da autenticação.</aside>

### Soft Descriptor

```xml
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <!-- ... -->
  <dados-pedido>
    <numero>178148599</numero>
    <valor>1000</valor>
    <moeda>986</moeda>
    <data-hora>2011-12-07T11:43:37</data-hora>
    <descricao>[origem:10.50.54.156]</descricao>
    <idioma>PT</idioma>
    <soft-descriptor>soft-descriptor</soft-descriptor>
  </dados-pedido>
  <!-- ... -->
</requisicao-transacao>
```

* **Objetivo** - Permite que o lojista envie um texto de até 13 caracteres que será impresso na fatura do portador, ao lado da identificação da loja, respeitando o comprimento das bandeiras:
  * **Visa**: 25 caracteres
  * **Mastercard**: 22 caracteres
* **Regras**
  * Tamanho máximo: 13 caracteres.
  * Disponível apenas para as bandeiras Visa e MasterCard.
  * Uso exclusivo de caracteres simples.

<aside class="notice">Para conhecer e/ou alterar o nome da loja que será impresso na fatura do portador entre em contato com nossa central de relacionamento</aside>

### Captura Automática

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <!-- ... -->
  <capturar>true</capturar>
  <!-- ... -->
</requisicao-transacao>
```

* **Objetivo** - A captura automática permite que uma requisição de autorização seja capturada imediatamente após sua aprovação. Dessa forma, não é preciso realizar uma `<requisicao-captura>`.
* **Regras**
  * Somente autorizações aprovadas serão capturadas automaticamente.

### Taxa de embarque

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <!-- ... -->
  <dados-pedido>
    <numero>178148599</numero>
    <valor>10000</valor>
    <moeda>986</moeda>
    <data-hora>2011-12-07T11:43:37</data-hora>
    <descricao>[origem:10.50.54.156]</descricao>
    <idioma>PT</idioma>
    <soft-descriptor>softdescriptor</soft-descriptor>
    <taxa-embarque>1000</taxa-embarque>
  </dados-pedido>
  <!-- ... -->
</requisicao-transacao>
```

* **Objetivo** A taxa de embarque é um campo informativo que define o montante do total da transação (informado na tag dados-pedido//valor) que deve ser destinado ao pagamento da taxa à Infraero.
  * O valor da taxa de embarque não é acumulado ao valor da autorização. Por exemplo, em uma venda de passagem aérea de R$ 200,00 com taxa de embarque de R$ 25,00 deve-se enviar o campo `<valor>22500</valor>` e `<taxa-embarque>2500</taxa-embarque>`.
* **Regras**
  * Disponível apenas para as bandeiras Visa e Mastercard.
  * O valor da taxa de embarque não é somado ao valor da autorização, ou seja, é apenas informativo.

<aside class="notice">Existem regras específicas para a requisição de captura com taxa de embarque, disponíveis no item Captura Total e Parcial.</aside>

### AVS (Address Verification Service)

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-transacao id="a97ab62a-7956-41ea-b03f-c2e9f612c293" versao="1.2.1">
  <!-- ... -->
  <dados-pedido>
    <numero>178148599</numero>
    <valor>10000</valor>
    <moeda>986</moeda>
    <data-hora>2011-12-07T11:43:37</data-hora>
    <descricao>[origem:10.50.54.156]</descricao>
    <idioma>PT</idioma>
    <soft-descriptor>softdescriptor</soft-descriptor>
    <taxa-embarque>1000</taxa-embarque>
    <avs>
	<![CDATA[
      <dados-avs>
        <endereco>Rua Teste AVS</endereco>
        <complemento>Casa</complemento>
        <numero>123</numero>
        <bairro>Vila AVS</bairro>
        <cep>12345-123</cep>
      </dados-avs>
	]]>
    </avs>
  </dados-pedido>
  <!-- ... -->
</requisicao-transacao>
```

* **Objetivo** - O AVS é um serviço para transações de cartão não presente onde é realizada uma validação cadastral através do batimento dos dados numéricos do endereço informado pelo portador (endereço de entrega da fatura) na loja virtual, com os dados cadastrais do emissor.
  * O AVS é um serviço que auxilia na redução do risco de não reconhecimento de compras online. É uma ferramenta que o estabelecimento utilizará para a análise de suas vendas, antes de decidir pela captura da transação e a entrega do produto ou serviço.
* **Regras**
  * Disponível apenas para as bandeiras Visa, Mastercard e AmEx.
  * Produtos permitidos: somente crédito.
  * O retorno da consulta ao AVS é separado em dois itens: CEP e endereço.
  * Cada um deles pode ter os seguintes valores:
    * C – Confere;
    * N – Não confere;
    * I – Indisponível;
    * T – Temporariamente indisponível;
    * X – Serviço não suportado para esta Bandeira.
  * O nó contendo o XML do AVS deve estar encapsulado pelo termo “CDATA”, para evitar problemas com o parser da requisição.
  * É necessário que todos os campos contidos no nó AVS sejam preenchidos.
  * Necessário habilitar a opção do AVS no cadastro. Para habilitar a opção AVS no cadastro ou consultar os bancos participantes, entre em contato com o Suporte Web Cielo E-commerce.

<aside class="warning">Conforme contrato, este serviço adicional está sujeito a cobrança a partir do momento em que a consulta de AVS for solicitada. Para maiores informações, favor entrar em contato com a central de atendimento, seu gerente de contas ou o Suporte Web Cielo E-commerce.</aside>

## Captura

Uma transação autorizada somente gera o crédito para o estabelecimento comercial caso ela seja capturada. Por isso, **toda venda que o lojista queira efetivar será preciso realizar a captura (ou confirmação) da transação**.

Para vendas na modalidade de Crédito, essa confirmação pode ocorrer em dois momentos:

* Imediatamente após a autorização (captura total);
* Posterior à autorização (captura total ou parcial).

No primeiro caso, não é necessário enviar uma requisição de captura, pois ela é feita automaticamente pela Cielo após a autorização da transação. Para tanto, é preciso configurar a requisição de transação definindo-se o valor “true” para a TAG `<capturar>`, conforme visto na seção “Criando uma transação”.

Já no segundo caso, é preciso fazer uma “captura posterior”, através de uma nova requisição ao Webservice da Cielo para confirmar a transação e receber o valor da venda.

<aside class="warning">O prazo máximo para realizar a captura posterior é de 5 dias corridos após a data da autorização. Por exemplo, se uma autorização ocorreu em 10/12 às 15h20m45s, o limite para captura será às 15h20m45s do dia 15/12.</aside>

<aside class="notice">Na modalidade de débito não existe essa opção: toda transação de débito autorizada é capturada automaticamente.</aside>

### Captura Total e Parcial

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-captura id="0374f305-0e23-4aad-82a2-055788c8cf4d" versao="1.2.1">
  <tid>10069930690360EF1001</tid>
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb99741c739dd84d7b06ec78c9bac718838630f30b112d033ce2e621b34f3</chave>
  </dados-ec>
  <valor>10000</valor>
  <taxa-embarque>1000</taxa-embarque>
</requisicao-captura>

```

* **Objetivo** - Realizar a captura total e parcial de uma transação previamente autorizada.
* **Regras**
  * Disponível somente para transações dentro do prazo máximo de captura.
  * Caso o valor não seja informado, o sistema assumirá a captura do valor total.
  * O valor da captura deve ser menor ou igual ao valor da autorização.
  * Em caso de falha, novas tentativas de captura poderão ser feitas.
  * Em caso de sucesso, o status é alterado para “6 – Capturada”.
  * **Transações com Taxa de embarque:**
    * Na requisição de captura, o valor da taxa de embarque indica o montante do total que será capturado que deve ser destinado a esse fim.
    * Obrigatório caso seja captura parcial.
    * Caso a captura seja total, o sistema irá considerar o valor da taxa de embarque informado no requisição de autorização (`<requisicao-transacao>`).

### Retorno

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="0378c8cf4d">
  <tid>10069930690360EF1001</tid>
  <pan>uv9yI5tkhX9jpuCt+dfrtoSVM4U3gIjvrcwMBfZcadE=</pan>
  <!-- ... -->
  <captura>
    <codigo>6</codigo>
    <mensagem>Transacao capturada com sucesso</mensagem>
    <data-hora>2011-12-08T14:23:08.779-02:00</data-hora>
    <valor>900</valor>
    <taxa-embarque>900</taxa-embarque>
  </captura>
</transacao>
```

Os campos do nó <captura> estão detalhados a seguir:

|Elemento|Tipo|Tamanho|Descrição|
|--------|----|-------|---------|
|captura|Nó com dados da captura caso tenha passado por essa etapa.|
|captura.codigo|Numérico|1..2|Código do processamento|
|captura.mensagem|Alfanumérico|1..100|Detalhe do processamento.|
|captura.datahora|Alfanumérico|19|Data hora do processamento.|
|captura.valor|Numérico|1..12|Valor do processamento sem pontuação. Os dois últimos dígitos são os centavos.|
|captura.taxa-embarque|Numérico|1..9|Montante declarado como taxa de embarque que foi capturado.|

## Cancelamento

O cancelamento é utilizado quando o lojista decide não efetivar um pedido de compra, seja por insuficiência de estoque, por desistência da compra pelo consumidor, ou qualquer outro motivo. Seu uso faz-se necessário principalmente se a transação estiver capturada, pois haverá débito na fatura do portador, caso ela não seja cancelada.

<aside class="notice">Se a transação estiver apenas autorizada e a loja queira cancelá-la, o pedido de cancelamento não é necessário, pois após o prazo de captura expirar, ela será cancelada automaticamente pelo sistema.</aside>

### Cancelamento Total e Parcial

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-cancelamento id="13368079-dedc-4cdf-9140-84473faf83d4" versao="1.2.1">
  <tid>100699306903613D1001</tid>
  <dados-ec>
    <numero>1006993069</numero>
    <chave>25fbb99741c739dd84d7b06ec78c9bac718838630f30b112d033ce2e621b34f3
</chave>
  </dados-ec>
  <valor>200</valor>
</requisicao-cancelamento>
```

* **Objetivo** - Realizar o cancelamento do valor total ou parcial de uma transação.
* **Regras**
  * O cancelamento total é válido tanto para transações capturadas, como autorizadas; o parcial é válido apenas para as capturadas.
  * O prazo de cancelamento é de até 120 dias para a modalidade crédito e D+0 para débito.
  * O cancelamento total, quando realizado com sucesso, altera o status da transação para “9 – Cancelada”, enquanto que o parcial não altera o status da transação, mantendo-a como “6 – Capturada”.
  * Caso a TAG `<valor>` não seja fornecida, o sistema assumirá um cancelamento total.
  * Para bandeira AMEX está disponível apenas o cancelamento total.
  * Para a modalidade débito, não existe a possibilidade de efetuar cancelamento parcial.
  * **Transações com Taxa de embarque:**
    * Transações capturadas com o mesmo valor da autorização (ou seja, captura total) possuem o mesmo tratamento para cancelamentos parciais e totais, pois o valor da taxa de embarque é cancelado integralmente.

|Elemento|Tipo|Obrigatório|Tamanho|Descrição|
|--------|----|-----------|-------|---------|
|tid|Alfanumérico|Sim|1..40|Identificador da transação.|
|dados-ec.numero|Numérico|Sim|1..10|Número de credenciamento da loja com a Cielo.|
|dados-ec.chave|Alfanumérico|Sim|1..100|Chave de acesso da loja atribuída pela Cielo.|
|valor|Numérico|Opcional|1..12|Valor a ser cancelado. **Caso não seja informado, será um cancelamento total.**|

#### Retorno

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao xmlns="http://ecommerce.cbmp.com.br" versao="1.2.1" id="2c18f00a-3ff6-4c85-8865-a4fde599b2b2">
  <tid>100699306903613E1001</tid>
  <pan>uv9yI5tkhX9jpuCt+dfrtoSVM4U3gIjvrcwMBfZcadE=</pan>
  <!-- ... -->
  <cancelamentos>
    <cancelamento>
      <codigo>9</codigo>
      <mensagem>Transacao cancelada com sucesso</mensagem>
      <data-hora>2011-12-08T16:46:35.109-02:00</data-hora>
      <valor>1000</valor>
    </cancelamento>
  </cancelamentos>
</transacao>
```

<aside class="notice">Os cancelamentos (parciais ou totais) das transações com taxa de embarque e captura parcial não serão acatadas automaticamente pelo sistema.</aside>

# Testes e Homologação

## Endpoint

Os testes de integração deverão ser realizados antes do início da homologação, durante o desenvolvimento (codificação) da solução. Para isso, deve-se considerar o seguinte ambiente como EndPoint do Webservice: [https://qasecommerce.cielo.com.br/servicos/ecommwsec.do](https://qasecommerce.cielo.com.br/servicos/ecommwsec.do)

<aside class="warning">Toda a conexão aos serviços da Cielo deve ser feita através das URL’s divulgadas neste manual. A Cielo desaconselha fortemente a conexão direta via IP, uma vez que estes podem variar sem aviso prévio.</aside>

## Dados para testes

A massa de dados para realizar os testes neste ambiente está disposta na tabela abaixo:

|Bandeira|Autenticação|Número do cartão de teste|Validade|Código de segurança - CVC|
|--------|------------|-------------------------|--------|-------------------------|
|Visa|Sim|4012001037141112|05/2018|123|
|Mastercard|Sim|5453010000066167|05/2018|123|
|Visa|Não|4012001038443335|05/2018|123|
|Mastercard|Não|5453010000066167|05/2018|123|
|Amex|Não|376449047333005|05/2018|123|
|Elo|Não|6362970000457013|05/2018|123|
|Diners|Não||
|Discover|Não|6011020000245045|05/2018|123|
|JCB|Não|3566007770004971|05/2018|123|
|Aura|Não|5078601912345600019|05/2018|123|

## Chave de testes

Para facilitar o desenvolvimento disponibilizamos duas chaves para testes, uma para cada modalidade de integração. Com base nas configurações iniciais feitas durante o seu credenciamento, escolha os dados corretos para realizar os testes:

|Número estabelecimento comercial|Chave de testes|
|--------------------------------|---------------|
|1006993069|25fbb99741c739dd84d7b06ec78c9bac718838630f30b112d033ce2e621b34f3|

<aside class="warning">O valor do pedido além de seguir o formato sem pontos ou vírgulas decimais, deve terminar em “00”, caso contrário, a autorização será sempre negada. Exemplo: R$ 15,00 deve ser formatado como “1500”.</aside>

<aside class="notice">O ambiente de testes só deve ser utilizado pelos estabelecimentos de testes listados no quadro acima. O uso de dados originais do estabelecimento gerará transações não possíveis de rastreamento, gerando resultados incorretos. No ambiente de testes, use as credenciais para testes, no ambiente de produção, use os dados originais do estabelecimento.</aside>

Após a conclusão do desenvolvimento, a etapa de Homologação garantirá que a implementação foi adequada e a solução do Cliente está apta para interagir no ambiente produtivo da Cielo. Ela sempre acontece depois que o desenvolvimento foi finalizado e testado. É composta pelas seguintes etapas:

![fluxo testes](/images/fluxo-testes.png)

1. Finalização do Cadastro: nesta etapa o Cliente deve enviar um email para [cieloecommerce@cielo.com.br](mailto:cieloecommerce@cielo.com.br), solicitando a Chave de Produção. A mensagem deve conter as
seguintes informações, que irão completar o cadastro:
  * URL Definitiva do site (ambiente de produção).
  * Nome da empresa responsável pelo desenvolvimento da integração.
  * Nome e e-mail do técnico (desenvolvedor) responsável pela integração.
  * Número de credenciamento (junto à Cielo) da loja virtual.
  * Razão social e nome fantasia da loja virtual.
  * Um usuário e senha na loja virtual para efetuar compras de testes.
  * URL do logotipo da loja no formato GIF e tamanho de 112X25 pixels.

<aside class="notice">A imagem do logotipo deve estar hospedada em ambiente seguro (HTTPS), caso contrário o consumidor receberá notificações de segurança que podem culminar no abandono da compra.</aside>

Em resposta, a Cielo retornará uma chave válida no ambiente de produção. Logo, a loja está habilitada a realizar seus testes nesse ambiente. Inicia-se a segunda etapa. É importante que testes sejam realizados para cobrir os seguintes tópicos:

* Interação com o Webservice: testes com a conexão utilizada.
* Integração visual: a ida e a volta do fluxo a Cielo (fluxos alternativos devem ser
* considerados).
* Aplicação correta da marca da bandeira.
* Modalidades de pagamento: testes com as combinações possíveis de pagamento.

Neste momento, deve-se considerar o ambiente: [https://ecommerce.cielo.com.br/servicos/ecommwsec.do](https://ecommerce.cielo.com.br/servicos/ecommwsec.do)

<aside class="notice">A integração da loja virtual deverá ser feita sempre através da URL acima e não por IP.</aside>

Os testes em produção devem ser feitos com cartões de propriedade da Loja ou cujo portador tenha autorizado seu uso, uma vez que neste ambiente existe compromisso financeiro sobre as transações realizadas.

Ao término, uma nova solicitação deve ser enviada para cieloecommerce@cielo.com.br, para que a Cielo realize a homologação de fato. Um conjunto de testes será executado aprovar e negar transações. O resultado “HOMOLOGADO” é enviado por e-mail. Caso haja algum ponto que não permite a conclusão da homologação, a informação será igualmente enviada por email solicitando as correções necessárias.

# Considerações Finais

## Regras para leitura do cartão na loja

A leitura dos dados do cartão no ambiente próprio é controlada por regras definidas pelo programa de segurança imposto pelas bandeiras de cartões.

Para a Visa, esse programa é o conhecido como AIS (Account Information Security) PCI. Para maiores informações acesse: [www.cielo.com.br](www.cielo.com.br) > Serviços > Serviços de Segurança > AIS – Programa de Segurança da Informação , ou entre em contato conosco.

Para a Mastercard o programa de segurança é o SDP (Site Data Protection) PCI. Para maiores informações acesse: [http://www.mastercard.com/us/sdp/index.html](http://www.mastercard.com/us/sdp/index.html), ou entre em contato conosco.

Ademais, atendidos os requisitos, no momento do credenciamento E-commerce deve ser mencionada a escolha por leitura do cartão na própria loja.

## Certificado digital

Em alguns ambientes é preciso extrair o Certificado Digital que a aplicação do Cielo E-commerce utiliza para ser instalado na Trustedstore do cliente, especialmente em ambientes Java e PHP.

Para obter o certificado, abra um browser e acesse [http://ecommerce.cielo.com.br](http://ecommerce.cielo.com.br) e clique no ícone que exibe as informações sobre o certificado:

**Google Chrome**:

![Certificado no Google Chrome](/images/certificado-chrome.png)

**Mozilla Firefox**:

![Certificado no Mozilla Firefox](/images/certificado-firefox.png)

**Internet Explorer**:

![Certificado no Internet Explorer](/images/certificado-ie.png)

Programa **Verified by Visa** (Visa)

Programa internacional da Visa para possibilitar a autenticação do comprador no momento de uma compra em ambiente E-commerce. Visite [http://www.verifiedbyvisa.com.br/](http://www.verifiedbyvisa.com.br/) para maiores informações.

Programa **Secure Code** (Mastercard)

Programa internacional da Mastercard para possibilitar a autenticação do comprador no momento de uma compra em ambiente E-commerce. Visite [http://www.mastercard.com/securecode](http://www.mastercard.com/securecode) para maiores informações.

# Apêndice

## Status das transações

|Status|Código|
|------|------|
|Transação Criada|0|
|Transação em Andamento|1|
|Transação Autenticada|2|
|Transação não Autenticada|3|
|Transação Autorizada|4|
|Transação não Autorizada|5|
|Transação Capturada|6|
|Transação Cancelada|9|
|Transação em Autenticação|10|
|Transação em Cancelamento|12|

## ECI

|Resultado da Autenticação|Visa|Mastercard|Aura|Demais|
|Portador autenticado com sucesso.|5|2|n/d|n/d|
|Portador não fez autenticação, pois o emissor não forneceu mecanismos de autenticação.|6|1|n/d|n/d|
|Portador não se autenticou com sucesso, pois ocorreu um erro técnico inesperado.|7|1|n/d|n/d|
|Portador não se autenticou com sucesso.|70|n/d|n/d|
|A loja optou por autorizar sem passar pela autenticação.|7|0|0|7|