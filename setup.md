# Detalhes de configuração do ambiente

Este é o conjunto de produto WSO2 e ferramentas que utilizaremos durantes os
laboratórios. Recomenda-se que você só instale os produtos a medida que forem
necessários nos Labs. Muito cuidado e atenção na configuração do off-set das
portas, pois em alguns momentos trabalharemos com até 4 produtos ao mesmo tempo.

1. Java 1.7
2. WSO2 ESB 4.8.1 (http://docs.wso2.com/display/ESB481/Getting+Started)
  * Instale o Patch ESBJAVA-3272 (Resolve um bug de redeploy) (https://wso2.org/jira/secure/attachment/33396/org.wso2.carbon.application.upload_4.2.0.jar)
  * Faça download do jar e copie para ESB_HOME/repository/components/patches/patch9999
(será necessário criar o diretório /patch9999).
3. WSO2 App Server 5.2.1 (http://docs.wso2.com/display/AS521/Getting+Started)
4. WSO2 DSS Server (https://docs.wso2.com/display/DSS322/Getting+Started)
5. WSO2 APIM 1.9.0 (https://docs.wso2.com/display/AM190/Getting+Started)
6. WSO2 BAM 2.5.0 (https://docs.wso2.com/display/BAM250/Getting+Started)
7. WSO2 IS 5.0.0
8. WSO2 CEP 3.1.0
9. WSO2 GREG 4.6.0
10. SoapUI 5.0.0 ou superior (http://www.soapui.org/)
11. MySQL Server e MySQL Client (http://www.mysql.com/downloads/)
12. Apache Ant (http://ant.apache.org/bindownload.cgi)
13. Google Chrome (https://support.google.com/chrome/answer/95346?hl=en)
14. Advanced Rest Client (https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo)

## Off-set das portas

O off-set de porta dos produtos pode ser configurado no arquivo: [PRODUCT_HOME]/repository/conf/carbon.xml

|Produto|Porta|
|-------|-----|
|API-M  |0    |
|BAM    |1    |
|ESB    |2    |
|DSS    |3    |
|AS     |4    |
|GREG   |5    |
