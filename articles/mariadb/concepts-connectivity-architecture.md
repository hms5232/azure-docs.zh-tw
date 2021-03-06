---
title: 連線性架構-適用於 MariaDB 的 Azure 資料庫
description: 描述適用於 MariaDB 的 Azure 資料庫伺服器的連接架構。
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/8/2020
ms.openlocfilehash: c3f557c757a46252b9fa0416cc62a827b233f1b2
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2020
ms.locfileid: "88065347"
---
# <a name="connectivity-architecture-in-azure-database-for-mariadb"></a>適用於 MariaDB 的 Azure 資料庫中的連線架構
本文說明適用於 MariaDB 的 Azure 資料庫連線架構，以及如何從 Azure 內部和外部的用戶端，將流量導向至您的適用於 MariaDB 的 Azure 資料庫實例。

## <a name="connectivity-architecture"></a>連線架構

您的適用於 MariaDB 的 Azure 資料庫的連線是透過負責將連入連線路由至叢集中伺服器實體位置的閘道所建立。 下圖說明流量的流程。

![連線性架構的總覽](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

當用戶端連接到資料庫時，它們會取得連接到閘道的連接字串。 此閘道的公用 IP 位址會接聽埠3306。 在資料庫叢集中，會將流量轉送到適當的適用於 MariaDB 的 Azure 資料庫。 因此，為了連接到您的伺服器（例如從公司網路），必須開啟用戶端防火牆，以允許輸出流量能夠連線到我們的閘道。 您可以在下方找到每個區域的閘道所使用的 IP 位址的完整清單。

## <a name="azure-database-for-mariadb-gateway-ip-addresses"></a>適用於 MariaDB 的 Azure 資料庫閘道 IP 位址

下表列出所有資料區域之適用於 MariaDB 的 Azure 資料庫閘道的主要和次要 Ip。 主要 IP 位址是閘道目前的 IP 位址，而第二個 IP 位址是容錯移轉的 IP 位址（萬一主要複本失敗時）。 如前所述，客戶應允許輸出到這兩個 IP 位址。 第二個 IP 位址不會在任何服務上接聽，直到適用於 MariaDB 的 Azure 資料庫啟用以接受連線為止。

| **區功能變數名稱稱** | **閘道 IP 位址** |
|:----------------|:-------------|
| 澳大利亞中部| 20.36.105.0     |
| 澳大利亞 Central2     | 20.36.113.0   |
| 澳大利亞東部 | 13.75.149.87, 40.79.161.1     |
| 澳大利亞東南部 |191.239.192.109, 13.73.109.251   |
| 巴西南部 | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| 加拿大中部 |40.85.224.249  |
| 加拿大東部 | 40.86.226.166    |
| 美國中部 | 23.99.160.139, 13.67.215.62, 52.182.136.37, 52.182.136.38     |
| 中國東部 | 139.219.130.35    |
| 中國東部 2 | 40.73.82.1  |
| 中國北部 | 139.219.15.17    |
| 中國北部 2 | 40.73.50.0     |
| 東亞 | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| 美國東部 | 40.121.158.30, 191.238.6.43, 40.71.8.203, 40.71.83.113   |
| 美國東部 2 |40.79.84.180, 191.239.224.107, 52.177.185.181, 40.70.144.38, 52.167.105.38  |
| 法國中部 | 40.79.137.0, 40.79.129.1  |
| 法國南部 | 40.79.177.0     |
| 德國中部 | 51.4.144.100     |
| 德國東北部 | 51.5.144.179  |
| 印度中部 | 104.211.96.159     |
| 印度南部 | 104.211.224.146  |
| 印度西部 | 104.211.160.80    |
| 日本東部 | 13.78.61.196, 191.237.240.43  |
| 日本西部 | 104.214.148.156, 191.238.68.11, 40.74.96.6, 40.74.96.7    |
| 南韓中部 | 52.231.32.42   |
| 南韓南部 | 52.231.200.86    |
| 美國中北部 | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| 歐洲北部 | 40.113.93.91, 191.235.193.75, 52.138.224.6, 52.138.224.7    |
| 南非北部  | 102.133.152.0    |
| 南非西部 | 102.133.24.0   |
| 美國中南部 |13.66.62.124, 23.98.162.75, 104.214.16.39, 20.45.120.0   |
| 東南亞 | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| 阿拉伯聯合大公國中部 | 20.37.72.64  |
| 阿拉伯聯合大公國北部 | 65.52.248.0    |
| 英國南部 | 51.140.184.11   |
| 英國西部 | 51.141.8.11  |
| 美國中西部 | 13.78.145.25     |
| 西歐 | 40.68.37.158, 191.237.232.75, 13.69.105.208, 104.40.169.187  |
| 美國西部 | 104.42.238.205, 23.99.34.75, 13.86.216.212, 13.86.217.212 |
| 美國西部 2 | 13.66.226.202  |
||||

## <a name="connection-redirection"></a>連接重新導向

適用於 MariaDB 的 Azure 資料庫支援額外的連線原則「重新導向 **」，協助**減少用戶端應用程式與適用于 mariadb 伺服器之間的網路延遲。 使用這項功能，在適用於 MariaDB 的 Azure 資料庫伺服器建立初始 TCP 會話之後，伺服器會將裝載適用于 mariadb 伺服器之節點的後端位址傳回給用戶端。 之後，所有後續封包都會直接流向伺服器，略過閘道。 當封包直接流向伺服器時，延遲和輸送量會提升效能。

具有引擎版本10.2 和10.3 的適用於 MariaDB 的 Azure 資料庫伺服器支援這項功能。

PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure)延伸模組提供重新導向的支援，由 Microsoft 所開發，並可在[PECL](https://pecl.php.net/package/mysqlnd_azure)上取得。 如需如何在您的應用程式中使用重新導向的詳細資訊，請參閱設定重新導向[一文。](./howto-redirection.md)

> [!IMPORTANT]
> PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) \(英文\) 延伸模組中的重新導向支援目前為預覽狀態。

## <a name="next-steps"></a>後續步驟

* [使用 Azure 入口網站建立和管理適用於 MariaDB 的 Azure 資料庫防火牆規則](./howto-manage-firewall-portal.md)
* [使用 Azure CLI 建立和管理適用於 MariaDB 的 Azure 資料庫防火牆規則](./howto-manage-firewall-cli.md)