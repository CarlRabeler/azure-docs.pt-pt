---
title: Ligar a Base de Dados do Azure para PostgreSQL a partir de C# | Microsoft Docs
description: "Este início rápido fornece um código de exemplo de C# (.NET) que pode utilizar para se ligar e consultar dados da Base de Dados do Azure para PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: csharp
ms.topic: hero-article
ms.date: 06/23/2017
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: b9382de2b8c672670213d9f5d0daf1eb0bff8c78
ms.contentlocale: pt-pt
ms.lasthandoff: 07/27/2017

---

# <a name="azure-database-for-postgresql-use-net-c-to-connect-and-query-data"></a>Base de Dados do Azure para PostgreSQL: utilizar .NET (C#) para ligar e consultar dados
Este guia de introdução explica como se pode ligar a uma Base de Dados do Azure para PostgreSQL através de uma aplicação C#. Explica como utilizar as instruções SQL para consultar, inserir, atualizar e eliminar dados da base de dados. Os passos neste artigo pressupõem que está familiarizado com a programação com C# e que nunca trabalhou com a Base de Dados do Azure para PostgreSQL.

## <a name="prerequisites"></a>Pré-requisitos
Este guia de início rápido utiliza os recursos criados em qualquer um destes guias como ponto de partida:
- [Criar BD - Portal](quickstart-create-server-database-portal.md)
- [Criar BD - CLI](quickstart-create-server-database-azure-cli.md)

Também tem de:
- Instalar o [.NET Framework](https://www.microsoft.com/net/download). Siga os passos no artigo ligado para instalar o .NET especificamente para a sua plataforma (Windows, Ubuntu Linux ou macOS). 
- Instale o [Visual Studio](https://www.visualstudio.com/downloads/) ou o Visual Studio Code para escrever e editar o código.
- Instale a biblioteca [Npgsql](http://www.npgsql.org/doc/index.html) conforme descrito abaixo.

## <a name="install-npgsql-references-into-your-visual-studio-solution"></a>Instalar as referências de Npgsql na sua solução do Visual Studio
Para ligar a partir da aplicação C# ao PostgreSQL, utilize a biblioteca ADO.NET de código aberto, chamada Npgsql. O NuGet ajuda a transferir e gerir facilmente as referências.

1. Crie uma nova solução C#, ou abra uma existente: 
   - No Visual Studio, crie uma solução, clicando no menu Ficheiro **Novo** > **Projeto**.
   - Na caixa de diálogo Novo Projeto, expanda **Modelos** > **Visual C#**. 
   - Escolha um modelo apropriado, como **Aplicação de Consola (.NET Core)**.

2. Utilize o Gestor de Pacote Nuget para instalar o Npgsql:
   - Clique no menu **Ferramentas** > **Gestor de Pacotes NuGet** > **Consola do Gestor de Pacotes**.
   - Na **Consola do Gestor de Pacotes**, escreva `Install-Package Npgsql`
   - O comando de instalação transfere o Npgsql.dll e as assemblagens relacionadas e adiciona-as como dependências à solução.

## <a name="get-connection-information"></a>Obter informações da ligação
Obtenha as informações de ligação necessárias para se ligar à Base de Dados do Azure para PostgreSQL. Necessita do nome do servidor e das credenciais de início de sessão totalmente qualificados.

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. No menu esquerdo do portal do Azure, clique em **Todos os recursos** e procure o servidor que acabou de criar, por exemplo, **mypgserver-20170401**.
3. Clique no nome do servidor **mypgserver 20170401**.
4. Selecione a página **Descrição Geral** do servidor. Anote o **Nome do servidor** e **Nome de início de sessão de administrador do servidor**.
 ![Base de Dados do Azure para o PostgreSQL – Início de sessão de administrador do servidor](./media/connect-csharp/1-connection-string.png)
5. Caso se tenha esquecido das informações de início de sessão do seu servidor, navegue até à página **Descrição geral** para visualizar o nome de início de sessão de administrador do servidor e, se necessário, repor a palavra-passe.

## <a name="connect-create-table-and-insert-data"></a>Ligar, criar tabela e inserir dados
Utilize o código para ligar e carregar os dados com as instruções SQL **CREATE TABLE** e **INSERT INTO**. O código utiliza a classe NpgsqlCommand com o método [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para estabelecer uma ligação ao PostgreSQL. Em seguida, o código utiliza o método [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), define a propriedade CommandText e chama o método [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) para executar os comandos da base de dados. 

Substitua os parâmetros Anfitrião, DBName, Utilizador e Palavra-passe pelos valores que especificou ao criar o servidor e a base de dados. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresCreate
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4}; SSL Mode=Prefer; Trust Server Certificate=true",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "DROP TABLE IF EXISTS inventory;";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished dropping table (if existed)");

            command.CommandText = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
            command.ExecuteNonQuery();
            Console.Out.WriteLine("Finished creating table");

            command.CommandText =
                String.Format(
                    @"
                                INSERT INTO inventory (name, quantity) VALUES ({0}, {1});
                                INSERT INTO inventory (name, quantity) VALUES ({2}, {3});
                                INSERT INTO inventory (name, quantity) VALUES ({4}, {5});
                            ",
                    "\'banana\'", 150,
                    "\'orange\'", 154,
                    "\'apple\'", 100
                    );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="read-data"></a>Ler dados
Utilize o código seguinte para se ligar e ler dados com uma instrução SQL **SELECT**. O código utiliza a classe NpgsqlCommand com o método [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para estabelecer uma ligação ao PostgreSQL. O código utiliza então o método [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) e o método [ExecuteReader ()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) para executar os comandos da base de dados. Em seguida, o código utiliza [Read()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) para avançar para os registos nos resultados. Em seguida, o código utiliza [GetInt32()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) e [GetString()](http://www.npgsql.org/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) para analisar os valores do registo.

Substitua os parâmetros Anfitrião, DBName, Utilizador e Palavra-passe pelos valores que especificou ao criar o servidor e a base de dados. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresRead
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText = "SELECT * FROM inventory;";

            var reader = command.ExecuteReader();
            while (reader.Read())
            {
                Console.WriteLine(
                    string.Format(
                        "Reading from table=({0}, {1}, {2})",
                        reader.GetInt32(0).ToString(),
                        reader.GetString(1),
                        reader.GetInt32(2).ToString()
                        )
                    );
            }

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


## <a name="update-data"></a>Atualizar dados
Utilize o código seguinte para se ligar e ler dados com uma instrução SQL **UPDATE**. O código utiliza a classe NpgsqlCommand com o método [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para estabelecer uma ligação ao PostgreSQL. Em seguida, o código utiliza o método [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), define a propriedade CommandText e chama o método [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) para executar os comandos da base de dados.

Substitua os parâmetros Anfitrião, DBName, Utilizador e Palavra-passe pelos valores que especificou ao criar o servidor e a base de dados. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresUpdate
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("UPDATE inventory SET quantity = {0} WHERE name = {1};",
                200,
                "\'banana\'"
                );

            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```


## <a name="delete-data"></a>Eliminar dados
Utilize o código seguinte para se ligar e ler os dados com uma instrução SQL **DELETE**. 

 O código utiliza a classe NpgsqlCommand com o método [Open()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para estabelecer uma ligação ao PostgreSQL. Em seguida, o código utiliza o método [CreateCommand()](http://www.npgsql.org/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), define a propriedade CommandText e chama o método [ExecuteNonQuery()](http://www.npgsql.org/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) para executar os comandos da base de dados.

Substitua os parâmetros Anfitrião, DBName, Utilizador e Palavra-passe pelos valores que especificou ao criar o servidor e a base de dados. 

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Npgsql;

namespace Driver
{
    public class AzurePostgresDelete
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mypgserver-20170401.postgres.database.azure.com";
        private static string User = "mylogin@mypgserver-20170401";
        private static string DBname = "mypgsqldb";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            var conn = new NpgsqlConnection(connString);

            Console.Out.WriteLine("Opening connection");
            conn.Open();

            var command = conn.CreateCommand();
            command.CommandText =
            String.Format("DELETE FROM inventory WHERE name = {0};",
                "\'orange\'");
            int nRows = command.ExecuteNonQuery();
            Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));

            Console.Out.WriteLine("Closing connection");
            conn.Close();

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Migrar a base de dados com Exportar e Importar](./howto-migrate-using-export-and-import.md)

