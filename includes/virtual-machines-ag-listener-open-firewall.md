이 단계에서는 부하가 분산된 끝점에 대한 프로브 포트(이전에 지정한 대로 59999)를 여는 방화벽 규칙과 가용성 그룹 수신기 포트를 여는 다른 규칙을 만듭니다. 가용성 그룹 복제본이 포함된 Azure VM에서 부하가 분산된 끝점을 만들었으므로 해당 Azure VM에서 수신기 포트와 프로브 포트를 열어야 합니다.

1. 복제본을 호스트하는 VM에서 **고급 보안이 포함된 Windows 방화벽**을 시작합니다.
2. **인바운드 규칙**을 마우스 오른쪽 단추로 클릭하고 **새 규칙**을 클릭합니다.
3. **규칙 종류**에서 **포트**를 선택한 후 **다음**을 클릭합니다.
4. **프로토콜 및 포트** 페이지에서 **TCP**를 선택하고 **특정 로컬 포트** 상자에 **59999**를 입력합니다. 그런 후 **다음**을 클릭합니다.
5. **작업** 페이지에서 **연결 허용**을 선택한 상태로 유지하고 **다음**을 클릭합니다.
6. **프로필** 페이지에서 기본 설정을 그대로 적용하고 **다음**을 클릭합니다.
7. **이름** 페이지의 **이름** 텍스트 상자에 **Always On 수신기 프로브 포트**와 같은 규칙 이름을 지정하고 **마침**을 클릭합니다.
8. 가용성 그룹 수신기 포트(이전에 스크립트의 $EndpointPort 매개 변수에서 지정된 대로)에 대해 위의 단계를 반복하고 **Always On 수신기 포트**와 같은 적절한 규칙 이름을 지정합니다.



<!--HONumber=Nov16_HO5-->


