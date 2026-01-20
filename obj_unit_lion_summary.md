### **obj_unit_lion (킹 유닛) 특징**

*   **역할**: 게임의 승패를 결정하는 핵심 유닛입니다. 코드 상에서 "KING"으로 표시됩니다.
*   **이동**: 시작 지점에서 상하좌우 및 대각선을 포함한 모든 방향으로 1칸씩 움직일 수 있습니다.
    ```gml
    // x, y 좌표의 차이가 각각 1 이하일 경우에만 이동 가능
    return (abs(_xstart - _xdest) <= 1 && abs(_ystart - _ydest) <= 1);
    ```
*   **공격**:
    *   공격 시, 대상 유닛의 최대 체력(`unit_max_value`) 만큼의 피해를 주어 **한 번에 즉시 처치**합니다.
    ```gml
    // 공격 함수: 대상의 최대 체력으로 방어 함수를 호출
    if (_target.unit_func_defence(id, _target.unit_max_value)) { ... }
    ```
*   **방어 (생명력)**:
    *   체력(`unit_value`)이 1로 설정되어 있어, **어떤 공격이든 피해를 받으면 즉시 파괴**됩니다.
    ```gml
    // 방어 함수: 0보다 큰 피해를 받으면 자신의 체력을 0으로 만들고 파괴됨
    if (_dmg <= 0) return false;
    unit_value = 0;
    unit_hurt = true;
    ```
*   **패배 조건**:
    *   이 유닛이 파괴되면 해당 진영이 패배하는 트리거로 작동합니다.
    *   `ui_control` 오브젝트에 패배 신호를 보내고, 턴 순서를 초기화하는 등 게임 종료와 관련된 처리를 수행합니다.
    ```gml
    // 자신의 진영이 패배했음을 알리는 변수 설정
    _test_faction_lost = _self.unit_factionmask;
    order_rotation_timer = 180; // 턴 타이머 리셋
    ```
*   **상속**: 다른 기본 유닛 오브젝트(`obj_unit`)의 기능을 상속받아 그 위에 사자 유닛만의 고유한 로직을 덮어쓰는 구조로 되어있습니다 (`event_inherited()`).