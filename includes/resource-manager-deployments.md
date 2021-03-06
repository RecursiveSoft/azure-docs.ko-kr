## <a name="incremental-and-complete-deployments"></a>증분 및 전체 배포
리소스를 배포할 때 배포를 증분 업데이트 또는 전체 업데이트할지 지정합니다. 이러한 두 모드 간 주요 차이점은 Resource Manager가 템플릿에 없는 리소스 그룹에서 기존 리소스를 처리하는 방식입니다.

* 전체 모드에서는 Resource Manager가 리소스 그룹에 존재하지만 템플릿에는 지정되지 않은 리소스를 **삭제**합니다. 
* 증분 모드에서는 Resource Manager가 리소스 그룹에 존재하지만 템플릿에는 지정되지 않은 리소스를 **변경하지 않고 유지**합니다.

두 모드 모두에 대해 Resource Manager는 템플릿에 지정된 모든 리소스를 프로비전하려고 합니다. 리소스가 리소스 그룹에 이미 존재하고 해당 설정이 변경되지 않으면 작업 결과도 변경되지 않습니다. 리소스에 대한 설정을 변경하면 리소스가 새로운 설정으로 프로비전됩니다. 그러나 기존 리소스의 위치 또는 유형을 업데이트할 수 없습니다. 대신, 새 리소스를 필요한 위치 또는 유형으로 배포합니다.

기본적으로 Resource Manager는 증분 모드를 사용합니다.

