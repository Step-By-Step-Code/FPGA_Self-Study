## 

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

## 📌 규칙
```verilog
assign out = {a, b, c};
```
- a는 out의 상위 비트(MSB) 쪽에 배치
- c는 out의 하위 비트(LSB) 쪽에 배치

 