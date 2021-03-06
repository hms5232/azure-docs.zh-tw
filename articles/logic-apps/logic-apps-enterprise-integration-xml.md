---
title: XML 訊息和一般檔案
description: 使用企業整合套件處理、驗證和轉換 Azure Logic Apps 中的 XML 訊息
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: 3cb3472572abbd891f8d36ea036b371c1224f38c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "74792144"
---
# <a name="xml-messages-and-flat-files-in-azure-logic-apps-with-enterprise-integration-pack"></a>採用 Enterprise Integration Pack 的 Azure Logic Apps 中的 X12 訊息和一般檔案

在[Azure Logic Apps](logic-apps-overview.md)中，您可以使用企業整合套件來處理您傳送和接收的 XML 訊息。 如果您已使用 BizTalk Server，企業整合套件會提供類似的功能來轉換和驗證訊息、使用一般檔案，甚至使用 XPath 從訊息擴充或解壓縮特定屬性。 如果您不熟悉此空間，這些功能會擴充您在邏輯應用程式的工作流程中處理訊息的方式。 例如，如果您有企業對企業（B2B）案例，並使用特定的 XML 架構，您可以使用企業整合套件來增強公司處理這些訊息的方式。

例如，企業整合套件包含下列功能：

* [Xml 驗證](logic-apps-enterprise-integration-xml-validation.md)：根據特定的架構驗證傳入或傳出的 XML 訊息。

* [Xml 轉換](logic-apps-enterprise-integration-transform.md)：使用對應依據您的需求或夥伴的需求，轉換或自訂 xml 訊息。

* 一般檔案[編碼和](logic-apps-enterprise-integration-flatfile.md)一般檔案解碼：對一般檔案進行編碼或解碼。

  例如，SAP 接受並以一般檔案格式傳送 IDOC 檔案。 許多整合平台會建立 XML 訊息，包括邏輯應用程式。 因此，您可以建立使用一般檔案編碼器將 XML 檔案「轉換」為一般檔案的邏輯應用程式。

* [XPath](workflow-definition-language-functions-reference.md#xpath)：擴充訊息，並從訊息中解壓縮特定屬性。 然後您可以使用擷取的屬性將訊息傳送至目的地或中繼端點。

## <a name="sample"></a>範例

使用 Azure Logic Apps 中的 XML 功能，[部署完整運作的邏輯應用程式](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-veter-pipeline)（GitHub 範例）。

## <a name="next-steps"></a>後續步驟

深入瞭解[企業整合套件](logic-apps-enterprise-integration-overview.md)
