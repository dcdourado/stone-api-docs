---
title: "Regras de negócio"
slug: "regras-de-negócio"
draft: false
weight: 3
date: "2020-04-26T22:22:46.764Z"
lastmod: "2020-10-24T00:14:01.560Z"
---
#### Válidas para todos os tipos de boleto

| Regras              | Todos os tipos                                                                                                                                                  |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Registro            | O registro de boletos é automático e feito em até alguns poucos minutos após sua emissão.                                                                       |
| Baixa final         | Um boleto não pago sofrerá baixa automática sete dias após sua data limite.                                                                                     |
| Baixa por pagamento | Ao ser pago o boleto sofrerá baixa automática, impedindo novos pagamentos desse mesmo documento.                                                                |
| Baixa solicitada    | Quando o amissor solicita a baixa de um boleto que ainda não foi pago e não chegou a data limite.                                                               |
| Data de vencimento  | Precisa ser igual ou maior que o dia atual e igual ou menor do que a limit_date. Caso seja um dia não útil o pagamento poderá ser feito até o próximo dia útil. |
| Formato / Layout    | Hoje os dados do boleto são devolvidos no formato PDF através de um link.                                                                                       |


<br>
<br>

#### Variáveis por tipo de boleto


| Regras                            | Depósito                                                                                                                                                                                                                                                                                                                                  | Proposta                                                                                                                                                                                                                                                                                                                                                         | Cobrança *(todos os tipos)*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Disponível para                   | Todas as contas de pagamento da base.                                                                                                                                                                                                                                                                                                     | Todas as contas de pagamento da base.                                                                                                                                                                                                                                                                                                                            | É necessário sinalizar na integração para habilitação desse tipo para a conta pagamento.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Data limite<br>*(limit_date)*     | Sempre deve ser igual a data de vencimento, podendo ser até 365 dias após a data de emissão. <br>Caso seja um dia não útil o pagamento poderá ser feito até o próximo dia útil.<br>Caso seja informada alguma data diferente da vencimento retornará erro. Caso não seja informada será configurada automaticamente a data de vencimento. | Deve ser igual ou maior a data de vencimento.<br><br>Caso não seja informada será configurada automaticamente a data de vencimento.                                                                                                                                                                                                                              | Deve ser igual ou maior a data de vencimento.<br><br>Caso não seja informada será configurada automaticamente a data de vencimento.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Pagamento                         | Não obrigatório                                                                                                                                                                                                                                                                                                                           | Não obrigatório                                                                                                                                                                                                                                                                                                                                                  | Obrigatório                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Pagador<br> *(customer)*          | Sempre deve ser igual ao beneficiário.<br>Caso seja informado algum dado diferente do beneficiário será ignorado.<br> Caso não seja informado será configurado automaticamente com dados do beneficiário.                                                                                                                                 | Pode ser diferente do beneficiário.<br><br>Obrigatório o envio do objeto.                                                                                                                                                                                                                                                                                        | Pode ser diferente do beneficiário.<br><br>Obrigatório o envio do objeto.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Valor<br> *(amount)*              | Valor Min de R$ 20,00 e valor Max: R$ 10.000,00.                                                                                                                                                                                                                                                                                          | Valor Min de R$ 20,00 e valor Max: R$ 10.000,00.                                                                                                                                                                                                                                                                                                                 | Valor Min de R$ 20,00 e valor Max: R$ 10.000,00.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Juros<br> *(interest)*            | Não suporta                                                                                                                                                                                                                                                                                                                               | Não suporta                                                                                                                                                                                                                                                                                                                                                      | Juros deve ser maior que 0% e até 1% ao mês.<br>O valor é aplicado por dia levando em conta o valor proporcional dado o juros mensal. Ex: 1% ao mês, seria 0,033% ao dia.<br>O juros passam a contar a partir da data informada. Essa data tem que ser maior que o vencimento e menor que data limite. Caso nenhuma data seja informada serão aplicados a patir do dia seguinte ao vencimento.<br>Nos casos em que a data de vencimento cai em um dia não últil ela é automaticamente estendida para o próximo dia útil e os juros só passam a ser aplicados depois dessa data.<br>Campo não obrigatório, só preencher se for aplicar juros. |
| Multa<br> *(fine)*                | Não suporta                                                                                                                                                                                                                                                                                                                               | Não suporta                                                                                                                                                                                                                                                                                                                                                      | Multa deve ser maior que 0% e até 2% do valor do boleto.<br>A multa é cobrada uma única vez.<br>A multa passa a contar a partir da data informada. Essa data tem que ser maior que o vencimento e menor que data limite.<br>Caso nenhuma data seja informada será aplicada a patir do dia seguinte ao vencimento.<br>Nos casos em que a data de vencimento cai em um dia não últil ela é automaticamente estendida para o próximo dia útil e a multa só passará a ser aplicada depois dessa data.<br>Campo não obrigatório, só preencher se for aplicar multa.                                                                               |
| Desconto<br> *(discount)*         | Não suporta                                                                                                                                                                                                                                                                                                                               | Não suporta                                                                                                                                                                                                                                                                                                                                                      | Desconto deve ser maior que 0% e até 90%.<br>O desconto se aplica caso o pagameto seja feito até a data informada. A data informada deve ser menor do que a data de vencimento do boleto e maior que sua data de emissão.<br>Campo não obrigatório, só preencher se for aplicar desconto.                                                                                                                                                                                                                                                                                                                                                     |
| Múltiplos descontos               | Não suporta                                                                                                                                                                                                                                                                                                                               | Não suporta                                                                                                                                                                                                                                                                                                                                                      | É possível aplicar até 3 descontos a um boleto onde cada um terá sua data até a qual o desconto é válido.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Sacador avalista<br> *(receiver)* | Não suporta                                                                                                                                                                                                                                                                                                                               | Quando um boleto é emitido em uma conta, mas o comprador final reconhece a compra em nome de outra empresa é possível usar o campo sacador avalista para apresentar o nome dessa empresa no boleto.<br>O beneficiário final não muda, ou seja, o valor do boleto será liquidado para a conta que o emitiu.<br>Funcionalidade comumente usada por subadquirentes. | Quando um boleto é emitido em uma conta, mas o comprador final reconhece a compra em nome de outra empresa é possível usar o campo sacador avalista para apresentar o nome da empresa que o comprador reconhe no boleto.<br>O beneficiário final não muda, ou seja, o valor do boleto será liquidado para a conta que o emitiu.<br>Funcionalidade comumente usada por subadquirentes.                                                                                                                                                                                                                                                        |
---


{{% pageinfo %}}
**Atenção**

Desconto em boleto não é valido para pagamentos realizados no final de semana.

{{% /pageinfo %}}