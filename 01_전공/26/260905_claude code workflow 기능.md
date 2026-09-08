---
tags:
  - claude_code
  - agent_workflow
  - ai4research
links:
  -
---
## workflow 기능이란?
- 자바 스크립트로 agent들이 해야할 workflow를 정의하는 방법.
- 워크플로우는 **작업이 한 에이전트가 컨텍스트에 보유할 수 있는 것보다 크거나**, **같은 동작을 여러 단계**에서 해야할 때 유용한 기능.

## Can / Can't
1. Q: python 스크립트를 결정론적으로 호출할 수 있는가?
    1. A: workflow 스크립트 자체에서는 파일 시스템에 접근할 수 없다. agent가 실행을 해야하는데, 검증 결과를 보고 분기하는게 if/while 같은 코드 레벨로 짜여지기 때문에 오케스트레이터가 조건을 지킬 가능성이 높아짐.
2. Q: 실행 순서를 그래프로 조절할 수 있는가?
    1. A: 가능하다. pipeline()/parallel()이 이런 목적이다. **workflow 기능의 핵심은 loops, conditional, fan-out을 deterministic하게 하자!**
3. Q: 사람이 중간에 개입해서 accept/reject를 결정할 수 있는가?
    1. A: 불가능. workflow는 백그라운드에서 끝까지 돌고 결과를 반환하는 구조. 즉, **사람의 개입이 필요없는 일련의 작업에 사용해야함.** "작업 -> 사람 피드백 -> 작업 -> 사람 피드백" 이런 구조에는 workflow 기능이 맞지 않음.

## 결론
- 중간에 사람의 개입이 필요한 전체 작업에서는 workflow 기능을 쓸 수 없다. workflow는 백그라운드로 한번에 돌기 때문에 중간에 사람이 개입할 수 없다.
- orchestrator-worker 구조에서 orchestrator가 아니라 worker에 workflow를 작성해서 사용하는 것이다.
- **workflow 기능을 사용해도 자유로운 human-in-the-loop을 구현하기는 어렵다! 그렇지만 작은 단위로는 workflow를 이용해서 deteministic point를 만들어낼 수 있다.**