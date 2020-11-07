# 핑퐁게임_System-On-chip 설계 및 프로그래밍 

---
### 설명(요약문)

---

**SoC 설계 및 프로그래밍 강의를 수강하면서 제작한 프로젝트에 대한 내용을 정리/공유한 Github 포트폴리오입니다.**

- HW 설계 및 SW 프로그래밍의 Co-design을 통한 TFT LCD(게임 화면)과 4개의 Switch(방향키 제어) 기반의 Ping-pong Game을 제작 
: HW-SW 데이터 통신 실시간/동적(Realtime and Dynamic)으로 화면 픽셀을 조절하여 두명의 플레이어 1p,2p가 즐길 수 있는 탁구 게임을 구현함.
<img src="/SOC발표JPG/SoC 텀프 최종 발표_0003.jpg" height="80%" width="80%">

- **SW개발 : Xilinx Vivado SoC Embedded Design&Programming Tool**
- **HW개발(SoC)  :  Text LCD와 LED, 7-Segment, TFT-LCD 화면(적당한 크기의 디스플레이 화면에 해당)이 탑재된 Xilinx Zynq-7000 SoC(System-on-Chip) Kit 이용**

  

Xilinx사의 Vivado라는 SoC 칩을 실습에 사용함 

- Verilog를 이용한 하드웨어(논리회로 IP 모듈)을 설계
- 해당 하드웨어 모듈의 포트 번호를 받아 칩을 소프트웨어적으로 제어하기 위한 C언어 기반의 SDK Firmware를 설계함




### 저장소 유형

---

- 유형: 교과목형
- 수강정보

    ITEC412/SoC 설계 및 프로그래밍/2020년/1학기/문병인 교수님
    
    
### 1. 프로젝트 설명(개요)


---
#### Hardware 설계

Verilog를 통해서 SOC Kit에 탑재된 입출력을 제어하는 하드웨어 모듈(IP)을 설계함. 

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0004.jpg" height="60%" width="60%">

---
#### Software 설계

C 기반의 SDK(Software Development Kit) 개발 툴을 통해서 프로그래밍 함.  

SOC의 Main Logic을 제어하는 두뇌 역할. HW의 IP별 Port를 Read/Write하여 직접적으로 SW 제어문을 통해 HW를 실시간 제어함 

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0006.jpg" height="60%" width="60%">

---





### 2. HARDWARE 설계 (AXI Registers IP)

---

**- TFT LCD를 제어하는 IP**

```
  module Debounce_Switch (
	input wire i_Clk,
	input wire i_Switch,
	output wire o_Switch 
);

parameter c_DEBOUNCE_LIMIT = 250000; // 10ms at 25MHz

reg r_State = 1'd0;
reg [17:0] r_Count = 18'd0;

always @(posedge i_Clk)
	begin
		if(i_Switch !== r_State && r_Count < c_DEBOUNCE_LIMIT)
			r_Count <= r_Count + 18'd1; // This is Counter
		else if(r_Count == c_DEBOUNCE_LIMIT)
			begin
				r_Count <= 18'd0;
				r_State <= i_Switch;
			end
		else
			r_Count <= 18'd0;
	end

assign o_Switch = r_State;

endmodule
```


  

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0007.jpg" height="60%" width="60%">

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0008.jpg" height="60%" width="60%">

**- PushButton을 제어하는 IP**

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0009.jpg" height="60%" width="60%">

**- Text LCD를 제어하는 IP**

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0010.jpg" height="60%" width="60%">

**- 7-Segment를 제어하는 IP**

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0011.jpg" height="60%" width="60%">



### 3. SOFTWARE 프로그래밍 (SDK, Firmware) 

** C 코드를 통한 HW(SOC)의 레지스터(AXI) 값을 직접적으로 변경하여 상태를 전달/변경하여 제어하는 역할.**

---

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0012.jpg" height="100%" width="100%">

<img src="/SOC발표JPG/SoC 텀프 최종 발표_0013.jpg" height="100%" width="100%">





