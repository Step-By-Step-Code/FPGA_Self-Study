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

- **CASE문**
    - default는 설계의 안정상 있는 것이 좋음
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



1. **Blocking 할당 [ = ] : 조합논리에서 사용 (always @*)**
    - 즉시 업데이트
2. **Non-Blocking 할당 [ <= ] : 순차로직에서 사용 (always @(posedge clk))**
    - 시간 단계 끝에서 업데이트


-----------------------------------------------------------------------

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

-------------------------------------------------------------------------------------------------

## reg 변수 선언
**Verilog에서 reg는 변수 타입**
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

-----------------------------------------------------------------------------------------------------------------------------------------

## 📌 규칙
```verilog
assign out = {a, b, c};
```
- a는 out의 상위 비트(MSB) 쪽에 배치
- c는 out의 하위 비트(LSB) 쪽에 배치

 