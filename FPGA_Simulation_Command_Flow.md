# xvlog → xelab → xsim

## xvlog
- **Vivado 시뮬레이터 xsim의 Verilog/SystemVerilog 컴파일러**
- 예시 → xvlog 하위_모듈.v 상위_모듈.v 
- 하위 모듈 → 상위 모듈 순이 안정적
    1. Default : Verilog 문법으로 해석
    2. -sv : SystemVerilog 문법으로 해석
    3. -d name=VAL : 매크로 정의
    4. -i <dir> : 검색 경로추가
    5. -L <lib> : UVM 등 참조 라이브러리 지정
    6. -log <file> : 로그 파일 이름 지정
    7. -work <lib> : 결과물을 저장할 라이브러리 이름을 지정
- Verilog : 디지털 회로 설계용 언어
- SystemVerilog : Verilog + Testbench + OOP + 고급 데이터 타입
- 컴파일된 결과는 xsim.dir에 저장

## xelab
- **최상위 모듈 기준으로 연결 후 Simulation용 실행 스냅샷을 만든다
- 예시 → xelab tb_combi_test -debug wave -s tb_combi_test
- xelab <top_module_name> -s <snapshot_name> [옵션]
    1. -debug
        - typical : 일반적인 디버깅 정보 (파형, 변수 확인)
        - wave : 파형 중심 정보 저장
    2. -timescale 1ns/1ps : 시뮬레이션 시간 단위 강제 지정
    3. -L <lib> : 다른 라이브러리 참조
션션
## xsim
- 스냅샷을 받아 시뮬레이션 실행
- 예시 → xsim tb_combi_test -gui -wdb simulate_xsim_tb_combi_test.wdb
- xsim <snapshot_name> [옵션] 
    1. -gui : GUI 모드로 실행
    2. -runall : GUI 사용하지 않고 처음부터 끝까지 실행
    3. -tclbatch <file.tcl> : TCL 스크립트 실행
        - Vivado는 내부 명령어 체계 TCL 기반
            - GUI에서 버튼 눌러도 TCL 명령이 실행
        - GUI 작업을 그대로 스크립트화 가능
    4. -wdb <file.wdb> : 파형(DB) 저장 파일 지정