# LAB 5 - DSS da tabela Employees - CRUD Soap

### Objetivo

O objetivo deste laboratório é expor uma tabela do banco de dados através de
serviços SOAP.

### Recursos

* WSO2 DSS 3.2.2
* Um banco de dados com uma tabela populada
* Driver de banco

### Procedimento

//TODO: inlcuir como configurar o Driver no DSS

1. Depois de logar no DSS, crie o DataSource,
`Configure > Data Sources > Add Data Source` (a criação do DS é bastante simples
   e auto-explicativa).

2. Agora vamos gerar o Data Service para a tabela do banco
  * Acesse `Main > Data Services > Generate`
  * Escolha o DataSource criado no passo (1)
  * Informe o nome do banco
  * Selecione a tabela (Dica: comece testando com uma tabela)
  * Mantenha a seleção de **Multiple Services** e informe um namespace qualquer
  (p. ex: dss.exemplo1.com.br)
  * Finalize

3. As operações foram criadas, utilize o SOAP UI para testar o serviço. É possível obter o WSDL na listagem de `Services`.
