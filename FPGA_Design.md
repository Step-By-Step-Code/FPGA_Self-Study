## 모듈 설계 원칙
1. 모듈은 작고 단일 기능에 집중
    - MUX, Counter, Adder 등 작은 단위로 분리
    - 복잡한 기능은 여러 작은 모듈로 쪼개서 상위에서 합침

2. 인터페이스(포트) 명확히
    - 모듈 간 연결은 input/output으로만
    - 내부 동작은 외부에서 몰라도 되게 캡슐화

3. 인스턴스 네이밍 규칙
    - 인스턴스 이름은 기능 기반으로 명확히 작성 (예: adder0, mux_ctrl)
    - 동일 모듈을 여러 번 사용할 수 있으므로 이름 구분 중요

4. 재사용 가능하게 설계: 같은 모듈을 여러 번 쓸 수 있도록 일반화

5. 테스트벤치 분리: 상위 모듈에 testbench 작성해서 하위 모듈까지 자연스럽게 검증

6. Port Mapping 방식
- 명시적 연결 (Named association) → 가독성 좋음
    ```verilog
    full_adder fa0 (.a(a[0]), .b(b[0]), .cin(cin), .sum(sum[0]), .cout(c1));
    ```

- 순서 연결 (Positional association) → 코드 짧음, 실수 위험 있음
    ```verilog
    full_adder fa0 (a[0], b[0], cin, sum[0], c1);
    ```
7. 오류를 줄이기 위한 방식
- verilog 파일 하나 당 하나의 모듈을 규칙
    - 일부 툴이나 특정 옵션에서 순서 문제 발생할 수 있음
        - 인스턴스로 사용되는 모듈 정의 파일이 먼저 컴파일

-------------------------------------------------------------------
## @(event);
- 특정 이벤트가 발생할 때까지 시뮬레이션 흐름을 멈추고 대기하는 명령어야.
- 예시 : **@(negedge clk);**
-------------------------------------------------------------------
## 모듈 인스턴스화
    모듈이름 인스턴스이름 (
        .포트이름(연결할 신호),
        .포트이름(연결할 신호)
    );

## 파라미터가 들어가는 모듈
- 형식
```verilog
module 모듈이름 #(
    parameter PARAM1 = 기본값1,   // 파라미터1
    parameter PARAM2 = 기본값2    // 파라미터2
)(
    input  wire ... ,             // 입력 포트
    output wire ...               // 출력 포트
);
    // 내부에서 파라미터 활용 가능
    wire [PARAM1-1:0] data_bus;
    ...
endmodule
```

- 사용하는 방법

```verilog
// 기본값 사용
모듈이름 인스턴스이름 (
    .포트이름(신호),
    .포트이름(신호)
);

// 파라미터 재정의 후 인스턴스화
모듈이름 #(
    .PARAM1(값1),
    .PARAM2(값2)
) 인스턴스이름 (
    .포트이름(신호),
    .포트이름(신호)
);
```
-------------------------------------------------------------------------

## Backtick[`] 사용하는 경우
1. 매크로 정의/호출
```verilog
`define WIDTH 8

module my_module;
  reg [`WIDTH-1:0] data;   // = reg [7:0] data;
endmodule
```

2. 조건부 컴파일
```verilog
`define DEBUG

module test;
  initial begin
`ifdef DEBUG
    $display("Debug mode ON");
`endif
  end
endmodule
```

3. include
```verilog
`include "my_header.vh"
```

-----------------------------------------------------------------------------------


## 숫자 표현방식
- [비트수]'[기수][값]
    - 비트수: 표현할 비트 폭 (optional)
    - ' 작은 따옴표 사용
    - 기수(radix):
        - b → 2진수 (binary)
        - o → 8진수 (octal)
        - d → 10진수 (decimal)
        - h → 16진수 (hexadecimal)
    - 값: 해당 진법의 숫자
```verilog
4'b1010   // 4비트 2진수: 1010 (decimal 10)
8'd25     // 8비트 10진수: 00011001
12'hABC   // 12비트 16진수: 1010 1011 1100
6'o77     // 6비트 8진수: 111111 (decimal 63)
```
-----------------------------------------------------
## [IF문, Case문, Ternary Operator] Condition

| 특징            | if-else / 삼항 연산자    | case                 |
| ------------- | ------------------- | -------------------- |
| **우선순위**      | 있음 | 없음|
| **조건 평가 방식**  | 순차적 (위 → 아래)        | 병렬적                  |
| **하드웨어 구현**   | 우선순위 인코더 (MUX 체인)   | n:1 멀티플렉서            |
| **적합한 사용 사례** | 우선순위가 중요한 경우        | 상호 배타적인 여러 분기를 처리할 때 |


- **기본 if-else**
```verilog
if (cond) 
    out = a;
else 
    out = b;
```

- **if – else if – else if … – else (다중 분기)**
```verilog
if (sel == 2'b00)
    out = a;
else if (sel == 2'b01)
    out = b;
else if (sel == 2'b10)
    out = c;
else
    out = d;
```

- **CASE문 [MUX, DEMUX, FSM]**
    - default는 설계의 안정상 있는 것이 좋음
        - Latch 발생
            - 오류, 디버깅 어려움, 클럭 신호 없이 항상 활성화 → 불필요한 전력 소비.
            - 클럭 신호에 제어되지 않음 → 입력 신호에 즉시 변화 → 타이밍 분석 어려움 → 메타 안정성 문제 발생, 글리치 발생

```verilog
case (<expression>)
    <value1>: begin
        // statement
    end
    <value2>: begin
        // statement
    end
    default: begin
        // statement
    end
endcase 
```

- **Ternary Operator**
    - 회로적으로 MUX를 표현
        - 2:1 MUX 4개 Gate
        - 4:1 MUX 7개 Gate
        - 8:1 MUX 15개 Gate

```verilog
    <result> = (<condition>) ? <value_if_true> : <value_if_false>;
```

- **Latch 설계**
```verilog
    always @(*) begin
        if() statement;
    end
```

- **여러 줄을 묶을 때**
```verilog
if() begin
end
// --------------
else if() begin
end
// --------------
else begin
end
```

- **DEMUX 구현**
```verilog
always @(*) begin
    case (sel)
        1'b0: begin
            out[0] = in;
            out[1] = 0;
        end
        1'b1: begin
            out[1] = in;
            out[0] = 0;
        end 
    endcase   
end
```



1. **Blocking 할당 [ = ] : 조합논리에서 사용 (always @*)**
    - 즉시 업데이트
2. **Non-Blocking 할당 [ <= ] : 순차로직에서 사용 (always @(posedge clk))**
    - 시간 단계 끝에서 업데이트


---------------------------------------------------

## Modeling Styles
```verilog
not (y, a); // Gate-Level Modeling
```


```verilog
assign y = ~a; // Dataflow Modeling
```

```verilog
always @(*) begin // Behavioral Modeling
        y = ~a;
end
```
1. **always** : 계속 실행되는 프로세스 블록
        - Sensitivity list가 변할 때마다 실행
2. **@(...)** : Sensitivity List
        - @(*) : 모든 신호를 감지
        - 
3. **begin ... end** : 여러 개의 문장을 한 블록으로 묶어줌

-------------------------------------------------

## 변수 선언
### Verilog에서 reg는 변수 타입
1. 조합 논리 사용되는 경우
```verilog
reg y;

always @(*) begin
    y = ~a;
end
```

2. 순차 논리로 사용되는 경우
```verilog
reg y;

always @(posedge clk) begin
    y <= a;
end
```

3. 초기값 저장 Testbench
- 시뮬레이션 환경에서 메모리 변수처럼 사용
```verilog
reg [7:0] data;
initial begin
    data = 8'b10101010;
end
```

### parmeter, localparam
```verilog
parameter WIDTH = 8;
my_module #(.WIDTH(16)) u0 (...);
```
- **parameter**
    - 상수, 외부에서 override 가능
    - 모듈의 기본값 상수 정의
    - 인스턴스화 할 때 값 변경 가능

```verilog
localparam S_IDLE = 2'b00;
localparam S_RUN  = 2'b01;
localparam S_DONE = 2'b10;
```

- **localparam**
    - parameter 비슷하게 상수 정의
    - 모듈 외부에서 값을 바꿀 수 없음 → 고정된 상수
    - FSM 상태값 정의할 때 사용


----------------------------------------------------------------------------------------
## {} : 비트 연결 연산자
- a 4bit, b 4bit
    - {a, b} : 8bit → [a 상위비트]  [b 하위비트]

----------------------------------------------------------------------------------------
## 📌 규칙
```verilog
assign out = {a, b, c};
```
- a는 out의 상위 비트(MSB) 쪽에 배치
- c는 out의 하위 비트(LSB) 쪽에 배치

 