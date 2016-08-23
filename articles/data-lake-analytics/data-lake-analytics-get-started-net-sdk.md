<properties 
   pageTitle="Introdução à Análise do Azure Data Lake ao utilizar o SDK do .NET | Azure" 
   description="Saiba como utilizar o SDK do .NET para criar contas de Data Lake Store, criar tarefas de Data Lake Analytics e submeter tarefas escritas em U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/22/2016"
   ms.author="edmaca"/>

# Tutorial: introdução à Análise do Azure Data Lake ao utilizar o SDK do .NET

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Saiba como utilizar o SDK do .NET para criar contas de Azure Data Lake Analytics, definir as tarefas de Data Lake Analytics em [U-SQL](data-lake-analytics-u-sql-get-started.md) e submeter as tarefas para contas de Data Lake Analytics. Para mais informações sobre a Data Lake Analytics, consulte [Descrição geral da Análise do Azure Data Lake](data-lake-analytics-overview.md).

Neste tutorial, irá desenvolver uma aplicação de consola C# que contém um script U_SQL que lê um ficheiro de valores separados por tabulações (TSV) e converte-o num ficheiro de valores separados por vírgulas (CSV). Para seguir o mesmo tutorial, utilizando outras ferramentas suportadas, clique nos separadores na parte superior desta secção.

[AZURE.INCLUDE [basic-process-include](../../includes/data-lake-analytics-basic-process.md)]

##Pré-requisitos

Antes de começar este tutorial, tem de ter o seguinte:

- **Visual Studio 2015, Visual Studio 2013 atualização 4 ou Visual Studio 2012 com Visual C++ instalado**.
- **SDK do Microsoft Azure para .NET versão 2.5 ou superior**.  Instale-lo através do [Instalador de Plataforma Web](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Data Lake Tools para Visual Studio](http://aka.ms/adltoolsvs)**. 
- Criar uma aplicação do Azure Active Directory (AAD) e obtenha o **ID de Cliente**, o **ID do Inquilino** e a **Chave**. Para mais informações sobre as aplicações do AAD e instruções sobre como obter um ID de cliente, consulte [Criar aplicação e serviço principal do Active Directory utilizando o portal](../resource-group-create-service-principal-portal.md). O URI de Resposta e a Chave também estarão disponíveis no portal assim que a aplicação tiver sido criada e a chave gerada.


##Criar aplicação da consola

Neste tutorial, irá processar alguns registos de pesquisa.  O registo de pesquisa pode ser armazenado no arquivo Data Lake ou no armazenamento de Blobs do Azure. 

Um registo de pesquisa de exemplo foi copiado para um contentor de Blobs do Azure público. Na aplicação, irá transferir o ficheiro para a estação de trabalho e, em seguida, carregar o ficheiro para a conta do Data Lake Store predefinida.

**Para criar uma aplicação**

1. Abra o Visual Studio.
2. Crie uma aplicação de consola C#.
3. Abra a consola de Gestão de Pacotes NuGet e execute os seguintes comandos:

        Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre
        Install-Package Microsoft.Azure.Management.DataLake.Store -Pre
        Install-Package Microsoft.Azure.Management.DataLake.StoreUploader -Pre
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
        Install-Package WindowsAzure.Storage

4. Adicione um novo ficheiro ao projeto denominado **SampleUSQLScript.txt**, e, em seguida, cole o seguinte script U-SQL. As tarefas de Data Lake Analytics são escritas em linguagem U-SQL. Para saber mais sobre U-SQL, consulte [Introdução à linguagem U SQL](data-lake-analytics-u-sql-get-started.md) e [Referência de linguagem U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Este script U-SQL lê o ficheiro de dados de origem, utilizando **Extractors.Tsv()**, e, em seguida, cria um ficheiro csv, utilizando **Outputters.Csv()**. 
    
    No programa C#, terá de preparar o ficheiro **/Samples/Data/SearchLog.tsv** e a pasta **/Output/**.    
    
    É mais simples utilizar caminhos relativos para ficheiros armazenados em contas do Data Lake predefinidas. Também pode utilizar caminhos absolutos.  Por exemplo 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Tem de utilizar caminhos absolutos para aceder a ficheiros em contas de Armazenamento ligadas.  A sintaxe para ficheiros armazenados numa Conta de Armazenamento do Azure ligada é:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Neste momento não existe um problema conhecido com o Azure Data Lake Service.  Se a aplicação de amostra for interrompida ou encontrar um erro, poderá ter de eliminar manualmente as contas do Data Lake Store e do Data Lake Analytics que o script cria.  Se não estiver familiarizado com o Portal, o guia [Gerir o Azure Data Lake Analytics utilizando o Portal do Azure](data-lake-analytics-manage-use-portal.md) ajudá-lo-á a começar.       
       
5. Em Program.cs, cole o seguinte código:

        using Microsoft.Azure.Management.DataLake.Analytics;
        using Microsoft.Azure.Management.DataLake.Analytics.Models;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.Azure.Management.DataLake.StoreUploader;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System;
        using System.Collections.Generic;
        using System.IO;
        
        namespace SdkSample
        {
          class Program
          {
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
        
            private static string _adlaAccountName;
            private static string _adlsAccountName;
            private static string _resourceGroupName;
            private static string _location;
            private static string _tenantId;
            private static string _subId;
            private static string _clientId;
            private static string _clientKey;
        
            private static void Main(string[] args)
            {
              _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name for a NEW Store account.
              _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>"; // TODO: Replace this value with the name for a NEW Analytics account.
              _resourceGroupName = "<RESOURCE-GROUP>"; // TODO: Replace this value. This resource group should already exist.
              _location = "East US 2";
              _tenantId = "<TENANT-ID>";
              _subId = "<SUBSCRIPTION-ID>";
              _clientId = "<CLIENT-ID>";
              _clientKey = "<CLIENT-KEY>";
        
              string localFolderPath = @"c:\temp\"; // TODO: Make sure this exists and contains SampleUSQLScript.txt.
              var tokenCreds = Authenticate(_tenantId, _clientId, _clientKey);
        
              SetupClients(tokenCreds, _subId); 
        
              // Run sample scenarios
              WaitForNewline("Authenticated.", "Creating NEW accounts.");
              CreateAccounts();
              WaitForNewline("Accounts created.", "Preparing the source data file.");
        
              // Transfer the source file from a public Azure Blob container to Data Lake Store.
              CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://adltutorials.blob.core.windows.net/adls-sample-data/SearchLog.tsv"));
              blob.DownloadToFile(localFolderPath + "SearchLog.tsv", FileMode.Create); // from WASB
              UploadFile(localFolderPath + "SearchLog.tsv", "/Samples/Data/SearchLog.tsv"); // to ADLS
              WaitForNewline("Source data file prepared.", "Submitting a job.");
        
              // Submit the job
              Guid jobId = SubmitJobByPath(localFolderPath + "SampleUSQLScript.txt", "My First ADLA Job");
              WaitForNewline("Job submitted.", "Waiting for job completion.");
        
              // Wait for job completion
              WaitForJob(jobId);
              WaitForNewline("Job completed.", "Downloading job output.");
        
              // Download job output
              DownloadFile(@"/Output/SearchLog-from-Data-Lake.csv", localFolderPath + "SearchLog-from-Data-Lake.csv");
              WaitForNewline("Job output downloaded.", "Deleting accounts.");
        
              // Delete accounts
              _adlaClient.Account.Delete(_resourceGroupName, _adlaAccountName);
              _adlsClient.Account.Delete(_resourceGroupName, _adlsAccountName);
        
              WaitForNewline("Accounts deleted. You can now exit.");
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(string reason, string nextAction = "")
            {
              Console.WriteLine(reason + "\r\nPress ENTER to continue...");
        
              Console.ReadLine();
        
              if (!String.IsNullOrWhiteSpace(nextAction))
                Console.WriteLine(nextAction);
            }
        
            public static TokenCredentials Authenticate(string tenantId, string clientId, string clientKey)
            {
              var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + _tenantId);
              var creds = new ClientCredential(_clientId, _clientKey);
              var tokenAuthResult = authContext.AcquireTokenAsync("https://management.core.windows.net/", creds).Result;
        
              return new TokenCredentials(tokenAuthResult.AccessToken);
            }
        
            public static void SetupClients(TokenCredentials tokenCreds, string subscriptionId)
            {
              _adlaClient = new DataLakeAnalyticsAccountManagementClient(tokenCreds);
              _adlaClient.SubscriptionId = subscriptionId;
        
              _adlaJobClient = new DataLakeAnalyticsJobManagementClient(tokenCreds);
        
              _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClient(tokenCreds);
        
              _adlsClient = new DataLakeStoreAccountManagementClient(tokenCreds);
              _adlsClient.SubscriptionId = subscriptionId;
        
              _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(tokenCreds);
            }
        
            public static void CreateAccounts()
            {
              //ADLS account first, ADLA requires an ADLS account
              var adlsParameters = new DataLakeStoreAccount(location: _location);
              _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);
        
              var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(_adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
              var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: _adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
              var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: _location);
              _adlaClient.Account.Create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }
        
            public static Guid SubmitJobByPath(string scriptPath, string jobName)
            {
              var script = File.ReadAllText(scriptPath);
        
              var jobId = Guid.NewGuid();
              var properties = new USqlJobProperties(script);
              var parameters = new JobInformation(jobName, JobType.USql, properties, priority: 1, degreeOfParallelism: 1, jobId: jobId);
              var jobInfo = _adlaJobClient.Job.Create(_adlaAccountName, jobId, parameters);
        
              return jobId;
            }
        
            public static JobResult WaitForJob(Guid jobId)
            {
              var jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
              while (jobInfo.State != JobState.Ended)
              {
                jobInfo = _adlaJobClient.Job.Get(_adlaAccountName, jobId);
              }
              return jobInfo.Result.Value;
            }
        
            public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
            {
              var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
              var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
              var uploader = new DataLakeStoreUploader(parameters, frontend);
              uploader.Execute();
            }
        
            public static void DownloadFile(string srcPath, string destPath)
            {
              var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
              var fileStream = new FileStream(destPath, FileMode.Create);
        
              stream.CopyTo(fileStream);
              fileStream.Close();
              stream.Close();
            }
          }
        }

6. Prima **F5** para executar a aplicação.

## Consulte também

- Para ver o mesmo tutorial, utilizando outras ferramentas, clique nos seletores de separador no topo da página.
- Para ver uma consulta mais complexa, consulte [Analisar registos de site através da Análise do Azure Data Lake](data-lake-analytics-analyze-weblogs.md).
- Para começar a desenvolver aplicações U-SQL, consulte [Desenvolver scripts SQL-U, utilizando as Ferramentas do Data Lake para Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Para saber mais sobre U-SQL, consulte [Introdução à linguagem U-SQL de Análise do Azure Data Lake](data-lake-analytics-u-sql-get-started.md) e [Referência de linguagem U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).
- Para tarefas de gestão, consulte [Gerir a Análise do Azure Data Lake através do Portal do Azure](data-lake-analytics-manage-use-portal.md).
- Para uma descrição geral da Data Lake Analytics, consulte [Descrição geral da Análise do Azure Data Lake](data-lake-analytics-overview.md).



<!--HONumber=Aug16_HO1-->


