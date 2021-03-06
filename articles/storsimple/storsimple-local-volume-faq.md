---
title: "StorSimple 로컬로 고정된 볼륨 FAQ | Microsoft Docs"
description: "StorSimple 로컬로 고정된 볼륨에 대한 질문과 대답에 대한 답변을 제공합니다."
services: storsimple
documentationcenter: NA
author: manuaery
manager: syadav
editor: 
ms.assetid: 7a2f4b1a-4ac9-4ce1-9359-bd40a9cbbb5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/11/2017
ms.author: manuaery
translationtype: Human Translation
ms.sourcegitcommit: a437ef2ba20970e2dd9a06fbc9e39fff2bb82da7
ms.openlocfilehash: 22eb95bf0e3a20893acbb860ad10cfe6a3bcf088
ms.lasthandoff: 01/12/2017


---
# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>StorSimple 로컬로 고정된 볼륨: 질문과 대답(FAQ)
## <a name="overview"></a>개요
다음은 StorSimple 로컬로 고정된 볼륨을 만들거나, 계층화된 볼륨을 로컬로 고정된 볼륨으로 변환(또는 그 반대로)하거나 로컬로 고정된 볼륨을 백업 및 복원하는 경우 발생할 수 있는 질문과 대답입니다.

질문과 대답은 다음과 같은 범주로 정렬됩니다

* 로컬로 고정된 볼륨 만들기
* 로컬 고정된 백업
* 계층화된 볼륨을 로컬로 고정된 볼륨으로 변환
* 로컬로 고정된 볼륨 복원
* 로컬로 고정된 볼륨 장애 조치

## <a name="questions-about-creating-a-locally-pinned-volume"></a>로컬로 고정된 볼륨 만들기에 대한 질문
**Q.** 8000 시리즈 장치에 만들 수 있는 로컬로 고정된 볼륨의 최대 크기는?

**A** StorSimple 8000 시리즈 업데이트 3.0을 실행 중인 장치에서 로컬로 고정된 볼륨은 최대 8.5TB, 계층화된 볼륨은 최대 200TB까지 프로비전할 수 있습니다. 더 큰 8600 장치에서 로컬로 고정된 볼륨은 최대 22.5TB, 계층화된 볼륨은 최대 500TB까지 프로비전할 수 있습니다.    
StorSimple 8000 시리즈 업데이트 2.x를 실행 중인 장치에서 로컬로 고정된 볼륨은 최대 8TB, 계층화된 볼륨은 최대 200TB까지 프로비전할 수 있습니다. 더 큰 8600 장치에서는 최대 20TB의 로컬로 고정된 볼륨 또는 최대 500TB의 계층화된 볼륨을 프로비전할 수 있습니다.   

**Q.** 최근에 8100 장치를 업데이트 2.0으로 업그레이드했고 로컬로 고정된 볼륨을 만들 때 사용할 수 있는 최대 크기는 6TB이며 8TB가 아닙니다. 8TB 볼륨을 만들 수 없는 이유는?

**A** 업데이트 2.0을 실행 중인 8100 장치에서 로컬로 고정된 볼륨은 최대 8TB, 계층화된 볼륨은 최대 200TB까지 프로비전할 수 있습니다. 장치에 계층화된 볼륨이 있는 경우 로컬로 고정된 볼륨을 만들기 위해 사용 가능한 공간은 최대 한도보다 비례적으로 낮을 수 있습니다. 예를 들어 (계층화된 용량의 절반인) 8100 장치에 계층화된 볼륨의 100TB가 이미 프로비전된 경우 8100 장치에서 만들 수 있는 로컬 볼륨의 최대 크기는 4TB로 줄어듭니다(로컬로 고정된 볼륨의 최대 용량에서 대략 절반).

장치에서 일부 로컬 공간이 계층화된 볼륨의 작업 집합을 호스트하는 데 사용되기 때문에 장치가 볼륨을 계층화하는 경우 로컬로 고정된 볼륨을 만들기 위해 사용 가능한 공간은 줄어듭니다. 반대로, 로컬로 고정된 볼륨을 만드는 작업은 계층화된 볼륨에 사용 가능한 공간을 비례적으로 감소시킵니다. 다음 테이블에서는 로컬로 고정된 볼륨이 만들어질 때 8100 및 8600 장치에서 사용 가능한 계층화된 용량을 요약합니다.

####<a name="update-30"></a>업데이트 3.0 
| 로컬로 고정된 볼륨이 프로비전된 용량 | 계층화  볼륨에 프로비전된 사용 가능한 용량 - 8100 | 계층화  볼륨에 프로비전된 사용 가능한 용량 - 8600 |
| --- | --- | --- |
| 0 |200TB |500TB |
| 1TB |176.5TB |477.8TB |
| 4TB |105.9TB |411.1TB |
| 8.5TB |0TB |311.1TB |
| 10TB |해당 없음 |277.8TB |
| 15TB |해당 없음 |166.7TB |
| 22.5TB |해당 없음 |0TB |

####<a name="update-2x"></a>업데이트 2.x  
 | 로컬로 고정된 볼륨이 프로비전된 용량 | 계층화  볼륨에 프로비전된 사용 가능한 용량 - 8100 | 계층화  볼륨에 프로비전된 사용 가능한 용량 - 8600 |  
 | --- | --- | --- |  
 | 0 |200TB |500TB |  
 | 1TB |25TB |475TB |  
 | 4TB |100TB |400TB |  
 | 8TB |0TB |300TB |  
 | 10TB |해당 없음 |250TB |  
 | 15TB |해당 없음 |125TB |  
 | 20TB |해당 없음 |0TB |   

**Q.** 로컬로 고정된 볼륨 만들기가 장기 실행 작업인 이유는? 

**A.** 로컬로 고정된 볼륨은 씩 프로비전됩니다. 장치의 로컬 계층에 공간을 만들려면 프로비전 프로세스 동안 기존의 계층화된 볼륨의 일부 데이터가 클라우드로 푸시될 수 있습니다. 프로비전된 볼륨의 크기, 장치의 기존 데이터 및 클라우드에 대한 사용 가능한 대역폭에 따라 로컬 볼륨을 만드는 데 몇 시간이 걸릴 수 있습니다.

**Q.** 로컬로 고정된 볼륨을 만드는 데 걸리는 시간은?

**A.** 로컬로 고정된 볼륨이 씩 프로비전되었기 때문에 프로비전 프로세스 동안 계층화된 볼륨에서 일부 기존 데이터가 클라우드로 푸시될 수 있습니다. 따라서 로컬로 고정된 볼륨을 만드는 데 걸리는 시간은 볼륨의 크기, 장치의 데이터 및 사용 가능한 대역폭을 비롯한 여러 요인에 따라 다릅니다. 볼륨이 없는 새로 설치된 장치에서 로컬로 고정된 볼륨을 만드는 데 걸리는 시간은 데이터의 테라바이트 당 10분 정도입니다. 그러나 로컬 볼륨을 만드는 작업은 사용 중인 장치에 있는 위에서 설명한 요인에 따라 몇 시간이 걸릴 수 있습니다.

**Q.** 로컬로 고정된 볼륨을 만들려고 합니다. 유의해야 하는 모범 사례는?

**A.** 로컬로 고정된 볼륨은 항상 데이터의 로컬 보증을 요구하며 클라우드 대기 시간에 민감한 워크로드에 적합합니다. 워크로드에 로컬 볼륨의 사용을 고려하는 동안 다음을 주의합니다.

* 로컬로 고정된 볼륨은 씩 프로비전되고 로컬 볼륨을 만드는 작업은 계층화된 볼륨에 대해 사용 가능한 공간에 영향을 줍니다. 따라서 더 작은 크기의 볼륨을 시작하고 저장소 요구 사항이 증가하면 확장하는 것이 좋습니다.
* 로컬 볼륨의 프로비전은 계층화된 볼륨에서 클라우드에 기존 데이터의 푸시를 포함할 수 있는 장기 실행 작업입니다. 결과적으로, 이러한 볼륨의 성능이 저하될 수 있습니다.
* 로컬 볼륨의 프로비전은 시간이 오래 걸리는 작업입니다. 관련된 실제 시간은 프로비전된 볼륨의 크기, 장치의 데이터 및 사용 가능한 대역폭 등 여러 요소에 따라 다릅니다. 클라우드로 기존 볼륨을 백업하지 않았다면 볼륨을 만드는 속도가 느립니다. 로컬 볼륨을 프로비전하기 전에 기존 볼륨의 클라우드 스냅숏을 만드는 것이 좋습니다.
* 기존의 계층화된 볼륨을 로컬로 고정된 볼륨으로 변환할 수 있으며 이 변환은 (계층화된 데이터[있다면]를 클라우드에서 가져오는 것 외에도) 로컬로 고정된 볼륨을 만드는 장치에서 공간의 프로비전을 포함합니다. 다시, 위에서 설명한 요인에 따라 결정되는 장기 실행 작업입니다. 기존 볼륨을 백업되지 않은 경우 프로세스가 더 느리기 때문에 변환하기 전에 기존 볼륨을 백업하는 것이 좋습니다. 또한 이 과정에서 장치의 성능이 감소할 수 있습니다.

[로컬로 고정된 볼륨을 만드는](storsimple-manage-volumes-u2.md#add-a-volume)

**Q.** 동시에 여러 개의 로컬로 고정된 볼륨을 만들 수 있나요?

**A.** 예, 하지만 로컬로 고정된 볼륨 만들기 및 확장 작업은 순차적으로 처리됩니다.

로컬로 고정된 볼륨은 씩 프로비전되고 이를 위해서는 장치에서 로컬 공간을 만들어야 합니다(프로비전 프로세스 동안 클라우드로 푸시할 계층화된 볼륨의 기존 데이터로 발생할 수 있음). 따라서 프로비전 작업이 진행 중이면 해당 작업이 완료될 때까지 다른 로컬 볼륨 만들기 작업은 큐에 대기됩니다.

마찬가지로 기존 로컬 볼륨을 확장하거나 계층화된 볼륨을 로컬로 고정된 볼륨으로 변환하는 경우 이전 작업이 완료될 때까지 새 로컬로 고정된 볼륨을 만드는 작업은 큐에 대기됩니다. 로컬로 고정된 볼륨의 크기를 확장하는 작업은 해당 볼륨에 대한 기존 로컬 공간의 확장을 포함합니다. 또한 계층화된 볼륨에서 로컬로 고정된 볼륨으로 변환하는 작업은 만들어진 로컬로 고정된 볼륨에 대한 로컬 공간을 만듭니다. 이러한 작업 모두에서 로컬 공간의 만들기 또는 확장은 장기 실행 작업입니다.

Azure StorSimple Manager 서비스의 **작업** 페이지에서 이러한 작업을 볼 수 있습니다. 적극적으로 처리되고 있는 작업은 공간 프로비전의 진행 상태를 반영하도록 지속적으로 업데이트됩니다. 남아있는 로컬로 고정된 볼륨 작업은 실행 중으로 표시되지만 진행은 중단되고 큐에 대기된 순서 대로 선택됩니다.

**Q.** 로컬로 고정된 볼륨을 삭제했습니다. 새 볼륨을 만들려고 할 때 사용 가능한 공간에 반영되는 회수된 공간이 표시되지 않는 이유는? 

**A.** 로컬에 고정된 볼륨을 삭제하면 새 볼륨에 사용할 수 있는 공간이 즉시 업데이트되지 않을 수 있습니다. StorSimple Manager 서비스는 사용 가능한 로컬 공간을 거의 매 시간마다 업데이트합니다. 새 볼륨을 만들기 전에 한 시간을 기다려 주세요.

**Q.** 클라우드 장비에 로컬로 고정된 볼륨이 지원되나요?

**A.** 로컬로 고정된 볼륨은 클라우드 장비(원래 StorSimple 가상 장치로 알려진 8010 및 8020 장치)에서 지원되지 않습니다.

**Q.** Azure PowerShell cmdlet을 사용하여 로컬로 고정된 볼륨을 만들고 관리할 수 있나요? 

**A.** 아니요, Azure PowerShell cmdlet을 통해 로컬로 고정된 볼륨을 만들 수 없습니다(Azure PowerShell을 통해 만든 볼륨은 계층화됨). 계층화된 볼륨 형식을 수정하는 원하지 않는 영향을 미치기 때문에 Azure PowerShell cmdlet를 사용하지 않아서 로컬로 고정된 볼륨의 속성을 수정하는 것이 좋습니다.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>로컬로 고정된 볼륨 백업에 대한 질문
**Q.** 로컬로 고정된 볼륨의 로컬 스냅숏이 지원되나요?

**A.** 예, 로컬로 고정된 볼륨의 로컬 스냅숏을 사용할 수 있습니다. 그러나 클라우드 스냅숏으로 로컬로 고정된 볼륨을 정기적으로 백업하하여 데이터가 재해에 대비하여 보호되도록 하는 것이 좋습니다.

**Q.** 로컬로 고정된 볼륨에서 로컬 스냅숏을 관리하기 위한 지침이 있나요?

**A.** 로컬로 고정된 볼륨에서 높은 비율의 데이터 이탈이 일어나는 빈번한 로컬 스냅숏으로 인해 장치의 로컬 공간을 빠르게 소모시키고 계층화된 볼륨의 데이터를 클라우드로 푸쉬하는 결과를 일으킬 수 있습니다. 따라서 로컬 스냅숏의 수를 최소화하는 것이 좋습니다.

**Q.** 로컬로 고정 볼륨의 내 로컬 스냅숏이 무효화될 수 있다는 경고가 나타났습니다. 언제 발생할 수 있나요?

**A.** 이 백업 정책 그룹에 연결된 볼륨에서 높은 비율의 데이터 이탈과 함께 빈번한 로컬 스냅숏으로 인해 장치의 로컬 공간을 신속하게 사용될 수 있습니다. 장치의 로컬 계층이 자주 사용되는 경우 확장된 클라우드 중단이 가득 찬 장치에 나타날 수 있습니다. 그리고 볼륨에 들어오는 쓰기는 스냅숏을 무효화할 수 있습니다(덮어쓴 데이터의 이전 블록을 참조하기 위해 스냅숏을 업데이트할 공간이 없음). 이러한 상황에서 볼륨에 대한 쓰기는 계속 제공될 수 있지만 로컬 스냅숏은 무효화될 수 있습니다. 기존 클라우드 스냅숏에 영향은 없습니다.

알림 경고는 빈도가 낮은 로컬 스냅숏을 가져오는 로컬 스냅숏 일정을 검토하거나 더 이상 필요하지 않는 이전의 로컬 스냅숏을 삭제하여 이런 상황이 발생할 수 있고 동일한 상황을 적시에 해결할 것을 알립니다.

로컬 스냅숏이 무효화되면 특정 백업 정책에 대한 로컬 스냅숏이 무효화된 로컬 스냅숏의 타임스탬프와 함께 무효화된 것을 알려주는 정보 알림을 받게 됩니다. 이러한 스냅숏은 자동으로 삭제되고 Azure 클래식 포털의 **백업 카탈로그** 페이지 더 이상 볼 수 없습니다.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>계층화된 볼륨을 로컬로 고정된 볼륨으로 변환하는 방법에 대한 질문
**Q.** 계층화된 볼륨을 로컬로 고정된 볼륨으로 변환하는 동안 장치에 속도 저하가 확인됩니다. 이것이 발생하는 이유는? 

**A.** 변환 프로세스는 다음의 두 단계로 이루어집니다. 

1. 곧 변환될 로컬로 고정된 볼륨에 대한 장치의 공간의 프로비전합니다.
2. 로컬 보증을 확실히 하도록 클라우드에서 계층화된 데이터를 다운로드합니다.

이 단계는 모두 변환 중인 볼륨의 크기, 장치의 데이터 및 사용 가능한 대역폭에 따라 달라지는 장기 실행 작업입니다. 기존의 계층화된 볼륨의 일부 데이터는 프로비전 프로세스의 일환으로 클라우드에 유출될 수 있으므로 이 시간 동안 장치의 성능이 저하될 수 있습니다. 또한 다음의 경우 변환 프로세스의 속도가 느려질 수 있습니다.

* 기존 볼륨을 클라우드에 백업하지 않았으므로 변환을 시작하기 전에 볼륨을 백업하는 것이 좋습니다.
* 대역폭 제한 정책을 적용했으며 이는 클라우드에 사용 가능한 대역폭을 제한할 수 있습니다. 따라서 클라우드에 대해 전용으로 사용되는 40 Mbps 이상의 연결이 있는 것이 좋습니다.
* 변환 프로세스는 위에서 설명한 여러 요인으로 인해 몇 시간이 걸립니다. 따라서 급증하는 동안이나 주말에 이 작업을 수행하여 최종 소비자에 미치는 영향을 방지하는 것이 좋습니다.

[계층화된 볼륨을 로컬로 고정된 볼륨으로 변환](storsimple-manage-volumes-u2.md#change-the-volume-type)

**Q.** 볼륨 변환 작업을 취소할 수 있나요?

**A.** 아니요, 변환 작업이 시작되면 취소할 수 없습니다. 앞의 질문에서 설명했듯이 프로세스 중에 발생할 수 있는 잠재적 성능 문제를 인지하고 변환을 계획할 때 위에 나열된 모범 사례를 준수합니다.

**Q.** 변환 작업이 실패한 경우 볼륨에 미치는 영향은?

**A.** 볼륨 변환은 클라우드 연결 문제로 인해 실패할 수 있습니다. 결국 장치는 계층화된 데이터를 클라우드에서 종료하려는 일련의 시도가 실패한 후에 변환 프로세스를 중지할 수 있습니다. 이러한 시나리오에서 볼륨 유형은 변환에 앞서 원본 볼륨 유형으로 이어집니다.

* 볼륨 변환 실패를 알려주는 중요한 경고가 발생합니다. 자세한 내용은 [로컬로 고정된 볼륨에 관련된 경고](storsimple-manage-alerts.md#locally-pinned-volume-alerts)
* 계층화된 볼륨을 로컬로 고정된 볼륨으로 변환하는 경우 데이터가 클라우드에 있으므로 볼륨은 계층화된 볼륨의 속성을 계속 보이게 됩니다. 연결 문제를 해결한 다음 변환 작업을 다시 시도하는 것이 좋습니다.
* 마찬가지로 로컬로 고정된 볼륨에서 계층화된 볼륨으로 변환하는 경우 볼륨이 로컬로 고정된 볼륨으로 표시되지만 계층화된 볼륨의 기능을 수행합니다(데이터가 클라우드에 유출되었을 수 있기 때문임). 그러나 장치의 로컬 계층에서 공간을 계속 차지합니다. 이 공간은 다른 로컬로 고정된 볼륨에 사용할 수 없습니다. 이 작업을 다시 시도하여 볼륨 변환이 완료되고 장치에서 로컬 공간을 회수할 수 있도록 하는 것이 좋습니다.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>로컬로 고정된 볼륨 복원에 대한 질문
**Q.** 로컬로 고정된 볼륨이 즉시 복원되나요?

**A.** 네, 로컬로 고정된 볼륨이 즉시 복원됩니다. 클라우드에서 볼륨에 대한 메타데이터 정보를 복원 작업의 일부로 끌어오는 즉시 볼륨은 온라인 상태가 되고 호스트에서 액세스될 수 있습니다. 그러나 볼륨 데이터에 대한 로컬 보증은 데이터가 클라우드에서 다운로드될 때까지 표시되지 않으며 복원 중에 이러한 볼륨에서 성능을 저하될 수 있습니다.

**Q.** 로컬로 고정된 볼륨을 복원하는 데 걸리는 시간은?

**A.** 볼륨 데이터가 백그라운드에서 계속 다운로드되는 동안 로컬로 고정된 볼륨은 즉시 복원되고 볼륨 메타데이터 정보를 클라우드에서 검색하는 즉시 온라인 상태가 됩니다. 볼륨 데이터에 대한 로컬 보증을 가져오는 복원 작업의 뒷 부분은 장기 실행 작업으로 모든 데이터를 다시 로컬로 만드는 데 몇 시간이 걸릴 수 있습니다. 동일한 작업을 완료하는 데 걸리는 시간은 복원 중인 볼륨의 크기 및 사용 가능한 대역폭 등 여러 요인에 따라 달라집니다. 복원되는 원본 볼륨이 삭제된 경우 장치에서 로컬 공간을 복원 작업의 일부로 만드는 데 추가 시간이 걸립니다.

**Q.** (볼륨을 계층화할 때 만든) 이전 스냅숏에 기존의 로컬로 고정된 볼륨을 복원해야 합니다. 이 경우에 볼륨은 계층화되어 복원될 수 있나요?

**A.** 아니요, 볼륨은 로컬로 고정된 볼륨으로 복원됩니다. 스냅숏이 볼륨이 계층화된 시점으로 시간이 지정되지만 기존 볼륨을 복원하는 동안 StorSimple은 항상 현재 존재하는 디스크의 볼륨 유형을 사용합니다.

**Q.** 최근 내 로컬로 고정된 볼륨을 확장했지만 볼륨의 크기가 작았을 시점으로 데이터를 복원해야 합니다. 복원이 현재 볼륨의 크기를 조정하고 복원이 완료되면 볼륨의 크기를 확장해야 합니까?

**A.** 예, 복원이 현재 볼륨의 크기를 조정하고 복원이 완료되면 볼륨의 크기를 확장해야 합니다.

**Q.** 복원하는 동안 볼륨의 유형을 변경할 수 있나요?

**A.**아니요, 복원하는 동안 볼륨의 유형을 변경할 수 없습니다.

* 삭제된 볼륨은 스냅숏에 저장된 형식으로 복원됩니다.
* 기존 볼륨은 스냅숏에 저장된 형식에 관계없이 해당하는 현재 유형을 기반으로 복원됩니다(이전의 두 질문 참조).

**Q.** 내 로컬로 고정된 볼륨을 복원해야 하지만 스냅샷에 잘못된 시점을 선택했습니다. 현재 복원 작업을 취소할 수 있습니까?

**A.** 예, 진행 중인 복원 작업을 취소할 수 있습니다. 볼륨의 상태는 복원이 시작할 때의 상태로 롤백될 수 있습니다. 그러나 복원이 진행 중인 동안 볼륨에 만들어진 쓰기는 손실됩니다.

**Q.** 내 로컬로 고정된 볼륨 중 하나에서 복원 작업을 시작했지만 이제 만들기를 다시 수집하지 않는 내 백로그 카탈로그에 스냅숏을 표시합니다. 이 용도는 무엇인가요?

**A.** 복원 작업에 앞서 전에 만들어지는 임시 스냅숏으로 복원을 취소되거나 실패하는 경우에 롤백에 사용됩니다. 이 스냅숏을 삭제하지 마세요. 복원이 완료되면 자동으로 삭제됩니다. 복원 작업에 로컬로 고정된 볼륨만 있거나 로컬로 고정된 볼륨과 계층화된 볼륨이 함께 있는 경우에 이 동작이 발생할 수 있습니다. 복원 작업에 계층화된 볼륨만 있으면 이 동작이 발생 하지 않습니다.

**Q.** 로컬로 고정된 볼륨을 복제할 수 있나요?

**A.** 예, 할 수 있습니다. 그러나 로컬로 고정된 볼륨은 기본적으로 계층화된 볼륨으로 복제됩니다. [로컬로 고정된 볼륨을 복제하는](storsimple-clone-volume-u2.md) 방법에 대한 자세한 내용

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>로컬로 고정된 볼륨 장애 조치에 대한 질문
**Q.** 내 장치를 다른 실제 장치로 장애 조치해야 합니다. 내 로컬로 고정된 볼륨이 로컬로 고정되거나 계층화되어 실패하나요?

**A.** 대상 장치의 소프트웨어 버전에 따라 로컬로 고정된 볼륨을 다음으로 장애 조치합니다.

* 대상 장치가 StorSimple 8000 시리즈 업데이트 2를 실행한 경우 로컬로 고정됨
* 대상 장치가 StorSimple 8000 시리즈 업데이트 1.x를 실행한 경우 계층화됨
* 대상 장치가 클라우드 장비인 경우 계층화됨(소프트웨어 버전 업데이트 2 또는 1.x)

자세한 내용은 [버전에 걸쳐 로컬로 고정된 볼륨의 장애 조치 및 DR](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**Q.** 로컬로 고정된 볼륨이 DR(재해 복구) 중에 즉시 복원되나요?

**A.** 네, 장애 조치 중에 로컬로 고정된 볼륨이 즉시 복원됩니다. 클라우드에서 볼륨에 대한 메타데이터 정보를 장애 조치 작업의 일부로 끌어오는 즉시 볼륨은 대상 장치에서 온라인 상태가 되고 호스트에서 액세스될 수 있습니다. 한편, 볼륨 데이터가 백그라운드에서 계속 다운로드되면 장애 조치 중에 이러한 볼륨의 성능이 저하될 수 있습니다.

**Q.** 장애 조치 작업이 완료되면 대상 장치에 복원되고 있는 로컬로 고정된 볼륨의 진행 상태를 어떻게 추적할 수 있나요?

**A.** 장애 조치 작업 중에 장애 조치 작업은 장애 조치 설정의 모든 볼륨이 즉시 복원되고 대상 장치에 온라인 상태가 되면 완료됨으로 표시됩니다. 여기에는 장애 조치될 수 있는 로컬로 고정된 볼륨을 모두 포함합니다. 그러나 데이터의 로컬 보증은 볼륨에 대한 모든 데이터가 다운로드되었을 때만 제공됩니다. 장애 조치의 일부로 만들어진 해당 복원 작업을 모니터링하여 장애 조치된 로컬로 고정된 볼륨에 대한 진행률을 각각 추적할 수 있습니다. 이러한 개별 복원 작업은 로컬로 고정된 볼륨에 대해서만 만들어집니다.

**Q.** 장애 조치 중에 볼륨의 유형을 변경할 수 있나요?

**A.** 아니요, 장애 조치 중에 볼륨의 유형을 변경할 수 없습니다. StorSimple 8000 시리즈 업데이트 2를 실행하는 다른 실제 장치에 장애 조치하는 경우 볼륨은 스냅숏에 저장된 볼륨 유형에 따라 장애 조치됩니다. 다른 장치 버전에 장애 조치하는 경우 장애 조치 후에 볼륨 유형에 대해 위의 질문을 참조합니다.

**Q.** 클라우드 장비에 로컬로 고정된 볼륨을 사용하여 볼륨 컨테이너를 장애 조치할 수 있나요?

**A.** 예, 할 수 있습니다. 로컬로 고정된 볼륨은 계층화된 볼륨으로 장애 조치됩니다. 자세한 내용은 [버전에 걸쳐 로컬로 고정된 볼륨의 장애 조치 및 DR](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


