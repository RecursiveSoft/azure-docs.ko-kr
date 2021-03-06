---
title: "백업에서 Azure SQL Database 복원 | Microsoft Docs"
description: "Azure SQL 데이터베이스를 이전 시점(최대 35일)에 롤백할 수 있는 특정 시점 복원에 대해 알아봅니다."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: fd1d334d-a035-4a55-9446-d1cf750d9cf7
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/20/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 751e1eaf65da889ab5c7dc26145c017682a12a4d
ms.lasthandoff: 03/28/2017


---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>자동화된 데이터베이스 백업을 사용하여 Azure SQL 데이터베이스 복구
SQL Database는 [자동화된 데이터베이스 백업](sql-database-automated-backups.md)을 사용하여 데이터베이스 복구를 위한 세 가지 옵션을 제공합니다. 데이터베이스를 백업 [보존 기간](sql-database-service-tiers.md) 동안 데이터베이스를 복원할 수 있습니다.

* 보존 기간 내에 지정된 지점으로 복구된 동일한 논리 서버의 새 데이터베이스 
* 삭제된 데이터베이스에 대한 삭제 시간으로 복구된 동일한 논리 서버의 데이터베이스
* 지역 복제 Blob Storage(RA-GRS)의 최신 매일 백업 지점으로 복구된 지역의 논리 서버에 있는 새 데이터베이스입니다.

> [!TIP]
> 자습서는 [데이터 보호 및 복구를 위한 백업 및 복원 시작](sql-database-get-started-backup-recovery-portal.md)을 참조하세요.
>

[자동화된 데이터베이스 백업](sql-database-automated-backups.md)을 사용하여 모든 지역에 있는 논리 서버에 [데이터베이스 사본](sql-database-copy.md)을 만들 수도 있습니다. 최대 10년 동안 Azure Recovery Services 자격 증명 모음에 Azure SQL Database 백업을 저장하는 SQL Database 서버를 구성할 수도 있습니다. [장기 백업 보존](sql-database-long-term-retention.md)을 참조하세요.

## <a name="recovery-time"></a>복구 시간
자동화된 데이터베이스 백업을 사용하여 데이터베이스를 복원하기 위한 복구 시간은 다음과 같은 다양한 요인의 영향을 받습니다. 

* 데이터베이스의 크기
* 데이터베이스의 성능 수준
* 관련된 트랜잭션 로그의 수
* 복원 지점으로 복구하기 위해 재생해야 하는 작업의 양
* 다른 지역으로 복원되는 경우의 네트워크 대역폭 
* 대상 지역에서 처리되는 동시 복원 요청의 수 
  
  매우 큰 및/또는 활성 데이터베이스의 경우 복원에는 몇 시간이 걸릴 수 있습니다. 지역에서 장시간 가동 중단된 경우 다른 지역에서 많은 수의 지리적 복원 요청이 처리 중일 수 있습니다. 많은 요청이 있는 경우 해당 지역에 데이터베이스의 복구 시간이 늘어날 수 있습니다. 대부분의 데이터베이스는 12시간 내에 완전히 복원됩니다.
  
  대량 복원을 위한 기본 제공 기능은 없습니다. [Azure SQL Database: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) 스크립트는 이 작업을 수행하는 한 가지 방법의 예입니다.

> [!IMPORTANT]
> 자동화된 백업을 사용하여 복구하려면 구독에서 SQL Server 참여자 역할의 구성원이거나 구독 소유자여야 합니다. Azure 포털, PowerShell 또는 REST API를 사용하여 복구할 수 있습니다. Transact-SQL은 사용할 수 없습니다. 
> 

## <a name="point-in-time-restore"></a>지정 시간 복원

[Azure Portal](sql-database-point-in-time-restore-portal.md), [PowerShell](scripts/sql-database-restore-database-powershell.md), 또는 [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx)를 사용하여 동일한 논리 서버에 새 데이터베이스로서 기존 데이터베이스를 이전 시점으로 복원할 수 있습니다. 

> [!IMPORTANT]
> 복원하는 동안 기존 데이터베이스를 덮어쓸 수는 없습니다.
>

모든 서비스 계층 또는 성능 수준으로, 단일 데이터베이스로서 또는 탄력적 풀에 데이터베이스를 복원할 수 있습니다. 논리 서버 또는 데이터베이스를 복원하는 탄력적 풀에 충분한 리소스가 있는지 확인합니다. 완료되면 복원된 데이터베이스는 일반적이고 완벽하게 액세스할 수 있는 온라인 데이터베이스입니다. 복원된 데이터베이스는 서비스 계층 및 성능 수준에 따라 정상 요금이 청구됩니다. 데이터베이스 복원이 완료될 때까지 요금이 발생하지 않습니다.

일반적으로 복구를 위해 이전 지점까지 데이터베이스를 복원합니다. 이렇게 하는 경우 원본 데이터베이스에 대한 대체로 복원된 데이터베이스를 처리하거나 데이터를 검색하고 원래 데이터베이스를 업데이트하는 데 사용할 수 있습니다. 

* ***데이터베이스 바꾸기:*** 복원된 데이터베이스를 원래 데이터베이스에 대한 대체로 여기는 경우 성능 수준 및 서비스 계층이 적절한지 확인하고 필요한 경우 데이터베이스의 크기를 조정해야 합니다. 원본 데이터베이스의 이름을 바꿀 수 있으며 T-SQL에서 ALTER DATABASE 명령을 사용하여 복원된 데이터베이스에 원래 이름을 제공할 수 있습니다. 
* ***데이터 복구:*** 복원된 데이터베이스에서 데이터를 가져와 사용자 또는 응용 프로그램 오류로부터 복구하려면 복원된 데이터베이스에서 원본 데이터베이스로 데이터를 추출하는 데 필요한 데이터 복구 스크립트를 작성하고 실행해야 합니다. 복원 작업을 완료하는 데 긴 시간이 걸리지만 복원 중인 데이터베이스가 복원 과정 내내 데이터베이스 목록에 표시됩니다. 복원하는 동안 데이터베이스를 삭제하는 경우 작업이 취소되고 복원이 완료되지 않은 데이터베이스에 대해서는 비용이 청구되지 않습니다. 

## <a name="deleted-database-restore"></a>삭제된 데이터베이스 복원
삭제된 데이터베이스 복원을 사용하면 [Azure Portal](sql-database-restore-deleted-database-portal.md), [PowerShell](scripts/sql-database-restore-database-powershell.md), 또는 [REST(createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)를 사용하여 삭제된 데이터베이스를 동일한 논리 서버의 삭제된 데이터베이스에 대한 삭제 시간으로 복원할 수 있습니다. 

> [!IMPORTANT]
> Azure SQL Database 서버 인스턴스를 삭제하면 모든 해당 데이터베이스도 삭제되고 복구할 수 없습니다. 현재는 삭제된 서버 복원에 대한 지원이 제공되지 않습니다.
> 

## <a name="geo-restore"></a>지역 복원
가장 최근의 지리적 복제 전체 및 차등 백업에서 Azure 지역에 있는 모든 서버의 SQL Database를 복원할 수 있습니다. 지리적 복원은 지역 중복 백업을 해당 원본으로 사용하고 가동 중단으로 인해 데이터베이스 또는 데이터 센터에 액세스할 수 없는 경우에도 데이터베이스를 복구하는 데 사용될 수 있습니다. [Azure Portal](sql-database-geo-restore-portal.md), [PowerShell](sql-database-geo-restore-powershell.md) 또는 [REST (createMode=Recovery)](https://msdn.microsoft.com/library/azure/mt163685.aspx)를 사용할 수 있습니다. 

지리적 복원은 데이터베이스가 호스팅되는 지역에 사고가 발생하여 데이터베이스를 사용할 수 없게 되었을 때를 위한 기본 복구 옵션입니다. 지역에서 대규모 인시던트로 인해 데이터베이스 응용 프로그램을 사용할 수 없게 되면 지역에서 복제된 백업에서 다른 지역에 있는 서버로 데이터베이스를 복원할 수 있습니다. 차등 백업을 만들 때와 다른 지역에 있는Azure Blob에 지역에서 복제될 때 사이에 지연이 있습니다. 재해가 발생한 경우 최대 1시간 동안의 데이터가 손실되므로 이 지연은 최대 1시간일 수 있습니다. 다음 그림에서는 다른 지역에서 마지막으로 사용할 수 있는 백업에서 데이터베이스의 복원을 보여 줍니다.

![geo-restore](./media/sql-database-geo-restore/geo-restore-2.png)

지역 복원 기능을 사용하여 가동 중단에서 복구하는 방법에 대한 자세한 내용은 [가동 중단에서 복구](sql-database-disaster-recovery.md)

> [!IMPORTANT]
> 백업에서 복원은 RPO와 ERT(예상 복구 시간)가 가장 긴 SQL Database에서 사용할 수 있는 재해 복구 솔루션 중 가장 기본적인 기능입니다. 최대 크기 2GB인 기본 데이터베이스의 경우 지리적 복원은 ERT가 12시간인 적절한 DR 솔루션을 제공합니다. 대규모 표준 또는 프리미엄 데이터베이스의 경우 복구 시간을 줄이거나 데이터 손실 가능성을 줄이려면 활성 지역 복제를 사용하도록 고려해야 합니다. 활성 지역 복제는 지속적으로 복제된 보조 데이터베이스에 대한 장애 조치를 시작하도록 하여 훨씬 더 낮은 RPO와 ERT를 제공합니다. 자세한 내용은 [활성 지역 복제](sql-database-geo-replication-overview.md)를 참조하세요.
> 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>자동화된 백업을 사용하여 프로그래밍 방식으로 복구 수행
앞서 설명한 것처럼 Azure Portal 외에, Azure PowerShell 또는 REST API를 사용하여 데이터베이스 복구를 프로그래밍 방식으로 수행할 수 있습니다. 다음 표는 사용 가능한 명령의 집합을 보여 줍니다.

### <a name="powershell"></a>PowerShell
| Cmdlet | 설명 |
| --- | --- |
| [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648.aspx) |하나 이상의 데이터베이스를 가져옵니다. |
| [Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387.aspx) |복원할 수 있는 삭제된 데이터베이스를 가져옵니다. |
| [Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx) |데이터베이스의 지역 중복 백업을 가져옵니다. |
| [Restore-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx) |SQL 데이터베이스를 복원합니다. |
|  | |

### <a name="rest-api"></a>REST API
| API | 설명 |
| --- | --- |
| [REST(createMode=Recovery)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |데이터베이스를 복원합니다. |
| [데이터베이스 만들기 또는 업데이트 상태 가져오기](https://msdn.microsoft.com/library/azure/mt643934.aspx) |복원 작업 동안 상태를 반환합니다. |
|  | |

## <a name="summary"></a>요약
자동 백업은 사용자 및 응용 프로그램 오류, 우발적인 데이터베이스 삭제 및 장시간의 가동 중단에서 데이터베이스를 보호합니다. 이 기본 제공 기능은 모든 서비스 계층 및 성능 수준에 사용할 수 있습니다. 

## <a name="next-steps"></a>다음 단계
* 비즈니스 연속성의 개요 및 시나리오를 보려면 [비즈니스 연속성 개요](sql-database-business-continuity.md)
* Azure SQL 데이터베이스 자동화 백업에 대한 자세한 내용은 [SQL 데이터베이스 자동화 백업](sql-database-automated-backups.md)
* 장기 백업 보존에 대해 알아보려면 [장기 백업 보존](sql-database-long-term-retention.md)을 참조하세요.
* Azure Portal을 사용하여 Azure Recovery Services에서 자동화된 백업을 장기 보존에서 구성, 관리 및 복원하려면 [Azure Portal을 사용하여 장기 백업 보존 관리](sql-database-manage-long-term-backup-retention-portal.md)를 참조하세요. 
* PowerShell을 사용하여 Azure Recovery Services에서 자동화된 백업을 장기 보존에서 구성, 관리 및 복원하려면 [PowerShell을 사용하여 장기 백업 보존 관리](sql-database-manage-long-term-backup-retention-powershell.md)를 참조하세요.
* 빠른 복구 옵션에 대해 알아보려면 [활성 지역 복제](sql-database-geo-replication-overview.md)  
* 보관을 위해 자동화된 백업을 사용하는 방법을 알아보려면 [데이터베이스 복사](sql-database-copy.md)

