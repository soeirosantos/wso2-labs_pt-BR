# LAB 6 - DSS da tabela Employees - CRUD Rest

### Objetivo

O objetivo deste laboratório é expor uma tabela do banco de dados através
uma API REST.

### Recursos

* WSO2 DSS 3.2.2
* Um banco de dados com uma tabela populada
* Driver de banco

### Procedimento

//TODO: inlcuir como configurar o Driver no DSS

1. Depois de logar no DSS, selecione Data Service criado no **Lab 5** e navegue
até `Resources`.

2. Clicando em `Add New Resource`, informe o Resource Path e selecione a query
desejada para cada funcionalidade, p. ex:

|Method|Resource Path     |Query                          |
|------|------------------|-------------------------------|
|GET   |/employees        |select_all_employees_query     |
|GET   |/employees/{empid}|select_with_key_employees_query|
|POST  |/employees        |insert_employees_query         |
|PUT   |/employees        |update_employees_query         |
|DELETE|/employees/{empid}|delete_employees_query         |

3. Utilize o Advanced Rest Client para testar. Lembre-se que os recursos estão
disponíveis a partir do seguinte contexto:
`/services/employees_DataService/`
