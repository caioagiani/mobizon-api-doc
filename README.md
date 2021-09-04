FORMAT: 1A
HOST: https://api.mobizon.com.br/service/

# Mobizon - Manual API

<img src="https://mobizon.com.br/public/documents/0011/6064238d799ebdcb970a882a62d05a80.png" style="width: 100%;"/>

Esta documentação detalha os endpoints disponíveis na API do [mobizon](https://mobizon.com.br) e disponibiliza um console interativo
para que você consiga testar as requisições sem ser necessário escrever nenhuma linha de código. Além disso, através
do console também é possível gerar automaticamente o código das requisições para diversas liguagens (PHP, Java, Ruby, entre muitas outras)

Para testar os exemplos é necessário criar sua conta e gerar sua chave de acesso. Caso você ainda não tenha, basta
[criar uma clicando aqui](https://mobizon.com.br/).

<img src="https://mobizon.com.br/public/documents/0011/8ac92d3a71051678c0a32f91676f99be.png?mobizon-api" style="width: 100%;"/>

## Visão geral
Nossa API oferece a oportunidade de configurar notificações automatizadas por SMS diretamente de qualquer software, CRM ou aplicativo.

Para a comunicação do servidor API, é utilizado um protocolo HTTP, incluindo suporte SSL seguro. As solicitações são enviadas usando os métodos GET e POST, e o servidor responde no formato XML ou JSON, dependendo da sua preferência.

```Atenção! Sua API Key possui privilégios de uso próprio, portanto certifique-se de mantê-la protegida.```

## Status

Veja a tabela de `API response codes`/`SMS status` da API:

Para mais informacoes, [veja aqui](https://github.com/mobizon/mobizon-api-docs).

### API response codes

Code                                        | Type    | Description
--------------------------------------------|---------|----------------
<span data-anchor="api-code-0">0</span>     | int | A operação foi completa com sucesso.
<span data-anchor="api-code-1">1</span>     | int | Erro de validação de dados transmitidos durante a criação ou atualização de qualquer entidade. O campo de dados fornece informações sobre os campos preenchidos incorretamente. Corrija os erros e repita a solicitação com novos dados.
<span data-anchor="api-code-2">2</span>     | int | A entrada solicitada não foi encontrada. É provável que seja excluído, o ID da entrada está incorreto ou o usuário que tenta acessar a entrada não possui os direitos de acesso apropriados para ela.
<span data-anchor="api-code-3">3</span>     | int | Ocorreu um erro de aplicativo não identificado. Entre em contato com a equipe de suporte e nos informe os detalhes da solicitação para a qual foi recebido.
<span data-anchor="api-code-4">4</span>     | int | O parâmetro "`module`" está incorreto. Verifique a documentação API.
<span data-anchor="api-code-5">5</span>     | int | O parâmetro "`method`" está incorreto. Verifique a documentação API.
<span data-anchor="api-code-6">6</span>     | int | O parâmetro "`format`" está incorreto. Verifique a documentação API.
<span data-anchor="api-code-8">8</span>     | int | Falha na autenticação. O erro ocorre se: 1. Os detalhes de login estão incorretos. 2. Quando uma sessão do usuário expirou durante a operação do sistema ou foi fechada à força pelo servidor. Para obter informações detalhadas, consulte o campo "`message`".
<span data-anchor="api-code-9">9</span>     | int | Erro de acesso ao método obrigatório.
<span data-anchor="api-code-10">10</span>   | int | Erro ao salvar dados do servidor diretamente durante esta operação. Normalmente este erro está relacionado ao acesso simultâneo aos dados de vários clientes ou alterações nas condições de salvamento dos dados.
<span data-anchor="api-code-11">11</span>   | int | Alguns dos parâmetros necessários estão faltando na requisição. Verifique a documentação API e adicione os parâmetros necessários.
<span data-anchor="api-code-12">12</span>   | int | O parâmetro de entrada da solicitação não está em conformidade com as condições ou restrições especificadas. Este código de erro ocorre quando um parâmetro viola as restrições ao executar uma solicitação com parâmetros. Parece um erro de validação de atributo, mas pode ser recebido em solicitações não destinadas à criação ou modificação de dados.
<span data-anchor="api-code-13">13</span>   | int | Uma tentativa de solicitar o servidor API que não atende a este usuário. Se você receber este código, procure o domínio correto no campo "`data`".
<span data-anchor="api-code-14">14</span>   | int | Este erro ocorre se a conta do usuário foi **bloqueada** ou **removida**.
<span data-anchor="api-code-15">15</span>   | int | Ocorreu um erro durante a execução de qualquer operação não relacionada à atualização de dados. Os detalhes desse erro estão listados no campo "`message`" da resposta da API.
<span data-anchor="api-code-30">30</span>   | int | O excesso de operações permitidas limita o erro dentro de um intervalo de tempo específico. Este erro ocorre devido a solicitações excessivamente frequentes para o mesmo método de API. Caso ocorra, reduza a frequência das solicitações.
<span data-anchor="api-code-98">98</span>   | int | A operação não foi realizada totalmente, mas com parte dos dados. Normalmente, você obtém este código para qualquer operação em massa, durante a execução da qual alguns elementos não foram processados devido a erros ou restrições, mas outros foram processados. Ao receber este código, pode verificar a informação dos elementos processados e não processados e procurar erros no conteúdo do campo "`data`".
<span data-anchor="api-code-99">99</span>   | int | Nenhum dos elementos de operação em massa não foi processado. Para obter informações detalhadas sobre os erros em cada elemento específico, verifique o campo "dados", para a descrição geral do erro - o campo "`message`".
<span data-anchor="api-code-100">100</span> | int | Este código não é um erro e significa que a operação é executada como um processo em segundo plano. Neste caso, o campo "`data`" contém o ID do processo em segundo plano, cujo estado pode ser verificado usando o método `Taskqueue::GetStatus`.
<span data-anchor="api-code-999">999</span> | int | Erro geral. Para obter detalhes, verifique o campo "`message`".


### SMS status
Status   | Final         | Description
---------|---------------|------------------------------------
`NEW`    |      não      | Nova mensagem, ainda não enviada.
`ENQUEUD`|      não      | Passou na moderação e entrou na fila para envio.
`ACCEPTD`|      não      | Enviado do sistema e aceito pela operadora para posterior envio ao destinatário.
`UNDELIV`|      sim      | Não entregue ao destinatário.
`REJECTD`|      sim      | Recusado pela operadora por um de vários motivos - **número do destinatário errado**, **texto proibido**, **remetente bloqueado pelo destinatário ou vice-versa**, etc..
`PDLIVRD`|      não      | Nem todos os segmentos de mensagem foram entregues ao destinatário (esse status se aplica apenas a mensagens, mas não a segmentos). Algumas operadoras retornam um relatório de entrega apenas para o primeiro segmento da mensagem longa, é por isso que o status de tais mensagens será alterado para `DELIVRD` após o período de expiração.
`DELIVRD`|      sim      | Entregue ao destinatário totalmente.
`EXPIRED`|      sim      | A entrega falhou porque a mensagem expirou (**3 dias por padrão**).
`DELETED`|      sim      | Excluído devido a restrições e não entregue ao destinatário.

## Suporte
Caso haja dúvidas ou encontrar algum problema, envie email para suporte@mobizon.com.br.

## Módulo Mensagem SMS [/message]

### Envio de mensagem única [POST /message/sendSmsMessage]

Permite enviar uma única mensagem para um número de celular especificado, mais detalhes: [#SendSmsMessage](https://mobizon.com.br/en/help/api-docs/message#SendSmsMessage).

+ Request (application/x-www-form-urlencoded)
    
    + Attributes
    
        + apiKey (string, required) - Sua Key da API (você precisa obtê-la em seu painel de controle).
        + output (string, optional) - Formato de resposta do servidor.
            + Default: `json`
        + api (string, optional) - Versão da API.
            + Default: `v1`
        + recipient (string, required) - Número de telefone do destinatário em formato internacional (sem incluir o caractere +), ex.: 5511963928063.
        + text (string, required) - Texto da sua mensagem SMS no formato de string URL encoded: "Texto da mensagem aqui!".
        + from (string, optional) - Seu Sender ID (assinatura alfanumérica). Se nenhum for fornecido, seu Sender ID padrão será usado ou, se você não tiver nenhum, o Sender ID padrão do sistema.

    + Headers

            Cache-control: no-cache
            
    + Body 

            recipient=5511999865802&text=Mobizon SMS API&from=&output=json&api=v1&apiKey=brXXXXXXXXXXXXXX


+ Response 200 (application/json)

    + Body

            {
              "code": 0,
              "data": {
                "campaignId": "1111111",
                "messageId": "11111111",
                "status": 2
              },
              "message": ""
            }

### Lista de mensagens SMS [POST /message/List]

Lista todas as mensagens SMS enviadas, mais detalhes: [#List](https://mobizon.com.br/en/help/api-docs/message#List).

+ Request (application/x-www-form-urlencoded)
    
    + Attributes 
    
        + apiKey (string, required) - Sua Key da API (você precisa obtê-la em seu painel de controle).
        + output (string, optional) - Formato de resposta do servidor.
            + Default: `json`
        + api (string, optional) - Versão da API.
            + Default: `v1`
        + criteria (array[ListSMS], optional)
        + pagination (array[Pagination], optional)
        + sort (array[Sort], optional)
      
    
    + Headers

            Cache-control: no-cache
            
    + Body 

            criteria%5Bfrom%5D=&pagination%5BcurrentPage%5D=2&pagination%5BpageSize%5D=50&sort%5BcampaignId%5D=ASC&apiKey=brXXXXXXXXXXXXXX


+ Response 200 (application/json)

    + Body

            {
              "code": 0,
              "data": {
                "items": [
                  {
                    "contentProviderName": null,
                    "partnerId": "8",
                    "userId": "11111",
                    "usedStopListWords": null,
                    "countryA2": null,
                    "operatorName": null,
                    "id": "11111",
                    "campaignId": "333333333",
                    "segNum": "1",
                    "segUserBuy": "0.0600",
                    "status": "DELIVRD",
                    "uuid": "bb1a64f6-0017-1a8a-9fa7-xxxxxxxxxxx",
                    "from": "N-666666",
                    "to": "5511999865802",
                    "groups": null,
                    "text": "Mobizon SMS API",
                    "startSendTs": "2021-09-04 13:31:12",
                    "statusUpdateTs": "2021-09-04 13:31:19",
                    "contactCardId": null,
                    "contentProviderId": null,
                    "segPartnerSell": null,
                    "segPartnerBuy": null,
                    "segSystemSell": null,
                    "segSystemBuy": null,
                    "systemCurrency": null,
                    "campaign": {
                      "commonStatus": "DONE",
                      "counters": {
                        "campaignId": "399768",
                        "updateTs": "2021-09-04 13:31:29",
                        "totalNewSegNum": "0",
                        "totalEnqueudSegNum": "0",
                        "totalAcceptdSegNum": "0",
                        "totalDelivrdSegNum": "1",
                        "totalRejectdSegNum": "0",
                        "totalExpiredSegNum": "0",
                        "totalUndelivSegNum": "0",
                        "totalDeletedSegNum": "0",
                        "totalUnknownSegNum": "0",
                        "totalPdlivrdSegNum": "0",
                        "totalSegNum": "1",
                        "totalNewMsgNum": "0",
                        "totalEnqueudMsgNum": "0",
                        "totalAcceptdMsgNum": "0",
                        "totalDelivrdMsgNum": "1",
                        "totalRejectdMsgNum": "0",
                        "totalExpiredMsgNum": "0",
                        "totalUndelivMsgNum": "0",
                        "totalDeletedMsgNum": "0",
                        "totalUnknownMsgNum": "0",
                        "totalPdlivrdMsgNum": "0",
                        "totalMsgNum": "1",
                        "totalCost": "0.0600",
                        "totalPartnerCost": "0.0500",
                        "totalNewMsgCost": "0.0000",
                        "totalEnqueudMsgCost": "0.0000",
                        "totalAcceptdMsgCost": "0.0000",
                        "totalDelivrdMsgCost": "0.0600",
                        "totalRejectdMsgCost": "0.0000",
                        "totalExpiredMsgCost": "0.0000",
                        "totalUndelivMsgCost": "0.0000",
                        "totalDeletedMsgCost": "0.0000",
                        "totalUnknownMsgCost": "0.0000",
                        "totalPdlivrdMsgCost": "0.0000",
                        "partnerCurrency": "BRL",
                        "userCurrency": "BRL",
                        "recipientsRejected": "0"
                      },
                      "createTs": "2021-09-04 13:31:11",
                      "creationWay": "1",
                      "currency": null,
                      "deferredToTs": null,
                      "endSendTs": "2021-09-04 13:31:19",
                      "expirationTs": "2021-09-03 07:31:29",
                      "extra": {
                        "validity": "1440",
                        "mclass": "1",
                        "isTestAlphanameUsed": true,
                        "coding": "0",
                        "charset": "UTF-8",
                        "trackShortLinkRecipients": 0
                      },
                      "from": "SMS",
                      "globalComment": null,
                      "globalModStatus": "AUTO_READY_FOR_SEND",
                      "groups": null,
                      "groupsList": [],
                      "highlightedText": null,
                      "id": "399768",
                      "isTemplateComment": null,
                      "moderationStatus": "READY_FOR_SEND",
                      "msgType": "SMS",
                      "name": null,
                      "partnerId": "8",
                      "partnerModStatus": "AUTO_READY_FOR_SEND",
                      "rateLimit": null,
                      "ratePeriod": null,
                      "recipientsSource": null,
                      "sendStatus": "DONE",
                      "startSendTs": "2021-09-04 13:31:12",
                      "text": "Mobizon SMS API",
                      "type": "1",
                      "usedStopListWords": null,
                      "userId": "1111111"
                    }
                  }
                ],
                "totalItemCount": "1"
              },
              "message": ""
            }

### Relatório de status de entrega SMS [POST /message/GetSMSStatus]

O método aceita uma string com um ID de mensagem e uma matriz de IDs de mensagem. Independentemente do tipo do parâmetro de entrada, o resultado retornado é sempre representado como uma matriz. Se você enviar IDs de mensagens inexistentes ou não pertencentes aos usuários, o resultado não conterá informações sobre essas mensagens, mais detalhes: [#GetSMSStatus](https://mobizon.com.br/en/help/api-docs/message#GetSMSStatus).

+ Request (application/x-www-form-urlencoded)
    
    + Attributes
    
        + apiKey (string, required) - Sua Key da API (você precisa obtê-la em seu painel de controle).
        + output (string, optional) - Formato de resposta do servidor.
            + Default: `json`
        + api (string, optional) - Versão da API.
            + Default: `v1`
        + ids (array[string], optional) - Message ID(s) is (are) an array or a string of Ids, separated by commas. Maximum equals to 100 characters.
        
    + Headers

            Cache-control: no-cache
            
    + Body 

            ids%5B0%5D=123&ids%5B1%5D=556&ids%5B2%5D=988


+ Response 200 (application/json)

    + Body

            {
              "code": 0,
              "data": [
                {
                  "id": "123",
                  "status": "DELIVRD",
                  "segNum": "1",
                  "startSendTs": "2021-09-04 13:28:29",
                  "statusUpdateTs": "2021-09-04 13:28:35"
                }
              ],
              "message": ""
            }

# Data Structures

## SendSMS

+ apiKey (string, required) - Sua Key da API (você precisa obtê-la em seu painel de controle).
+ recipient (string, required) - Número de telefone do destinatário em formato internacional (sem incluir o caractere +), ex.: 5511963928063.
+ text (string, required) - Texto da sua mensagem SMS no formato de string URL encoded: "Texto da mensagem aqui!".
+ from (string, optional) - Seu Sender ID (assinatura alfanumérica). Se nenhum for fornecido, seu Sender ID padrão será usado ou, se você não tiver nenhum, o Sender ID padrão do sistema.
+ output (string, optional) - Formato de resposta do servidor.
    + Default: `json`
+ api (string, optional) - Versão da API.
    + Default: `v1`
    
## ListSMS

+ id (number, optional) - Message ID
+ campaignId (number, optional) - Campaign ID
+ campaignIds (array, optional) - Search by campaign IDs; the parameter should be passed as an array or a string of IDs, separated by commas, the maximum number of IDs equals to 10; when this limit is exceeded, the search will occur on the first 10 entries of the list (if this parameter is set, then a forced limitation on the creation date is not used and search runs on all campaigns ever created)
+ from (string, optional) - Sender's signature
+ to (string, optional) - Recipient number
+ text (string, optional) - Message text
+ status (number, optional) - Message status
+ groups (string, optional) - Message recipient groups
+ contentProviderId (string, optional) - SMS centre ID
    
## Pagination

+ pageSize (number, optional) - Number of visible elements on the page
+ currentPage (number, optional) - Current page

## Sort
+ id (number, optional) - Message ID
+ campaignId (number, optional) - Campaign ID
+ from (number, optional) - Senders signature
+ to (number, optional) - Recipient number
+ text (number, optional) - Message text
+ status (number, optional) - Message status
+ groups (number, optional) - Message recipient groups
+ contentProviderId (number, optional) - SMS centre ID
