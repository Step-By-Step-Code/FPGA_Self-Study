## FSM [Moore/Mealy Model]
하나의 상태만을 가지게 되고, 어떠한 Event에 의해 현 State에서 다른 State로 변화. → 이를 전이[Transition]라 함 [IDLE, RUN, DONE] 
- System 및 Data Flow Path의 동작을 제어하는 데 사용되는 Sequential 회로
    - Current state와 Input에 따라서 Next state가 결정
- 현재 상태를 정확하게 명시하고 입력에 의해서 동작을 시키도록 기술
    - Control Logic 쉽게 설계할 수 있음

-----------------------------------------------------------
# Moore/Mealy Model → 상황에 맞게 사용해야함
- **Mealy Machine : 외부 출력 결정 시 입력과 현재 상태 모두에 영향 받음**
    - State와 Input을 보기 때문에 출력 Timing 손해
        - 입력 신호가 조합 경로를 통해 바로 출력에 반영됨
        - 즉, 입력이 클록 사이클 중간에 바뀌면 → 출력도 바로 흔들릴 수 있음
    - F/F을 거치지 않고 나간다면, 즉 Registered Output이 아니면 Noise/Glitch 유발 가능
        - 출력은 입력까지 같이 보기 때문에 응답이 빠르지만
        - 입력 노이즈 때문에 글리치가 생길 수 있음 (특히 출력이 레지스터링되지 않고 직접 연결될 때)

그래서 양방향 handshake 인터페이스 같이 "입력을 보고 바로 반응해야 하는 곳"에 유리
    - input까지 봐야하는 Handshake interface에 유리
- **Moore Machine 외부 출력 결정 시 입력이 개입하지 않음 → 현재 상태만을 보고 출력**
    - Output 입장에서 Timing이 더 좋음
        - 상태는 클록 상승엣지에서만 바뀌니까 → 출력도 클록 이벤트에 맞춰 안정적으로 변함
        - 즉, 출력이 플립플롭(F/F) 거쳐 나온 것처럼 동작 → 타이밍 여유(Slack)가 있음
    - Input을 안보기 때문에 단방향으로 흘러가는 시스템에서 사용할 수 있음