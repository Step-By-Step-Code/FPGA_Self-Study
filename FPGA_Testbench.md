## Testbench
- **Testbench도 하나의 모듈**

## 모듈 인스턴스화
- 이미 정의된 모듈을 다른 모듈 안에서 불러다 씀
```verilog
module_name instance_name(
    .in1(signal_a),
    .in2(signal_b),
    .out1(signal_c)
)
```