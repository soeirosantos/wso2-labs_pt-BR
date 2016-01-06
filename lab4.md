# LAB 4 - File System VfsStockQuoteProxy

### Objetivo

Neste laboratório vamos relaizar polling em um diretório para checar a presença de
arquivos xml e, a partir do conteúdo do arquivo, enviar uma requisição SOAP ao
nosso já conhecido `SimpleStockQuoteService`. O resultado será gravado em um novo
diretório.

### Recursos

* WSO2 ESB 4.9.0 (certifique-se que o offset da porta esteja devidamente configurado).

### Procedimento

1. Selecione um local no seu sistema de arquivos e crie os diretórios `in`,
`original` e `out`.

  * Na pasta `in` vamos colocar os arquivos a serem processados;
  * Na pasta `original` vamos colocar todos os arquivos processados;
  * E na pasta `out` vamos gravar os arquivos contendo o resultado.

2. Vamos usar o mesmo serviço de exemplo, `SimpleStockQuoteService`, utilizado no
**Lab 2**, que eu suponho que você tenha feito antes deste (se não executou, antes
   de continuar, realize os passos 1, 2 e 3 do **Lab 2**).

3. Inicie o ESB:

  ```shell
  $ cd $ESB_HOME/bin
  $ ./wso2server.sh
  ```
Acesse o Management Console usando o caminho https://localhost:9445/carbon e as credenciais `admin`/`admin`.

4. Siga o caminho: `Add > Proxy Service > Custom Proxy`

5. Clique em *switch to source view* e substitua o conteúdo pelo xml abaixo:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="VfsStockQuoteProxy"
       transports="vfs"
       statistics="disable"
       trace="disable"
       startOnLoad="true">
   <target>
      <outSequence>
         <property name="transport.vfs.ReplyFileName"
                   expression="fn:concat(fn:substring-after(get-property('MessageID'), 'urn:uuid:'), '.xml')"
                   scope="transport"/>
         <property name="OUT_ONLY" value="true"/>
         <send>
            <endpoint>
               <address uri="vfs:file://caminho/para/diretorios/out"/>
            </endpoint>
         </send>
      </outSequence>
      <endpoint>
         <address uri="http://localhost:9000/services/SimpleStockQuoteService"
                  format="soap12"/>
      </endpoint>
   </target>
   <publishWSDL uri="file:repository/samples/resources/proxy/sample_proxy_1.wsdl"/>
   <parameter name="transport.PollInterval">15</parameter>
   <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
   <parameter name="transport.vfs.FileURI">file://caminho/para/diretorios/in</parameter>
   <parameter name="transport.vfs.MoveAfterProcess">file://caminho/para/diretorios/original</parameter>
   <parameter name="transport.vfs.MoveAfterFailure">file://caminho/para/diretorios/original</parameter>
   <parameter name="transport.vfs.FileNamePattern">.*\.xml</parameter>
   <parameter name="transport.vfs.ContentType">text/xml</parameter>
   <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>
   <description/>
</proxy>
```
Nesse xml podemos ver uma série de parâmetros, conhecidos como service-level parameters, que configuram o comportamento esperado para a manipulação de arquivos. No link a seguir há uma tabela com a descrição de cada um dos parâmetros suportados: https://docs.wso2.com/display/ESB481/VFS+Transport#VFSTransport-parametersVFSservice-levelparameters

6. Clique em Save e Finalize.

7. Agora, crie um arquivo chamado `test.xml` na pasta `in` com o seguinte conteúdo:

  ```xml
  <?xml version='1.0' encoding='UTF-8'?>
  <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing">
      <soapenv:Body>
          <m0:getQuote xmlns:m0="http://services.samples">
              <m0:request>
                  <m0:symbol>WSO2</m0:symbol>
              </m0:request>
          </m0:getQuote>
      </soapenv:Body>
  </soapenv:Envelope>
  ```

8. Se tudo estiver ok, no log do serviço é possível ver que o ele foi invocado. E, na pasta out, você será capaz de ver o conteúdo do resultado.

9. **Bônus** -  Se você quiser um serviço mais sofisticado, no passo 5 utilize o xml abaixo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse"
       name="ComplexStockQuoteProxy"
       transports="https http vfs"
       startOnLoad="true"
       trace="disable">
   <description/>
   <target>
      <inSequence>
         <log level="full"/>
         <log level="custom">
            <property name="Symbol" expression="//symbol/text()"/>
         </log>
         <payloadFactory media-type="xml">
            <format>
               <ser:getQuote xmlns:ser="http://services.samples" xmlns:xsd="http://services.samples/xsd">
                  <ser:request>
                     <xsd:symbol>$1</xsd:symbol>
                  </ser:request>
               </ser:getQuote>
            </format>
            <args>
               <arg evaluator="xml" expression="//symbol/text()"/>
            </args>
         </payloadFactory>
         <header name="Action" scope="default" value="urn:getQuote"/>
         <log level="full">
            <property name="TRANSFORMED MESSAGE IS" value="PRINTED!!!"/>
         </log>
         <send>
            <endpoint>
         	<address uri="http://localhost:9000/services/SimpleStockQuoteService"
                  format="soap12"/>
	    </endpoint>
         </send>
      </inSequence>
      <outSequence>
         <log level="full"/>
         <property name="messageType"
                   value="application/json"
                   scope="axis2"
                   type="STRING"/>
         <property name="transport.vfs.ReplyFileName"
                   expression="fn:concat(fn:substring-after(get-property('MessageID'), 'urn:uuid:'), '.json')"
                   scope="transport"
                   type="STRING"/>
         <property name="OUT_ONLY" value="true" scope="default" type="STRING"/>
         <send>
            <endpoint>
               <address uri="vfs:file://caminho/para/diretorios/out"/>
            </endpoint>
         </send>
      </outSequence>
   </target>
   <parameter name="transport.PollInterval">15</parameter>
   <parameter name="transport.vfs.ActionAfterProcess">MOVE</parameter>
   <parameter name="transport.vfs.FileURI">file://caminho/para/diretorios/in</parameter>
   <parameter name="transport.vfs.MoveAfterProcess">file://caminho/para/diretorios/original</parameter>
   <parameter name="transport.vfs.MoveAfterFailure">file://caminho/para/diretorios/original</parameter>
   <parameter name="transport.vfs.FileNamePattern">.*\.xml</parameter>
   <parameter name="transport.vfs.ContentType">application/xml</parameter>
   <parameter name="transport.vfs.ActionAfterFailure">MOVE</parameter>

</proxy>
```
