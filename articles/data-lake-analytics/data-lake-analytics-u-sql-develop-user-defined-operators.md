---
title: 開發 U-SQL 使用者定義的運算子-Azure Data Lake Analytics
description: 了解如何開發使用者定義的運算子，以用於和重複用於 Azure Data Lake Analytics 作業中。
services: data-lake-analytics
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.topic: how-to
ms.date: 12/05/2016
ms.openlocfilehash: 71cb6e026c81b00e475ae3f4e7fc4958a8de82a3
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87131798"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>開發 U-SQL 使用者定義的運算子 (UDO)
本文說明如何開發使用者定義的運算子，來處理 U-SQL 作業中的資料。

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>在 U-SQL 中定義和使用使用者定義的運算子

### <a name="to-create-and-submit-a-u-sql-job"></a>建立和提交 U-SQL 工作

1. 從 Visual Studio 中，選取 [檔案] > [新增] > [專案] > [U-SQL 專案]****。
2. 按一下 [確定]  。 Visual Studio 會建立具有 Script.usql 檔案的解決方案。
3. 在 [方案總管]**** 中展開 Script.usql，然後按兩下 **Script.usql.cs**。
4. 將下列程式碼貼到檔案中：

   ```usql
   using Microsoft.Analytics.Interfaces;
   using System.Collections.Generic;
   namespace USQL_UDO
   {
       public class CountryName : IProcessor
       {
           private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
           {
               {
                   "Deutschland", "Germany"
               },
               {
                   "Suisse", "Switzerland"
               },
               {
                   "UK", "United Kingdom"
               },
               {
                   "USA", "United States of America"
               },
               {
                   "中国", "PR China"
               }
           };
           public override IRow Process(IRow input, IUpdatableRow output)
           {
               string UserID = input.Get<string>("UserID");
               string Name = input.Get<string>("Name");
               string Address = input.Get<string>("Address");
               string City = input.Get<string>("City");
               string State = input.Get<string>("State");
               string PostalCode = input.Get<string>("PostalCode");
               string Country = input.Get<string>("Country");
               string Phone = input.Get<string>("Phone");
               if (CountryTranslation.Keys.Contains(Country))
               {
                   Country = CountryTranslation[Country];
               }
               output.Set<string>(0, UserID);
               output.Set<string>(1, Name);
               output.Set<string>(2, Address);
               output.Set<string>(3, City);
               output.Set<string>(4, State);
               output.Set<string>(5, PostalCode);
               output.Set<string>(6, Country);
               output.Set<string>(7, Phone);
               return output.AsReadOnly();
           }
       }
   }
   ```

5. 開啟 **Script.usql**，並貼上下列 U-SQL 指令碼：

   ```usql
   @drivers =
       EXTRACT UserID      string,
               Name        string,
               Address     string,
               City        string,
               State       string,
               PostalCode  string,
               Country     string,
               Phone       string
       FROM "/Samples/Data/AmbulanceData/Drivers.txt"
       USING Extractors.Tsv(Encoding.Unicode);

   @drivers_CountryName =
       PROCESS @drivers
       PRODUCE UserID string,
               Name string,
               Address string,
               City string,
               State string,
               PostalCode string,
               Country string,
               Phone string
       USING new USQL_UDO.CountryName();

   OUTPUT @drivers_CountryName
       TO "/Samples/Outputs/Drivers.csv"
       USING Outputters.Csv(Encoding.Unicode);
   ```

6. 指定 Data Lake Analytics 帳戶、資料庫和結構描述。
7. 從 [方案總管]**** 中，在 [Script.usql]**** 上按一下滑鼠右鍵，然後按一下 [建置指令碼]****。
8. 從 [方案總管]**** 中，在 [Script.usql]**** 上按一下滑鼠右鍵，然後按一下 [提交指令碼]****。
9. 如果您尚未連線至 Azure 訂用帳戶，系統會提示您輸入 Azure 帳戶認證。
10. 按一下 [提交] 。 提交作業完成時，提交結果和工作連結都可以在 [結果] 視窗中取得。
11. 按一下 [重新整理]**** 按鈕，以查看最新的作業狀態並重新整理畫面。

### <a name="to-see-the-output"></a>查看輸出

1. 從 [伺服器總管]**** 依序展開 [Azure]****、[Data Lake Analytics]****、您的 Data Lake Analytics 帳戶、[儲存體帳戶]****，以滑鼠右鍵按一下 [預設儲存體]，然後按一下 [總管]****。

2. 展開範例、展開輸出，然後按兩下 [Drivers.csv] ****。

## <a name="next-steps"></a>後續步驟

* [使用使用者程式碼延伸 U-SQL 運算式](/u-sql/concepts/extending-u-sql-expressions-with-user-code)
* [使用適用於 Visual Studio 的 Data Lake 工具來開發 U-SQL 應用程式](data-lake-analytics-data-lake-tools-get-started.md)
