## 숫자 표현방식
- [비트수]'[기수][값]
    - 비트수: 표현할 비트 폭 (optional)
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
## [IF문, Case문] Condition
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

- 

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



1. **Blocking 할당 [ = ] : 조합논리에서 사용 (always @*)**
2. **Non-Blocking 할당 [ <= ] : 순차로직에서 사용 (always @(posedge clk))**


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

 