# HVX INTRINSIC EXAMPLES(V68)

<br>

<dl>
<p align="right"> 
	<font color=gray size=6> contact: jilong1993@gmail.com </font>
</p>
<dl>

<br>
<br>

----
* **解释中加入了一些个人理解，不代表编译器行为**
* 打印HVX_VectorPred时，是通过转成uint8_t来打印的，所以标上的是uint8_t或者int8_t的字样, 对应的实际是bit.

<div STYLE="page-break-after: always;"></div>

# Table
  - [ALU](#alu)
    - [add](#add)
    - [sub](#sub)
    - [abs](#abs)
    - [abs diff](#abs-diff)
    - [sad （Sum Absolute Diffence）](#sad-sum-absolute-diffence)
    - [max](#max)
    - [min](#min)
    - [avg](#avg)
    - [Condacc](#condacc)
    - [mpy](#mpy)
    - [Mpa](#mpa)
    - [shift](#shift)
  - [Vector Permutation](#vector-permutation)
    - [valign](#valign)
    - [rotr](#rotr)
    - [shuffle](#shuffle)
    - [deal](#deal)
    - [pack](#pack)
    - [unpack](#unpack)
    - [Other](#other)
  - [Logic Operation](#logic-operation)
    - [compare](#compare)
    - [Bit Operation](#bit-operation)
  - [Vector Transfer](#vector-transfer)
    - [data type upgrade](#data-type-upgrade)
    - [sat](#sat)
  - [Scatter \& Gather](#scatter--gather)
    - [gather](#gather)
    - [scatter](#scatter)
  - [Lut](#lut)
  - [Other](#other-1)
    - [store](#store)
    - [splat](#splat)
    - [setq](#setq)
    - [prefixsum](#prefixsum)

## ALU

### add
- Vector -> Vector

**HVX_Vector Q6_Vub_vadd_VubVub_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.ub[i] = sat_u8(Vu.ub[i] + Vv.ub[i])  带有饱和的无符号加法
```

| Vu(uint8_t) | 0 | 1 | 1 | 255 | 255 | 255 | 127 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 0 | 1 | 0 | 255 | 1 | 128 | 128 | ... |
| res(uint8_t) | 0 | 1 | 2 | 255 | 255 | 255 | 255 | 255 | ... |

**HVX_Vector Q6_Vb_vadd_VbVb_sat(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
Vd.b[i] = sat_s8(Vu.b[i] + Vv.b[i])  带有饱和的有符号加法
```

| Vu(int8_t) | 0 | 1 | 1 | -1 | -1 | -1 | 127 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 0 | 0 | 1 | 0 | -1 | 1 | -128 | -128 | ... |
| res(int8_t) | 0 | 1 | 2 | -1 | -2 | 0 | -1 | -128 | ... |

> Other Instructions:<br>
**HVX_Vector Q6_Vuh_vadd_VuhVuh_sat(HVX_Vector, HVX_Vector)**<br>
**HVX_Vector Q6_Vh_vadd_VhVh_sat(   HVX_Vector, HVX_Vector)**<br>
**HVX_Vector Q6_Vuw_vadd_VuwVuw_sat(HVX_Vector, HVX_Vector)**<br>
**HVX_Vector Q6_Vw_vadd_VwVw_sat(   HVX_Vector, HVX_Vector)**<br>

**HVX_Vector Q6_Vb_vadd_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.ub[i] = Vu.ub[i] + Vv.ub[i]  无饱和的加法，ub和b的行为是一样的
```

| Vu(uint8_t) | 0 | 1 | 1 | 255 | 255 | 255 | 127 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 0 | 1 | 0 | 255 | 1 | 128 | 128 | ... |
| res(uint8_t) | 0 | 1 | 2 | 255 | 254 | 0 | 255 | 0 | ... |

> Other Instructions:<br>
**HVX_Vector Q6_Vh_vadd_VhVh(HVX_Vector, HVX_Vector)**<br>
**HVX_Vector Q6_Vw_vadd_VwVw(HVX_Vector, HVX_Vector)**<br>

> **注: 没有非sat的无符号指令, 可以用有符号的非饱和指令代替**<br>



**TOADD:** <br>
> **HVX_Vector Q6_Vqf16_vadd_VhfVhf(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vqf16_vadd_Vqf16Vhf(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vqf16_vadd_Vqf16Vqf16(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vqf32_vadd_Vqf32Vqf32(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vqf32_vadd_Vqf32Vsf(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vqf32_vadd_VsfVsf(HVX_Vector Vu, HVX_Vector Vv)** <br>

> **HVX_Vector Q6_Vw_vadd_VwVwQ_carry_sat(HVX_Vector Vu, HVX_Vector Vv, HVX_VectorPred Qs)**<br>
> **HVX_Vector Q6_Vw_vadd_VwVwQ_carry(HVX_Vector Vu, HVX_Vector Vv, HVX_VectorPred* Qp)**<br>
> **HVX_Vector Q6_Vw_vsub_VwVwQ_carry(HVX_Vector Vu, HVX_Vector Vv, HVX_VectorPred* Qp)**<br>


- Vector -> VectorPair

**HVX_VectorPair Q6_Wh_vadd_VubVub(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
相加，结果存入更大的数中。（需要注意，存入的顺序是奇偶分离的）
Vdd.v[0].w[i] = Vu.w[i].h[0] + Vv.w[i].h[0];  
Vdd.v[1].w[i] = Vu.w[i].h[1] + Vv.w[i].h[1];
```

| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res.lo(int16_t) | 0 | 4 | 8 | 12 | 16 | 20 | 24 | 28 | ... |
| res.hi(int16_t) | 2 | 6 | 10 | 14 | 18 | 22 | 26 | 30 | ... |

> **Other Instructions:**<br>
**HVX_VectorPair Q6_Wh_vadd_VubVub(HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_VectorPair Q6_Ww_vadd_VhVh(  HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_VectorPair Q6_Ww_vadd_VuhVuh(HVX_Vector Vu, HVX_Vector Vv)**<br>
> 不存在b + b 到 h的指令 :(<br>

- VectorPair -> VectorPair
```cpp
VectorPair的加法，与Vector的加法类似。（区分sat和非sat模式）
```

**HVX_VectorPair Q6_Wb_vadd_WbWb(HVX_VectorPair Vuu, HVX_VectorPair Vvv)**

| Vuu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vvv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res_pair(uint8_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | ... |

> **Other Instructions:**<br>
> **HVX_VectorPair Q6_Wb_vadd_WbWb(        HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wb_vadd_WbWb_sat(    HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wh_vadd_WhWh(        HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wh_vadd_WhWh_sat(    HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Ww_vadd_WwWw(        HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Ww_vadd_WwWw_sat(    HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wub_vadd_WubWub_sat( HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wuh_vadd_WuhWuh_sat( HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wuw_vadd_WuwWuw_sat( HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>

**HVX_VectorPair Q6_Wh_vaddacc_WhVubVub(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint8_t) | 0 | 1 | 1 | 255 | 255 | 255 | 127 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 0 | 1 | 0 | 255 | 1 | 128 | 128 | ... |
| Vxx.lo(int16_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
| Vxx.hi(int16_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
| res_pair.lo(int16_t) | 1 | 2 | 511 | 255 | 1 | 0 | 1 | 0 | ... |
| res_pair.hi(int16_t) | 2 | 255 | 257 | 256 | 1 | 0 | 1 | 0 | ... |

**HVX_VectorPair Q6_Ww_vaddacc_WwVhVh(  HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)**
| Vu(int16_t) | 0 | 1 | 32767 | 32767 | 32767 | -32768 | -32768 | -32768 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 0 | 0 | 1 | 32767 | -32768 | 1 | 32767 | -32768 | ... |
| Vxx.lo(int32_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
| Vxx.hi(int32_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
| res_pair.lo(int32_t) | 1 | 32769 | 0 | 0 | 3 | 3 | 3 | 3 | ... |
| res_pair.hi(int32_t) | 2 | 65535 | -32766 | -65535 | 1 | 1 | 1 | 1 | ... |

**HVX_VectorPair Q6_Ww_vaddacc_WwVuhVuh(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint16_t) | 0 | 1 | 1 | 65535 | 65535 | 65535 | 32767 | 32768 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 0 | 1 | 0 | 65535 | 1 | 32768 | 32768 | ... |
| Vxx.lo(int32_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
| Vxx.hi(int32_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
| res_pair.lo(int32_t) | 1 | 3 | 131071 | 65536 | 3 | 3 | 3 | 3 | ... |
| res_pair.hi(int32_t) | 2 | 65536 | 65537 | 65537 | 1 | 1 | 1 | 1 | ... |

<br>
<br>

### sub
- 1 Vector -> Vector

**HVX_Vector Q6_Vub_vsub_VubVub_sat(HVX_Vector Vu, HVX_Vector Vv)**
> Vd.ub[i] = sat_u8(Vu.ub[i] - Vv.ub[i]) 带饱和的无符号减法 

| Vu(uint8_t) | 0 | 1 | 0 | 255 | 255 | 255 | 127 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 0 | 1 | 0 | 255 | 1 | 128 | 128 | ... |
| res(uint8_t) | 0 | 1 | 0 | 255 | 0 | 254 | 0 | 0 | ... |

**HVX_Vector Q6_Vb_vsub_VbVb_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.b[i] = sat_s8(Vu.b[i] - Vv.b[i]) 带饱和的有符号减法 
```

| Vu(int8_t) | 0 | 1 | 1 | -1 | -1 | -1 | 127 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 0 | 0 | 1 | 0 | -1 | 1 | -128 | -128 | ... |
| res(int8_t) | 0 | 1 | 0 | -1 | 0 | -2 | 127 | 0 | ... |

> **Other Instructions:**<br>
**HVX_Vector Q6_Vh_vsub_VhVh_sat(   HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_Vector Q6_Vuh_vsub_VuhVuh_sat(HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_Vector Q6_Vw_vsub_VwVw_sat(   HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_Vector Q6_Vuw_vsub_VuwVuw_sat(HVX_Vector Vu, HVX_Vector Vv)**<br>
     

**HVX_Vector Q6_Vb_vsub_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.ub[i] = Vu.ub[i] - Vv.ub[i] 带饱和的有符号减法, b和ub的行为一致。
```

| Vu(uint8_t) | 0 | 1 | 1 | 255 | 255 | 255 | 127 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 0 | 1 | 0 | 255 | 1 | 128 | 128 | ... |
| res(uint8_t) | 0 | 1 | 0 | 255 | 0 | 254 | 255 | 0 | ... |

> **Other Instructions**:<br>
**HVX_Vector Q6_Vh_vsub_VhVh(HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_Vector Q6_Vw_vsub_VwVw(HVX_Vector Vu, HVX_Vector Vv)**<br>
> 同加法, 没有无符号整型的非饱和减法, 可以用有符号整型的非饱和减法代替<br>
     

**HVX_Vector Q6_Vub_vsub_VubVb_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.ub[i] = sat_u8(Vu.ub[i] = Vv.b[i])  无符号ub减去有符号b，饱和后得到ub。（只有ub的指令，无对应的uh和uw指令 :( 
```
 
| Vu(uint8_t) | 0 | 1 | 0 | 1 | 255 | 0 | 255 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 0 | 0 | 1 | 1 | 0 | -1 | 1 | -128 | ... |
| res(uint8_t) | 0 | 1 | 0 | 0 | 255 | 1 | 254 | 255 | ... |

- 2 Vector -> VectorPair

**HVX_VectorPair Q6_Wh_vsub_VubVub(HVX_Vector Vu, HVX_Vector Vu)**
```cpp
ub相减得到h，无精度损失。注意：数据存入分奇偶
Vdd.v[0].h[i] = Vu.uh[i].ub[0] - Vv.uh[i].ub[0];
Vdd.v[1].h[i] = Vu.uh[i].ub[1] - Vv.uh[i].ub[1] ;
```

| Vu(uint8_t) | 0 | 1 | 0 | 255 | 255 | 255 | 127 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 0 | 1 | 0 | 255 | 1 | 128 | 128 | ... |
| res.lo(int16_t) | 0 | -1 | 0 | -1 | 0 | 0 | 0 | 0 | ... |
| res.hi(int16_t) | 1 | 255 | 254 | 0 | 0 | 0 | 0 | 0 | ... |

> **Other Instructions:**<br>
**HVX_VectorPair Q6_Ww_vsub_VhVh(HVX_Vector Vu, HVX_Vector Vv)**<br>
**HVX_VectorPair Q6_Ww_vsub_VuhVuh(HVX_Vector Vu, HVX_Vector Vv)**<br>
**不存在b - b = h的指令**<br>
 
- 3 VectorPair -> VectorPair 
```cpp
对应的VectorPair减法指令，与Vector类似，区分sat和非sat模式
```

**HVX_VectorPair Q6_Wub_vsub_WubWub_sat(HVX_VectorPair Vuu, HVX_VectorPair Vvv)**
| Vuu(uint8_t) | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vvv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res_pair(uint8_t) | 5 | 4 | 3 | 2 | 1 | 0 | 0 | 0 | ... |

> **Other Instructions**:<br>
**HVX_VectorPair Q6_Wb_vsub_WbWb(        HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wb_vsub_WbWb_sat(    HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wh_vsub_WhWh(        HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wh_vsub_WhWh_sat(    HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Ww_vsub_WwWw(        HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Ww_vsub_WwWw_sat(    HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wub_vsub_WubWub_sat( HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wuh_vsub_WuhWuh_sat( HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>
**HVX_VectorPair Q6_Wuw_vsub_WuwWuw_sat( HVX_VectorPair Vuu, HVX_VectorPair Vvv)**<br>

### abs
**HVX_Vector Q6_Vb_vabs_Vb(HVX_Vector Vu)** <br>
```cpp
注意:  abs得到的结果并不是sat的，所以如果Vu是-128，那么结果就是-128，而不是127
Vd.b[i] = abs(Vu.b[i])
```

| Vu(int8_t) | 0 | 1 | 2 | 127 | -1 | -2 | -64 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int8_t) | 0 | 1 | 2 | 127 | 1 | 2 | 64 | -128 | ... |

**HVX_Vector Q6_Vb_vabs_Vb_sat(HVX_Vector Vu)**
```cpp
Vd.b[i] = sat_s8(abs(Vu.b[i]))  饱和的abs指令
```

| Vu(int8_t) | 0 | 1 | 2 | 127 | -1 | -2 | -64 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int8_t) | 0 | 1 | 2 | 127 | 1 | 2 | 64 | 127 | ... |

> **Other Instructions：** <br> 
**HVX_Vector Q6_Vh_vabs_Vh(HVX_Vector Vu)** <br>
**HVX_Vector Q6_Vw_vabs_Vw(HVX_Vector Vu)** <br>
**HVX_Vector Q6_Vh_vabs_Vh_sat(HVX_Vector Vu)** <br>
**HVX_Vector Q6_Vw_vabs_Vw_sat(HVX_Vector Vu)** <br>

<br>
<br>

### abs diff 

**HVX_Vector Q6_Vub_vabsdiff_VubVub(HVX_Vector Vu,HVX_Vector Vv)**
```cpp
Vd.ub[i]  = abs(Vu.ub[i] - Vv.ub[i])   无符号数之间的绝对误差值
```

| Vu(uint8_t) | 0 | 1 | 2 | 127 | 255 | 254 | 64 | 128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 1 | 2 | 1 | 0 | 0 | 255 | 12 | 127 | ... |
| res(uint8_t) | 1 | 1 | 1 | 127 | 255 | 1 | 52 | 1 | ... |

**HVX_Vector Q6_Vuh_vabsdiff_VhVh(HVX_Vector Vu,HVX_Vector Vv)**
```cpp
Vd.uh[i] = abs (Vu.h[i] - Vv.h[i])  有符号数之间的绝对误差值
```

| Vu(int16_t) | 32767 | -32768 | 32767 | -32768 | 1 | 16  | 1 | 65408 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | -32768  | 32767 | 0     | 0     | 0 | 1  | -1 | 65531 | ... |
| res(uint16_t) | 65535 | 65535 | 32767 | 32768 | 1 | 15 | 2     | 123   | ... |

**Other Instructions:** <br>
> **HVX_Vector Q6_Vuh_vabsdiff_VuhVuh(HVX_Vector Vu,HVX_Vector Vv)** <br> 
> **HVX_Vector Q6_Vuw_vabsdiff_VwVw(HVX_Vector Vu,HVX_Vector Vv)**<br> 

<br>
<br>

### sad （Sum Absolute Diffence）

**HVX_VectorPair Q6_Wuw_vdsad_WuhRuh(HVX_VectorPair Vuu, Word32 Rt)**      
**Rt = 0xffff0001** (Rt.uh[0] = 0x0001, Rt.uh[1] = 0xFFFF)<br>
>
```cpp
将Vuu.uw[i]中的高低位uh 减去 Rt中的高低位uh， 得到的差值的绝对值加起来
Vdd.v[0].uw[i] = ABS(Vuu.v[0].uw[i].uh[0] - Rt.uh[0]);
Vdd.v[0].uw[i] += ABS(Vuu.v[0].uw[i].uh[1] - Rt.uh[1]);
Vdd.v[1].uw[i] = ABS(Vuu.v[0].uw[i].uh[1] - Rt.uh[0]);
Vdd.v[1].uw[i] += ABS(Vuu.v[1].uw[i].uh[0] - Rt.uh[1]) ;
```

> e.g.
> Vdd.v[0].uw[0] = abs(Vuu.v[0].uw[0].uh[0] - Rt.uh[0])  + abs(Vuu.v[0].uw[0].uh[1] - Rt.uh[1])
>   = abs(0 - 1) + abs(1 - 0xFFFF) = 0xFFFF = 65535  

| Vuu.lo(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vuu.hi(uint16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... |
| res_pair.lo(uint32_t) | 65535 | 65533 | 65533 | 65533 | 65533 | 65533 | 65533 | 65533 | ... |
| res_pair.hi(uint32_t) | 65471 | 65471 | 65471 | 65471 | 65471 | 65471 | 65471 | 65471 | ... |

**HVX_VectorPair Q6_Wuw_vdsadacc_WuwWuhRuh(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)**
**Rt = 0xffff0001**<br>
```cpp
在上面Q6_Wuw_vdsad_WuhRuh运算的基础上，再叠加一个Vxx
```

| Vxx.lo(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vxx.hi(uint32_t) | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | ... |
| Vuu.lo(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| Vuu.hi(uint32_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... |
| res_pair.lo(uint32_t) | 65535 | 65534 | 65535 | 65536 | 65537 | 65538 | 65539 | 65540 | ... |
| res_pair.hi(uint32_t) | 65503 | 65504 | 65505 | 65506 | 65507 | 65508 | 65509 | 65510 | ... |

**HVX_VectorPair Q6_Wuw_vrsad_WubRubI(HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1 = 0)**
**Rt = 0xffff0001**<br>
```cpp
请自行理解:(
Vdd.v[0].uw[i]  = ABS(Vuu.v[#u?1:0].uw[i].ub[0] - Rt.ub[(0- #u)&3]);
Vdd.v[0].uw[i] += ABS(Vuu.v[0 ].uw[i].ub[1] - Rt.ub[(1-#u)&3]);
Vdd.v[0].uw[i] += ABS(Vuu.v[0 ].uw[i].ub[2] - Rt.ub[(2-#u)&3]);
Vdd.v[0].uw[i] += ABS(Vuu.v[0 ].uw[i].ub[3] - Rt.ub[(3-#u)&3]);
Vdd.v[1].uw[i]   = ABS(Vuu.v[1 ].uw[i].ub[0] - Rt.ub[(2-#u)&3]);
Vdd.v[1].uw[i] += ABS(Vuu.v[1 ].uw[i].ub[1] - Rt.ub[(3-#u)&3]);
Vdd.v[1].uw[i] += ABS(Vuu.v[#u?1:0].uw[i].ub[2] - Rt.ub[(0 - #u)&3]);
Vdd.v[1].uw[i] += ABS(Vuu.v[0 ].uw[i].ub[3] - Rt.ub[(1-#u)&3]) ;
```

| Vuu.lo(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vuu.hi(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... |
| res_pair.lo(uint32_t) | 507 | 505 | 505 | 505 | 505 | 505 | 505 | 505 | ... |
| res_pair.hi(uint32_t) | 257 | 257 | 257 | 257 | 257 | 257 | 257 | 257 | ... |

**HVX_VectorPair Q6_Wuw_vrsad_WubRubI(HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1 = 1)**
**Rt = 0xffff0001**<br>

| Vuu.lo(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vuu.hi(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... |
| res_pair.lo(uint32_t) | 381 | 381 | 381 | 381 | 381 | 381 | 381 | 381 | ... |
| res_pair.hi(uint32_t) | 381 | 381 | 381 | 381 | 381 | 381 | 381 | 381 | ... |

**HVX_VectorPair Q6_Wuw_vrsadacc_WuwWubRubI(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1 = 0)**
**Rt = 0xffff0001**<br>
```cpp
在Q6_Wuw_vrsad_WubRubI的基础上再叠加一个Vxx
```

| Vxx.lo(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vxx.hi(uint32_t) | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | ... |
| Vuu.lo(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| Vuu.hi(uint32_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... |
| res_pair.lo(uint32_t) | 507 | 506 | 507 | 508 | 509 | 510 | 511 | 512 | ... |
| res_pair.hi(uint32_t) | 289 | 290 | 291 | 292 | 293 | 294 | 295 | 296 | ... |

<br>
<br>

### max

**HVX_Vector Q6_Vb_vmax_VbVb(HVX_Vector Vu,HVX_Vector Vv)**
```cpp
Vd.b[i] = max(Vu.b[i], Vv.b[i])  取两个向量最大值
```

| Vu(int8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 | ... |
| res(int8_t) | 4 | 4 | 4 | 4 | 4 | 5 | 6 | 7 | ... |

> **Other Instructions:** <br>
> **HVX_Vector Q6_Vh_vmax_VhVh(  HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vmax_VwVw(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vub_vmax_VubVub(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vuh_vmax_VuhVuh(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vhf_vmax_VhfVhf(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vsf_vmax_VsfVsf(HVX_Vector Vu,HVX_Vector Vv)** <br>
### min

**HVX_Vector Q6_Vb_vmin_VbVb(HVX_Vector Vu,HVX_Vector Vv)**
```cpp
Vd.b[i] = min(Vu.b[i], Vv.b[i])  取两个向量最小值
```
| Vu(int8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 | ... |
| res(int8_t) | 0 | 1 | 2 | 3 | 4 | 4 | 4 | 4 | ... |

> **Other Instructions:** <br>
> **HVX_Vector Q6_Vh_vmin_VhVh(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vmin_VwVw(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vub_vmin_VubVub(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vuh_vmin_VuhVuh(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vhf_vmin_VhfVhf(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vsf_vmin_VsfVsf(HVX_Vector Vu,HVX_Vector Vv)** <br>

<br>
<br>

### avg

**HVX_Vector Q6_Vb_vavg_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.b[i] = (Vu.b[i] + Vv.b[i]) / 2； 有符号数的相加除2； （ 需要注意负数的/2，本质就是右移一位）
```
| Vu(int8_t) | 0 | 1 | 127 | 127 | -1 | -5 | -128 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 2 | 1 | 127 | 126 | 3 | 2 | -1 | -128 | ... |
| res(int8_t) | 1 | 1 | 127 | 126 | 1 | -2 | -65 | -128 | ... |

**HVX_Vector Q6_Vb_vavg_VbVb_rnd(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.b[i] = (Vu.b[i] + Vv.b[i] + 1) / 2； 有符号数的相加，rounding后除2
```
| Vu(int8_t) | 0 | 1 | 127 | 127 | -1 | -5 | -128 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 2 | 1 | 127 | 126 | 3 | 2 | -1 | -128 | ... |
| res(int8_t) | 1 | 1 | 127 | 127 | 1 | -1 | -64 | -128 | ... |

**Other Instructions:** <br>
> **HVX_Vector Q6_Vh_vavg_VhVh(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vh_vavg_VhVh_rnd(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vavg_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vavg_VwVw_rnd(HVX_Vector Vu,HVX_Vector Vv** <br>

**HVX_Vector Q6_Vub_vavg_VubVub(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
Vd.ub[i] = (Vu.ub[i] + Vv.ub[i]) / 2； 无符号数的相加除2
```

| Vu(uint8_t) | 0 | 1 | 127 | 127 | 255 | 254 | 128 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 2 | 2 | 127 | 126 | 3 | 5 | 129 | 255 | ... |
| res(uint8_t) | 1 | 1 | 127 | 126 | 129 | 129 | 128 | 255 | ... |

**HVX_Vector Q6_Vub_vavg_VubVub_rnd(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.ub[i] = (Vu.ub[i] + Vv.ub[i] + 1) / 2； 无符号数的相加，rounding后除2
```

| Vu(uint8_t) | 0 | 1 | 127 | 127 | 255 | 254 | 128 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 2 | 2 | 127 | 126 | 3 | 5 | 129 | 255 | ... |
| res(uint8_t) | 1 | 2 | 127 | 127 | 129 | 130 | 129 | 255 | ... |

**Other Instructions:** <br>
> **HVX_Vector Q6_Vuh_vavg_VuhVuh(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vuh_vavg_VuhVuh_rnd(HVX_Vector Vu, HVX_Vector Vv** <br>
> **HVX_Vector Q6_Vuw_vavg_VuwVuw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vuw_vavg_VuwVuw_rnd(HVX_Vector Vu, HVX_Vector Vv** <br>

**HVX_Vector Q6_Vb_vnavg_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.b[i] = (Vu.b[i] - Vv.b[i])/2 ; 有符号数的相减除2
```
| Vu(int8_t) | 0 | 1 | -128 | 127 | 127 | 127 | -1 | 5 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 1 | -128 | 1 | -1 | -2 | -128 | -2 | 1 | ... |
| res(int8_t) | -1 | 64 | -65 | 64 | 64 | 127 | 0 | 2 | ... |

**HVX_Vector Q6_Vb_vnavg_VubVub(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vd.b[i] = (Vu.ub[i] - Vv.ub[i])/2 ; 无符号数的相减除2
```

| Vu(uint8_t) | 5 | 7 | 255 | 0 | 0 | 1 | 129 | 130 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 1 | 3 | 0 | 255 | 1 | 3 | 2 | 2 | ... |
| res(int8_t) | 2 | 2 | 127 | -128 | -1 | -1 | 63 | 64 | ... |

**Other Instructions:** <br>
> **HVX_Vector Q6_Vh_vnavg_VhVh(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vnavg_VwVw(HVX_Vector Vu,HVX_Vector Vv)** <br>

<br>
<br>

### Condacc

**HVX_Vector Q6_Vb_condacc_QnVbVb(HVX_VectorPred Qv, HVX_Vector Vx, HVX_Vector Vu)**
```cpp
Vx.b[i] = !Qv[i] ? Vx.b[i] + Vu.b[i] : Vx.b[i]
```

**HVX_Vector Q6_Vb_condnac_QnVbVb(HVX_VectorPred Qv, HVX_Vector Vx, HVX_Vector Vu)**
```cpp
Vx.b[i] = !Qv[i] ? Vx.b[i] - Vu.b[i] : Vx.b[i]
```

**HVX_Vector Q6_Vb_condacc_QVbVb(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)**
```cpp
Vx.b[i] = Qv[i] ? Vx.b[i] + Vu.b[i] : Vx.b[i]
```

**HVX_Vector Q6_Vb_condnac_QVbVb(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)**
```cpp
Vx.b[i] = Qv[i] ? Vx.b[i] - Vu.b[i] : Vx.b[i]
```

**Other Instructions:** <br>
> - HVX_Vector Q6_Vh_condacc_QnVhVh(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vh_condnac_QnVhVh(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vw_condacc_QnVwVw(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vw_condnac_QnVwVw(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vh_condacc_QVhVh(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vh_condnac_QVhVh(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vw_condacc_QVwVw(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)
> - HVX_Vector Q6_Vw_condnac_QVwVw(HVX_VectorPredQv, HVX_Vector Vx, HVX_Vector Vu)

<br>
<br>

### mpy

**HVX_VectorPair Q6_Wh_vmpy_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
两个Vector的b相乘，然后放到VectorPair的h中。 （奇偶分离）
Vdd.v[0].h[i] = (Vu.h[i].b[0] * Vv.h[i].b[0]) ;
Vdd.v[1].h[i] = (Vu.h[i].b[1] * Vv.h[i].b[1]) ;
```

| Vu(int8_t) | 0 | 1 | 2 | 127 | -1 | -2 | -128 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 0 | 1 | 2 | 127 | -1 | 1 | 127 | -128 | ... |
| res_pair.lo(int16_t) | 0 | 4 | 1 | -16256 | 0 | 0 | 0 | 0 | ... |
| res_pair.hi(int16_t) | 1 | 16129 | -2 | 16384 | 0 | 0 | 0 | 0 | ... |


HVX_VectorPair Q6_Wh_vmpy_VubVb(HVX_Vector Vu, HVX_Vector Vv)
```cpp
两个Vector的ub和b相乘，然后放到VectorPair的h中。 （奇偶分离）
Vdd.v[0].h[i] = (Vu.h[i].ub[0] * Vv.h[i].b[0]) ;
Vdd.v[1].h[i] = (Vu.h[i].ub[1] * Vv.h[i].b[1]) ;
```

| Vu(uint8_t) | 0 | 1 | 2 | 127 | 128 | 255 | 255 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 0 | 1 | 2 | 127 | -1 | 1 | 127 | -128 | ... |
| res_pair.lo(int16_t) | 0 | 4 | -128 | 32385 | 0 | 0 | 0 | 0 | ... |
| res_pair.hi(int16_t) | 1 | 16129 | 255 | -32640 | 0 | 0 | 0 | 0 | ... |


HVX_VectorPair Q6_Wuh_vmpy_VubVub(HVX_Vector Vu, HVX_Vector Vv)
```cpp
两个Vector的ub和ub相乘，然后放到VectorPair的uh中。 （奇偶分离）
Vdd.v[0].uh[i] = (Vu.h[i].ub[0] * Vv.h[i].ub[0]) ;
Vdd.v[1].uh[i] = (Vu.h[i].ub[1] * Vv.h[i].ub[1]) ;
```

| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 255 | ... |
| res_pair.lo(uint16_t) | 0 | 4 | 16 | 36 | 0 | 0 | 0 | 0 | ... |
| res_pair.hi(uint16_t) | 1 | 9 | 25 | 65025 | 0 | 0 | 0 | 0 | ... |

> **Other Instructions:** <br>
> **HVX_VectorPair Q6_Wuw_vmpy_VuhVuh(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_VectorPair Q6_Ww_vmpy_VhVh(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_VectorPair Q6_Ww_vmpy_VhVuh(HVX_Vector Vu, HVX_Vector Vv)**  <br>


**HVX_VectorPair Q6_Wh_vmpyacc_WhVbVb(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Q6_Wh_vmpy_VbVb之后再叠加一个Vxx
```

| Vu(int8_t) | 0 | 1 | 2 | 127 | -1 | -2 | -128 | -128 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | 0 | 1 | 2 | 127 | -1 | 1 | 127 | -128 | ... |
| Vxx.lo(int16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| Vxx.hi(int16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... |
| res_pair.lo(int16_t) | 0 | 5 | 3 | -16253 | 4 | 5 | 6 | 7 | ... |
| res_pair.hi(int16_t) | 65 | 16194 | 64 | 16451 | 68 | 69 | 70 | 71 | ... |

**Other Instructions:** <br>
> **HVX_VectorPair Q6_Wh_vmpyacc_WhVubVb(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPair Q6_Wuh_vmpyacc_WuhVubVub(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPair Q6_Wuw_vmpyacc_WuwVuhVuh(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPair Q6_Ww_vmpyacc_WwVhVh(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPair Q6_Ww_vmpyacc_WwVhVuh(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)** <br>
        

**HVX_VectorPair Q6_Wh_vmpy_VubRb(HVX_Vector Vu, Word32 Rt)**
**Rt = 0x04030201**

```cpp
Vu的ub与Rt对应的b相乘，存入Vdd
Vdd.v[0].h[i] = (Vu.uh[i].ub[0] * Rt.b[(2 * i + 0) % 4]);
Vdd.v[1].h[i] = (Vu.uh[i].ub[1] * Rt.b[(2 * i + 1) % 4]);
```

| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_pair.lo(int16_t) | 0 | 6 | 4 | 18 | 0 | 0 | 0 | 0 | ... |
| res_pair.hi(int16_t) | 2 | 12 | 10 | 1020 | 0 | 0 | 0 | 0 | ... |


**Other Instructions:** <br>
> **HVX_VectorPair Q6_Wuh_vmpy_VubRub(HVX_Vector Vu, Word32 Rt)**  <br>
> **HVX_VectorPair Q6_Wuw_vmpy_VuhRuh(HVX_Vector Vu, Word32 Rt)**  <br>
> **HVX_VectorPair Q6_Ww_vmpy_VhRh(HVX_Vector Vu, Word32 Rt)**     <br>
        

**HVX_VectorPair Q6_Wh_vmpyacc_WhVubRb(HVX_VectorPair Vxx, HVX_Vector Vu, Word32 Rt)**
**Rt = 0x04030201**
```cpp
Q6_Wh_vmpy_VubRb之后叠加Vxx
```

| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vxx.lo(int16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| Vxx.hi(int16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... |
| res_pair.lo(int16_t) | 0 | 7 | 6 | 21 | 4 | 5 | 6 | 7 | ... |
| res_pair.hi(int16_t) | 66 | 77 | 76 | 1087 | 68 | 69 | 70 | 71 | ... |

**Other Instructions:** <br>
> **HVX_VectorPair Q6_Wuh_vmpyacc_WuhVubRub(HVX_VectorPair Vxx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Wuw_vmpyacc_WuwVuhRuh(HVX_VectorPair Vxx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Ww_vmpyacc_WwVhRh(HVX_VectorPair Vxx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Ww_vmpyacc_WwVhRh_sat(HVX_VectorPair Vxx, HVX_Vector Vu, Word32 Rt)** <br>



**TODO:**
> **HVX_Vector Q6_Vh_vmpy_VhRh_s1_rnd_sat(HVX_Vector Vu, Word32 Rt)**  <br>
> **HVX_Vector Q6_Vh_vmpy_VhRh_s1_sat(HVX_Vector Vu, Word32 Rt)**  <br>
> **HVX_Vector Q6_Vh_vmpy_VhVh_s1_rnd_sat(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_Vector Q6_Vqf16_vmpy_VhfVhf(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_Vector Q6_Vqf16_vmpy_Vqf16Vhf(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_Vector Q6_Vqf16_vmpy_Vqf16Vqf16(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_Vector Q6_Vqf32_vmpy_Vqf32Vqf32(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_Vector Q6_Vqf32_vmpy_VsfVsf(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_VectorPair Q6_Wqf32_vmpy_VhfVhf(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_VectorPair Q6_Wqf32_vmpy_Vqf16Vhf(HVX_Vector Vu, HVX_Vector Vv)**  <br>
> **HVX_VectorPair Q6_Wqf32_vmpy_Vqf16Vqf16(HVX_Vector Vu, HVX_Vector Vv)**  <br>


**HVX_VectorPair Q6_Ww_v6mpy_WubWbI_h(HVX_VectorPair Vuu, HVX_VectorPair Vvv, Word32 Iu2)**

> 太复杂了, 不想测 :(


```c
PseudoCode:
c00=((  (((Vvv.v[0].uw[i].ub[3] >> (2 * 0)) & 3) << 8)  | Vvv.v[0].uw[i].ub[0]  ) << 6) >> 6;
c01=((  (((Vvv.v[0].uw[i].ub[3] >> (2 * 1)) & 3) << 8)  | Vvv.v[0].uw[i].ub[1]  ) << 6) >> 6;
c02=((  (((Vvv.v[0].uw[i].ub[3] >> (2 * 2)) & 3) << 8)  | Vvv.v[0].uw[i].ub[2]  ) << 6) >> 6;
c10=((  (((Vvv.v[1].uw[i].ub[3] >> (2 * 0)) & 3) << 8)  | Vvv.v[1].uw[i].ub[0]  ) << 6) >> 6;
c11=((  (((Vvv.v[1].uw[i].ub[3] >> (2 * 1)) & 3) << 8)  | Vvv.v[1].uw[i].ub[1]  ) << 6) >> 6;
c12=((  (((Vvv.v[1].uw[i].ub[3] >> (2 * 2)) & 3) << 8)  | Vvv.v[1].uw[i].ub[2]  ) << 6) >> 6;

if Iu3 == 0
    Vdd.v[1].w[i]  = (Vuu.v[1].uw[i].ub[3] * c10);
    Vdd.v[1].w[i] += (Vuu.v[1].uw[i].ub[1] * c11);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[3] * c12);
    Vdd.v[1].w[i] += (Vuu.v[1].uw[i].ub[2] * c00);
    Vdd.v[1].w[i] += (Vuu.v[1].uw[i].ub[0] * c01);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[2] * c02);
    // w1 = (c00*ub12 + c01*ub10 + c02*ub02 + c10*ub13 + c11*ub11 + c12*ub03)

    Vdd.v[0].w[i]  = (Vuu.v[1].uw[i].ub[2] * c10);
    Vdd.v[0].w[i] += (Vuu.v[1].uw[i].ub[0] * c11);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[2] * c12);
    // w0 = (c10*ub12 + c11*ub10 + c12*ub02)

if Iu3 == 1
    Vdd.v[1].w[i]  = (Vuu.v[1].uw[i].ub[3] * c00);
    Vdd.v[1].w[i] += (Vuu.v[1].uw[i].ub[1] * c01);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[3] * c02);
    // w1 = (c00*ub13 + c01*ub11 + c02*ub03)

    Vdd.v[0].w[i]  = (Vuu.v[1].uw[i].ub[3] * c10);
    Vdd.v[0].w[i] += (Vuu.v[1].uw[i].ub[1] * c11);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[3] * c12);
    Vdd.v[0].w[i] += (Vuu.v[1].uw[i].ub[2] * c00);
    Vdd.v[0].w[i] += (Vuu.v[1].uw[i].ub[0] * c01);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[2] * c02);
    // w0 = (c00*ub12 + c01*ub10 + c02*ub02 + c10*ub13 + c11*ub11 + c12*ub03)

if Iu3 == 2
    Vdd.v[1].w[i]  = (Vuu.v[1].uw[i].ub[1] * c10);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[3] * c11);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[1] * c12);
    Vdd.v[1].w[i] += (Vuu.v[1].uw[i].ub[0] * c00);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[2] * c01);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[0] * c02);
    // w1 = (c00*ub10 + c01*ub02 + c02*ub00 + c10*ub11 + c11*ub03 + c12*ub01)

    Vdd.v[0].w[i]  = (Vuu.v[1].uw[i].ub[0] * c10);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[2] * c11);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[0] * c12);
    // w0 = (c10*ub10 + c11*ub02 + c12*ub00)

if Iu3 == 3
    Vdd.v[1].w[i]  = (Vuu.v[1].uw[i].ub[1] * c00);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[3] * c01);
    Vdd.v[1].w[i] += (Vuu.v[0].uw[i].ub[1] * c02);
    // w1 = (c00*ub11 + c01*ub03 + c02*ub01)

    Vdd.v[0].w[i]  = (Vuu.v[1].uw[i].ub[1] * c10);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[3] * c11);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[1] * c12);
    Vdd.v[0].w[i] += (Vuu.v[1].uw[i].ub[0] * c00);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[2] * c01);
    Vdd.v[0].w[i] += (Vuu.v[0].uw[i].ub[0] * c02);
    // w0 = (c00*ub10 + c01*ub02 + c02*ub00 + c10*ub11 + c11*ub03 + c12*ub01)
```

**Other Instructions** <br>

> **HVX_VectorPair Q6_Ww_v6mpy_WubWbI_v(HVX_VectorPair Vuu, HVX_VectorPair Vvv, Word32 Iu2)** <br>
> **HVX_VectorPair Q6_Ww_v6mpyacc_WwWubWbI_h(HVX_VectorPair Vxx, HVX_VectorPair Vuu, HVX_VectorPair Vvv, Word32 Iu2)** <br>
> **HVX_VectorPair Q6_Ww_v6mpyacc_WwWubWbI_v(HVX_VectorPair Vxx, HVX_VectorPair Vuu, HVX_VectorPair Vvv, Word32 Iu2)** <br>




**HVX_Vector Q6_Vw_vmpye_VwVuh(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
res.w[i] = (Vu.w[i] * Vv.w[i].uh[0]) >> 16; 
```

| Vu(int32_t) | 0x12345678 | 0x12345678 | 0x12345678 | 0x12345678 | ... |
|---|---|---|---|---|---|
| Vv(int32_t) | 0x10001 | 0x100010 | 0x1000100 | 0x10001000 | ... |
| res(int32_t) | 0x1234 | 0x12345 | 0x123456 | 0x1234567 | ... |


**HVX_Vector Q6_Vuw_vmpye_VuhRuh(HVX_Vector Vu, Word32 Rt)**
**Rt = 0x12345678**

```cpp
res.uw[i] = (Vu.uw[i].uh[0] * Rt.uh[0])
```

| Vu(uint32_t) | 0x10001 | 0x100010 | 0x1000100 | 0x10001000 | ... |
|---|---|---|---|---|---|
| res(uint32_t) | 0x5678 | 0x56780 | 0x567800 | 0x5678000 | ... |


**HVX_Vector Q6_Vuw_vmpyeacc_VuwVuhRuh(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)**
**Rt = 0x12345678**

```cpp
res.uw[i] = (Vx.uw[i] + Vu.uw[i].uh[0] * Rt.uh[0])
```  

| Vx(uint32_t) | 0x1 | 0x1 | 0x1 | 0x1 | ... |
|---|---|---|---|---|---|
| Vu(uint32_t) | 0x10001 | 0x100010 | 0x1000100 | 0x10001000 | ... |
| res(uint32_t) | 0x5679 | 0x56781 | 0x567801 | 0x5678001 | ... |


**HVX_VectorPair Q6_W_vmpye_VwVuh(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
prod = Vu.w[i] * Vv.w[i].uh[0];
res_pair.v[0].w[i] = prod << 16; 
res_pair.v[1].w[i] = prod >> 16;
``` 

| Vu(int32_t) | 0x12345678 | 0x12345678 | 0x12345678 | 0x12345678 | ... |
|---|---|---|---|---|---|
| Vv(int32_t) | 0x10001 | 0x100010 | 0x1000100 | 0x10001000 | ... |
| res_pair.lo(int32_t) | 0x56780000 | 0x67800000 | 0x78000000 | 0x80000000 | ... |
| res_pair.hi(int32_t) | 0x1234 | 0x12345 | 0x123456 | 0x1234567 | ... |


**HVX_Vector Q6_Vw_vmpyo_VwVh_s1_sat(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
res.w[i] = sat_32{ (Vu.w[i] * Vv.w[i].h[1]) >> 15 };
```

| Vu(int32_t) | 0xffff8000 | 0xffff7000 | 0x80000000 | 0x7fffffff | ... |
|---|---|---|---|---|---|
| Vv(int32_t) | 0x10001 | 0x10001 | 0x80008000 | 0x80008000 | ... |
| res(int32_t) | 0xffffffff | 0xfffffffe | 0x7fffffff | 0x80000001 | ... |


**HVX_Vector Q6_Vw_vmpyoacc_VwVwVh_s1_sat_shift(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)**

```cpp
res.w[i] = sat_32{ (Vx.w[i] + Vu.w[i] * Vv.w[i].h[1]) >> 15 }; <br>
```

| Vx(int32_t) | 0x8000 | 0x8000 | 0x8000 | 0x8000 | ... |
|---|---|---|---|---|---|
| Vu(int32_t) | 0xffff8000 | 0xffff7000 | 0x80000000 | 0x7fffffff | ... |
| Vv(int32_t) | 0x10001 | 0x10001 | 0x80008000 | 0x80008000 | ... |
| res(int32_t) | 0x0 | 0xffffffff | 0x7fffffff | 0x80000002 | ... |


**HVX_Vector Q6_Vw_vmpyo_VwVh_s1_rnd_sat(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
res.w[i] = sat_32{  ( ((Vu.w[i] * Vv.w[i].h[1]) >> 14) + 1 ) >> 1  };
```

| Vu(int32_t) | 0xffff8000 | 0xffff7000 | 0x80000000 | 0x7fffffff | ... |
|---|---|---|---|---|---|
| Vv(int32_t) | 0x10001 | 0x10001 | 0x80008000 | 0x80008000 | ... |
| res(int32_t) | 0xffffffff | 0xffffffff | 0x7fffffff | 0x80000001 | ... |


**HVX_Vector Q6_Vw_vmpyoacc_VwVwVh_s1_rnd_sat_shift(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)**

```cpp
res.w[i] = sat_32{  ( ((Vx.w[i] + Vu.w[i] * Vv.w[i].h[1]) >> 14) + 1 ) >> 1  };
```

| Vx(int32_t) | 0x8000 | 0x8000 | 0x8000 | 0x8000 | ... |
|---|---|---|---|---|---|
| Vu(int32_t) | 0xffff8000 | 0xffff7000 | 0x80000000 | 0x7fffffff | ... |
| Vv(int32_t) | 0x10001 | 0x10001 | 0x80008000 | 0x80008000 | ... |
| res(int32_t) | 0x0 | 0x0 | 0x7fffffff | 0x80000002 | ... |

**HVX_VectorPair Q6_W_vmpyoacc_WVwVh(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)**
```cpp
prod = (Vu.w[i] * Vv.w[i].h[1]) + Vxx.v[1].w[i];
Vxx.v[1].w[i] = prod >> 16;
Vxx.v[0].w[i].h[0]=Vxx.v[0].w[i] >> 16;
Vxx.v[0].w[i].h[1]=prod & 0x0000ffff;
```

**HVX_Vector Q6_Vh_vdmpy_VubRb(HVX_Vector Vu, Word32 Rt)**
**Rt = 0x04030201**
```cpp
Vd.h[i] = Vu.ub[2*i] * Rt.b[2*i % 4] + Vu.ub[2*i + 1] * Rt.b[(2*i + 1) % 4]
```

| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int16_t) | 2 | 18 | 14 | 46 | 26 | 74 | 38 | 102 | ... |


**HVX_Vector Q6_Vh_vdmpyacc_VhVubRb(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)**
**Rt = 0x04030201**
```cpp
Vd.h[i] = Vx.h[i] + Vu.ub[2*i] * Rt.b[2*i % 4] + Vu.ub[2*i + 1] * Rt.b[(2*i + 1) % 4]
```

| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vx(int16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res(int16_t) | 2 | 19 | 16 | 49 | 30 | 79 | 44 | 109 | ... |



**Other Instructions:** <br>
> **HVX_Vector Q6_Vw_vdmpy_VhRb(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpyacc_VwVhRb(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpy_VhRh_sat(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpy_VhRuh_sat(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpyacc_VwVhRh_sat(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpyacc_VwVhRuh_sat(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpy_VhVh_sat(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vdmpyacc_VwVhVh_sat(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vdmpy_WhRh_sat(HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpyacc_VwWhRh_sat(HVX_Vector Vx, HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpy_WhRuh_sat(HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vdmpyacc_VwWhRuh_sat(HVX_Vector Vx, HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Wh_vdmpy_WubRb(HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Wh_vdmpyacc_WhWubRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Ww_vdmpy_WhRb(HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Ww_vdmpyacc_WwWhRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)** <br>


**HVX_Vector Q6_Vh_vmpyi_VhVh(HVX_Vector Vu, HVX_Vector Vv)**

```cpp
整数乘,没有饱和操作
res.h[i] = Vu.h[i] * Vv.h[i]; 
```

| Vu(int16_t) | 0 | 1 | 2 | -32768 | -32768 | 32767 | 32767 | -32768 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 0 | 1 | 3 | 2 | -1 | 32767 | -32768 | -32768 | ... |
| res(int16_t) | 0 | 1 | 6 | 0 | -32768 | 1 | -32768 | 0 | ... |


**HVX_Vector Q6_Vh_vmpyiacc_VhVhVh(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Q6_Vh_vmpyi_VhVh的基础上再叠加Vx
res.h[i] = Vx.h[i] + Vu.h[i] * Vv.h[i];
```

| Vx(int16_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int16_t) | 0 | 1 | 2 | -32768 | -32768 | 32767 | 32767 | -32768 | ... |
| Vv(int16_t) | 0 | 1 | 3 | 2 | -1 | 32767 | -32768 | -32768 | ... |
| res(int16_t) | 1 | 2 | 7 | 1 | -32767 | 2 | -32767 | 1 | ... |


**HVX_Vector Q6_Vh_vmpyi_VhRb(HVX_Vector Vu, Word32 Rt)**
```cpp
向量和固定值的整数乘法
res.h[i] = Vu.h[i] * Rt.b[i % 4];
```

**HVX_Vector Q6_Vh_vmpyiacc_VhVhRb(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)**
```cpp
Q6_Vh_vmpyi_VhRb后再叠加Vx
res.h[i] = Vx.h[i] + Vu.h[i] * Rt.b[i % 4];
```

**Other Instructions:**
> **HVX_Vector Q6_Vw_vmpyi_VwRb(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vmpyiacc_VwVwRb(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vmpyi_VwRub(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vmpyiacc_VwVwRub(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vmpyi_VwRh(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vmpyiacc_VwVwRh(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
    

**HVX_Vector Q6_Vw_vmpyie_VwVuh(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vu的w和Vv的偶数位uh乘法
res.w[i] = (Vu.w[i] * Vv.w[i].uh[0]);
```

**HVX_Vector Q6_Vw_vmpyio_VwVh(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vu的w和Vv的奇数位h乘法
res.w[i] = (Vu.w[i] * Vv.w[i].h[1]);
```


**HVX_Vector Q6_Vw_vmpyieacc_VwVwVuh(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Q6_Vw_vmpyie_VwVuh后再叠加Vx
res.w[i] = Vx.w[i] + (Vu.w[i] * Vv.w[i].uh[0]);
```

**HVX_Vector Q6_Vw_vmpyieacc_VwVwVh(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Q6_Vw_vmpyio_VwVh后再叠加Vx
res.w[i] = Vx.w[i] + (Vu.w[i] * Vv.w[i].h[0]);
```

**HVX_Vector Q6_Vw_vmpyieo_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vu.w[i]的低16位 x Vv.w[i]的高16位, 然后结果左移16位
res.w[i] = (Vu.w[i].h[0] * Vv.w[i].h[1]) << 16;
```

| Vu(int32_t) | 1 | 1 | 1 | 1 | 2 | 2 | 2 | 2 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int32_t) | 0x10000 | 0xffffffff | 0x80000000 | 0x7fffffff | 0x10000 | 0xffffffff | 0x80000000 | 0x7fffffff | ... |
| res(int32_t) | 0x10000 | 0xffff0000 | 0x80000000 | 0x7fff0000 | 0x20000 | 0xfffe0000 | 0x0 | 0xfffe0000 | ... |




**HVX_Vector Q6_Vuw_vrmpy_VubRub(HVX_Vector Vu, Word32 Rt)**
**Rt = 0x01020304**
```cpp
Vu.w[i]中的ub和常数Rt的ub对应相乘并相加 (无符号乘加)
res.uw[i] = (Vu.uw[i].ub[0] * Rt.ub[0] + 
             Vu.uw[i].ub[1] * Rt.ub[1] + 
             Vu.uw[i].ub[2] * Rt.ub[2] + 
             Vu.uw[i].ub[3] * Rt.ub[3]);
```

| Vu(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(uint32_t) | 30 | 70 | 110 | 150 | 30 | 70 | 110 | 150 | ... |


**HVX_Vector Q6_Vuw_vrmpyacc_VuwVubRub(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)**
**Rt = 0x01020304**

```cpp
Q6_Vuw_vrmpy_VubRub之后再叠加Vx
res.uw[i] = Vx.uw[i] + (Vu.uw[i].ub[0] * Rt.ub[0] + 
                        Vu.uw[i].ub[1] * Rt.ub[1] + 
                        Vu.uw[i].ub[2] * Rt.ub[2] + 
                        Vu.uw[i].ub[3] * Rt.ub[3]);
```

| Vx(uint32_t) | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
| res(uint32_t) | 31 | 71 | 111 | 151 | 31 | 71 | 111 | 151 | ... |


**HVX_Vector Q6_Vw_vrmpy_VubRb(HVX_Vector Vu, Word32 Rt)**
**Rt = 0xfffefdfc**
```cpp
Vu.w[i]中的ub和常数Rt的b对应相乘并相加  (有符号乘加)
res.w[i] = (Vu.w[i].ub[0] * Rt.b[0] + 
            Vu.w[i].ub[1] * Rt.b[1] + 
            Vu.w[i].ub[2] * Rt.b[2] + 
            Vu.w[i].ub[3] * Rt.b[3]);
```
> 

| Vu(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int32_t) | -30 | -70 | -110 | -150 | -30 | -70 | -110 | -150 | ... |


**HVX_Vector Q6_Vw_vrmpyacc_VwVubRb(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)**
**Rt = 0xfffefdfc**
```cpp
Q6_Vw_vrmpy_VubRb之后再叠加Vx
res.w[i] = Vx.w[i] + (Vu.w[i].ub[0] * Rt.b[0] + 
                      Vu.w[i].ub[1] * Rt.b[1] + 
                      Vu.w[i].ub[2] * Rt.b[2] + 
                      Vu.w[i].ub[3] * Rt.b[3]);
```

| Vx(uint32_t) | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
| res(int32_t) | -29 | -69 | -109 | -149 | -29 | -69 | -109 | -149 | ... |


**HVX_Vector Q6_Vuw_vrmpy_VubVub(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Vu.w[i]与Vv.w[i]的ub进行点积
res.uw[i] = (Vu.uw[i].ub[0] * Vv.uw[i].ub[0] + 
             Vu.uw[i].ub[1] * Vv.uw[i].ub[1] + 
             Vu.uw[i].ub[2] * Vv.uw[i].ub[2] + 
             Vu.uw[i].ub[3] * Vv.uw[i].ub[3]);
```

| Vu(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
| res(uint32_t) | 30 | 174 | 446 | 846 | 30 | 174 | 446 | 846 | ... |


**HVX_Vector Q6_Vuw_vrmpyacc_VuwVubVub(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)**
```cpp
Q6_Vuw_vrmpy_VubVub之后再叠加Vx
res.uw[i] = Vx.uw[i] + (Vu.uw[i].ub[0] * Vv.uw[i].ub[0] + 
                        Vu.uw[i].ub[1] * Vv.uw[i].ub[1] + 
                        Vu.uw[i].ub[2] * Vv.uw[i].ub[2] + 
                        Vu.uw[i].ub[3] * Vv.uw[i].ub[3]);
```

| Vx(uint32_t) | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | 0x1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint32_t) | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | 0x1020304 | 0x5060708 | 0x90a0b0c | 0xd0e0f10 | ... |
| res(uint32_t) | 31 | 175 | 447 | 847 | 31 | 175 | 447 | 847 | ... |

** Other Instructions:** <br>
> **HVX_Vector Q6_Vw_vrmpy_VbVb(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vrmpyacc_VwVbVb(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vrmpy_VubVb(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vrmpyacc_VwVubVb(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv)** <br>
    

**HVX_VectorPair Q6_Wuw_vrmpy_WubRubI(HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1)**

```cpp
for (i = 0; i < VELEM(32); i++)
{
    Vdd.v[0].uw[i]  = (Vuu.v[#u ? 1:0].uw[i].ub[0] * Rt.ub[(0-#u) & 0x3]);
    Vdd.v[0].uw[i] += (Vuu.v[0       ].uw[i].ub[1] * Rt.ub[(1-#u) & 0x3]);
    Vdd.v[0].uw[i] += (Vuu.v[0       ].uw[i].ub[2] * Rt.ub[(2-#u) & 0x3]);
    Vdd.v[0].uw[i] += (Vuu.v[0       ].uw[i].ub[3] * Rt.ub[(3-#u) & 0x3]);

    Vdd.v[1].uw[i]  = (Vuu.v[1       ].uw[i].ub[0] * Rt.ub[(2-#u) & 0x3]);
    Vdd.v[1].uw[i] += (Vuu.v[1       ].uw[i].ub[1] * Rt.ub[(3-#u) & 0x3]);
    Vdd.v[1].uw[i] += (Vuu.v[#u ? 1:0].uw[i].ub[2] * Rt.ub[(0-#u) & 0x3]);
    Vdd.v[1].uw[i] += (Vuu.v[0       ].uw[i].ub[3] * Rt.ub[(1-#u) & 0x3]);
}

// when Iu1 == 0
for (i = 0; i < VELEM(32); i++)
{
    Vdd.v[0].uw[i]  = (Vuu.v[0].uw[i].ub[0] * Rt.ub[0]);
    Vdd.v[0].uw[i] += (Vuu.v[0].uw[i].ub[1] * Rt.ub[1]);
    Vdd.v[0].uw[i] += (Vuu.v[0].uw[i].ub[2] * Rt.ub[2]);
    Vdd.v[0].uw[i] += (Vuu.v[0].uw[i].ub[3] * Rt.ub[3]);

    Vdd.v[1].uw[i]  = (Vuu.v[1].uw[i].ub[0] * Rt.ub[2]);
    Vdd.v[1].uw[i] += (Vuu.v[1].uw[i].ub[1] * Rt.ub[3]);
    Vdd.v[1].uw[i] += (Vuu.v[0].uw[i].ub[2] * Rt.ub[0]);
    Vdd.v[1].uw[i] += (Vuu.v[0].uw[i].ub[3] * Rt.ub[1]);
}

// when Iu1 == 1
for (i = 0; i < VELEM(32); i++)
{
    Vdd.v[0].uw[i]  = (Vuu.v[1].uw[i].ub[0] * Rt.ub[3]);
    Vdd.v[0].uw[i] += (Vuu.v[0].uw[i].ub[1] * Rt.ub[0]);
    Vdd.v[0].uw[i] += (Vuu.v[0].uw[i].ub[2] * Rt.ub[1]);
    Vdd.v[0].uw[i] += (Vuu.v[0].uw[i].ub[3] * Rt.ub[2]);

    Vdd.v[1].uw[i]  = (Vuu.v[1].uw[i].ub[0] * Rt.ub[1]);
    Vdd.v[1].uw[i] += (Vuu.v[1].uw[i].ub[1] * Rt.ub[2]);
    Vdd.v[1].uw[i] += (Vuu.v[1].uw[i].ub[2] * Rt.ub[3]);
    Vdd.v[1].uw[i] += (Vuu.v[0].uw[i].ub[3] * Rt.ub[0]);
}
```


**Other Instructions:** <br>
> **HVX_VectorPair Q6_Wuw_vrmpyacc_WuwWubRubI(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1)** <br>
> **HVX_VectorPair Q6_Ww_vrmpy_WubRbI(HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1)** <br>
> **HVX_VectorPair Q6_Ww_vrmpyacc_WwWubRbI(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt, Word32 Iu1)** <br>

**HVX_VectorPair Q6_Wh_vtmpy_WbRb(HVX_VectorPair Vuu, Word32 Rt)**

```c
for (i = 0; i < VELEM(16); i++) {
    Vdd.v[0].h[i]  = (Vuu.v[0].h[i].b[0] * Rt.b[(2*i    ) % 4]);
    Vdd.v[0].h[i] += (Vuu.v[0].h[i].b[1] * Rt.b[(2*i + 1) % 4]);
    Vdd.v[0].h[i] +=  Vuu.v[1].h[i].b[0];

    Vdd.v[1].h[i]  = (Vuu.v[0].h[i].b[1] * Rt.b[(2*i    ) % 4]);
    Vdd.v[1].h[i] += (Vuu.v[1].h[i].b[0] * Rt.b[(2*i + 1) % 4]);
    Vdd.v[1].h[i] +=  Vuu.v[1].h[i].b[1];
}
```
    
**Rt = 0xfeff0201**

| Vuu.lo(int16_t) | 0x201 | 0x201 | 0xfeff | 0xfeff | ... |
|---|---|---|---|---|---|
| Vuu.hi(int16_t) | 0x403 | 0x403 | 0xfcfd | 0xfcfd | ... |
| res_pair.lo(int16_t) | 8 | -2 | -8 | 2 | ... |
| res_pair.hi(int16_t) | 12 | -4 | -12 | 4 | ... |



**Other Instructions:** <br>
> **HVX_VectorPair Q6_Wh_vtmpyacc_WhWbRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)** <br>
>
> **HVX_VectorPair Q6_Wh_vtmpy_WubRb(HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Wh_vtmpyacc_WhWubRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)** <br>
>
> **HVX_VectorPair Q6_Ww_vtmpy_WhRb(HVX_VectorPair Vuu, Word32 Rt)** <br>
> **HVX_VectorPair Q6_Ww_vtmpyacc_WwWhRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)** <br>

### Mpa
**HVX_Vector Q6_Vh_vmpa_VhVhVhPh_sat(HVX_Vector Vx, HVX_Vector Vu, Word64 Rtt)**
```cpp
Vx.h[i]= sat_16((  ( (Vx.h[i] * Vu.h[i]) <<  1) + 
                   (Rtt.h[(Vu.h[i] >> 14) & 0x3] << 15)   ) >> 16)
```

**HVX_Vector Q6_Vh_vmpa_VhVhVuhPuh_sat(HVX_Vector Vx, HVX_Vector Vu, Word64 Rtt)**
```cpp
Vx.h[i]= sat_16((  (Vx.h[i] * Vu.uh[i]) +
                   (Rtt.uh[((Vu.uh[i] >> 14) & 0x3)] << 15)  ) >> 16) ;
```
**HVX_Vector Q6_Vh_vmps_VhVhVuhPuh_sat(HVX_Vector Vx, HVX_Vector Vu, Word64 Rtt)**
```cpp
Vx.h[i]= sat_16((  (Vx.h[i] * Vu.uh[i]) -
                   (Rtt.uh[((Vu.uh[i] >> 14) & 0x3)] << 15)  ) >>16)
```

**HVX_VectorPair Q6_Wh_vmpa_WubRb(HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Vdd.v[0].h[i] = (Vuu.v[0].uh[i].ub[0] * Rt.b[0]) + 
                (Vuu.v[1].uh[i].ub[0] * Rt.b[1]);
Vdd.v[1].h[i] = (Vuu.v[0].uh[i].ub[1] * Rt.b[2]) + 
                (Vuu.v[1].uh[i].ub[1] * Rt.b[3]);
```
**HVX_VectorPair Q6_Wh_vmpaacc_WhWubRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Q6_Wh_vmpa_WubRb之后再叠加Vxx
Vxx.v[0].h[i] += (Vuu.v[0].uh[i].ub[0] * Rt.b[0]) + 
                 (Vuu.v[1].uh[i].ub[0] * Rt.b[1]);
Vxx.v[1].h[i] += (Vuu.v[0].uh[i].ub[1] * Rt.b[2]) + 
                 (Vuu.v[1].uh[i].ub[1] * Rt.b[3]);
```

**HVX_VectorPair Q6_Wh_vmpa_WubRub(HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Vdd.v[0].uh[i] = (Vuu.v[0].uh[i].ub[0] * Rt.ub[0]) + 
                 (Vuu.v[1].uh[i].ub[0] * Rt.ub[1]);
Vdd.v[1].uh[i] = (Vuu.v[0].uh[i].ub[1] * Rt.ub[2]) + 
                 (Vuu.v[1].uh[i].ub[1] * Rt.ub[3]);
```
**HVX_VectorPair Q6_Wh_vmpaacc_WhWubRub(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Q6_Wh_vmpa_WubRub之后再叠加Vxx
Vxx.v[0].uh[i] += (Vuu.v[0].uh[i].ub[0] * Rt.ub[0]) + 
                  (Vuu.v[1].uh[i].ub[0] * Rt.ub[1]);
Vxx.v[1].uh[i] += (Vuu.v[0].uh[i].ub[1] * Rt.ub[2]) + 
                  (Vuu.v[1].uh[i].ub[1] * Rt.ub[3]);
```

**HVX_VectorPair Q6_Ww_vmpa_WhRb(HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Vdd.v[0].w[i] = (Vuu.v[0].w[i].h[0] * Rt.b[0]) + 
                (Vuu.v[1].w[i].h[0] * Rt.b[1]);
Vdd.v[1].w[i] = (Vuu.v[0].w[i].h[1] * Rt.b[2]) + 
                (Vuu.v[1].w[i].h[1] * Rt.b[3]) ;
```
**HVX_VectorPair Q6_Ww_vmpaacc_WwWhRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Vxx.v[0].w[i] += (Vuu.v[0].w[i].h[0] * Rt.b[0]) + 
                 (Vuu.v[1].w[i].h[0] * Rt.b[1]);
Vxx.v[1].w[i] += (Vuu.v[0].w[i].h[1] * Rt.b[2]) + 
                 (Vuu.v[1].w[i].h[1] * Rt.b[3]);
```

**HVX_VectorPair Q6_Ww_vmpa_WuhRb(HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Vdd.v[0].w[i] = (Vuu.v[0].w[i].uh[0] * Rt.b[0]) + 
                (Vuu.v[1].w[i].uh[0] * Rt.b[1]);
Vdd.v[1].w[i] = (Vuu.v[0].w[i].uh[1] * Rt.b[2]) + 
                (Vuu.v[1].w[i].uh[1] * Rt.b[3]);
```

**HVX_VectorPair Q6_Ww_vmpaacc_WwWuhRb(HVX_VectorPair Vxx, HVX_VectorPair Vuu, Word32 Rt)**
```cpp
Q6_Ww_vmpa_WuhRb之后再叠加Vxx
Vxx.v[0].w[i] += (Vuu.v[0].w[i].uh[0] * Rt.b[0]) + 
                 (Vuu.v[1].w[i].uh[0] * Rt.b[1]);
Vxx.v[1].w[i] += (Vuu.v[0].w[i].uh[1] * Rt.b[2]) + 
                 (Vuu.v[1].w[i].uh[1] * Rt.b[3]);
```

**HVX_VectorPair Q6_Wh_vmpa_WubWb(HVX_VectorPair Vuu, HVX_VectorPair Vvv)**
```cpp
Vdd.v[0].h[i] = (Vuu.v[0].uh[i].ub[0] * Vvv.v[0].uh[i].b[0]) + 
                (Vuu.v[1].uh[i].ub[0] * Vvv.v[1].uh[i].b[0]);
Vdd.v[1].h[i] = (Vuu.v[0].uh[i].ub[1] * Vvv.v[0].uh[i].b[1]) + 
                (Vuu.v[1].uh[i].ub[1] * Vvv.v[1].uh[i].b[1]) ;
```

**HVX_VectorPair Q6_Wh_vmpa_WubWub(HVX_VectorPair Vuu, HVX_VectorPair Vvv)**
```cpp
Vdd.v[0].h[i] = (Vuu.v[0].uh[i].ub[0] * Vvv.v[0].uh[i].ub[0]) +
                (Vuu.v[1].uh[i].ub[0] * Vvv.v[1].uh[i].ub[0]);
Vdd.v[1].h[i] = (Vuu.v[0].uh[i].ub[1] * Vvv.v[0].uh[i].ub[1]) + 
                (Vuu.v[1].uh[i].ub[1] * Vvv.v[1].uh[i].ub[1]) ;
```

<br>
<br>

### shift

**HVX_Vector Q6_Vh_vasl_VhR(HVX_Vector Vu, Word32 Rt)** <br>
```cpp
常数项左移 (shift left)
Vd.h[i] = (Vu.h[i] << (Rt & (16-1)));
```
> 这里的向左移位就是简单的向左移位并在右侧补零, 操作不当会造成溢出,需要格外注意. 

**Rt = 1**
| Vu(int16_t) | 1 | 16383 | 16384 | 32767 | -1 | -16384 | -16385 | -32768 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int16_t) | 2 | 32766 | -32768 | -2 | -2 | -32768 | 32766 | 0 | ... |

**HVX_Vector Q6_Vh_vaslacc_VhVhR(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
```cpp
Q6_Vh_vasl_VhR之后叠加Vx (需考虑溢出情况)
Vd.h[i] = Vx.h[i] + (Vu.h[i] << (Rt & (16-1)));
```
**Rt = 1**

| Vx(int16_t) | 2 | 2 | 2 | 2 | -2 | -2 | -2 | -2 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int16_t) | 1 | 16383 | 16384 | 32767 | -1 | -16384 | -16385 | -32768 | ... |
| res(int16_t) | 4 | -32768 | -32766 | 0 | -4 | 32766 | 32764 | -2 | ... |


**HVX_Vector Q6_Vh_vasl_VhVh(HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Vd.h[i] = (sxt(4+1)->16 (Vv.h[i]) > 0) ? (Vu.h[i] << sxt(4+1)->16(Vv.h[i])) :
                                         (Vu.h[i] >> sxt(4+1)->16(Vv.h[i])) ;
sxt(4+1)->16: 是一种运算符, 具体指代不清楚. sxt应该代表有符号扩展 (signed extension)
```

| Vu(int16_t) | 2 | 2 | 1 | 1 | 1 | -1 | -1 | -1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 1 | -1 | 15 | 16 | -1 | 15 | 16 | -1 | ... |
| res(int16_t) | 4 | 1 | -32768 | 0 | 0 | -32768 | -1 | -1 | ... |


**Other Instructions:** <br>
> **HVX_Vector Q6_Vw_vasl_VwR(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vasl_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vaslacc_VwVwR(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
    


**HVX_Vector Q6_Vh_vasr_VhR(HVX_Vector Vu, Word32 Rt)** <br>
```cpp
常数项右移 (shift right)
Vd.h[i] = (Vu.h[i] >> (Rt & (16-1)));
```
**Rt = 1**

| Vu(int16_t) | 1 | 16383 | 16384 | 32767 | -1 | -16384 | -16385 | -32768 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int16_t) | 0 | 8191 | 8192 | 16383 | -1 | -8192 | -8193 | -16384 | ... |

**HVX_Vector Q6_Vh_vasracc_VhVhR(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
**Rt = 1**
```cpp
Q6_Vh_vasr_VhR之后再叠加Vx
Vd.h[i] = Vx.h[i] + (Vu.h[i] >> (Rt & (16-1)));
```

| Vx(int16_t) | 2 | 2 | 2 | 16385 | -2 | -2 | -2 | -16385 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int16_t) | 1 | 16383 | 16384 | 32767 | -1 | -16384 | -16385 | -32768 | ... |
| res(int16_t) | 2 | 8193 | 8194 | -32768 | -3 | -8194 | -8195 | 32767 | ... |


**HVX_Vector Q6_Vh_vasr_VhVh(HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Vd.h[i] = (sxt(4+1)->16(Vv.h[i]) > 0) ? (Vu.h[i] >> sxt(4+1)->16(Vv.h[i])):
                                        (Vu.h[i] << sxt(4+1)->16(Vv.h[i])) ;
}
```

| Vu(int16_t) | 2 | 2 | 1 | 1 | 1 | -1 | -1 | -1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 1 | -1 | 15 | 16 | -1 | 15 | 16 | -1 | ... |
| res(int16_t) | 1 | 4 | 0 | 0 | 2 | -1 | 0 | -2 | ... |



** Other Instructions: ** <br>
> **HVX_Vector Q6_Vw_vasr_VwR(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vasracc_VwVwR(HVX_Vector Vx, HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vw_vasr_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>
    


**HVX_Vector Q6_Vh_vlsr_VhVh(HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Vd.uh[i] = (sxt(4+1)->16(Vv.h[i]) > 0) ? (Vu.uh[i] >> sxt(4+1)->16(Vv.h[i])) : 
                                         (Vu.uh[i] << sxt(4+1)->16(Vv.h[i]));
```
| Vu(int16_t) | 1 | 16383 | 16384 | 32767 | -1 | -16384 | -16385 | -32768 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 1 | -1 | 15 | 16 | -1 | 15 | 16 | -1 | ... |
| res(int16_t) | 0 | 32766 | 0 | 0 | -2 | 1 | 0 | 0 | ... |

** Other Instructions: ** <br>
> **HVX_Vector Q6_Vw_vlsr_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>



**HVX_Vector Q6_Vub_vlsr_VubR(HVX_Vector Vu, Word32 Rt)** <br>
```cpp
Vu的ub右移
Vd.b[i] = Vu.ub[i] >> (Rt & 0x7);
```
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 127 | 128 | 254 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(Rt = 1, uint8_t) | 0 | 0 | 1 | 1 | 63 | 64 | 127 | 127 | ... |
| res(Rt = -1, uint8_t) | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | ... |


** Other Instructions: ** <br>
> **HVX_Vector Q6_Vuh_vlsr_VuhR(HVX_Vector Vu, Word32 Rt)** <br>
> **HVX_Vector Q6_Vuw_vlsr_VuwR(HVX_Vector Vu, Word32 Rt)** <br>

## Vector Permutation 

[更多讨论: HVX Permutation 介绍](https://xiaomi.f.mioffice.cn/docx/doxk4PRZvEfQCGpAw0dG7wLFcmh)

### valign 
> align:  对两个向量进行拼接<br> 
> lalign: 跟algin的方向相反<br> 

**HVX_Vector Q6_V_valign_VVI(HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**
Iu3: immediate value, 0~7,  here we set it as 7
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | ... | 127 | 128 | 129 | 130 | 131 | 132 | 133 | 134 |

**HVX_Vector Q6_V_valign_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
Rt: register value,  here, we set as 9
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | ... | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | ... | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | ... | 127 | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 |

**HVX_Vector Q6_V_vlalign_VVI(HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**
Iu3: immediate value, 0~7,  here we set it as 7
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 128 | ... | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 |

**HVX_Vector Q6_V_vlalign_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
Rt: register value,  here, we set as 9
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | ... | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | ... | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 128 | ... | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 |
### rotr 
> ror:    一个向量进行循环右移 (rotate right)<br> 

**HVX_Vector Q6_V_vror_VR(HVX_Vector Vu, Word32 Rt)**
Rt: register value,  here, we set as 3
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | ... | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| res(uint8_t) | 3 | 4 | 5 | 6 | 7 | 8 | ... | 125 | 126 | 127 | 0 | 1 | 2 |

**HVX_Vector Q6_Vuw_vrotr_VuwVuw(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint32_t) | ffff0000 | ffff | ff00ff00 | ff00ff | f0f0f0f | f0f0f0f0 | 1234567 | 89abcdef | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint32_t) | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 4 | ... |
| res(uint32_t) | ffff000 | f0000fff | ff00ff0 | f00ff00f | f0f0f0f0 | f0f0f0f | 70123456 | f89abcde | ... |
### shuffle 
> shuffle: 洗牌指令<br> 
> shuff: 向量内部的洗牌<br> 
> shuffe: 向量间的洗牌, 只洗偶数(even)<br> 
> shuffo: 向量间的洗牌, 只洗奇数(odd)<br> 
> shuffoe: (全都要)向量间的洗牌, 奇数和偶数都洗, 偶数放在低位, 奇数放在高位<br> 
> shuff_VVR: 更灵活的洗牌方式, 也比较复杂<br> 

**HVX_VectorPred Q6_Qb_vshuffe_QhQh(HVX_VectorPred Qs, HVX_VectorPred Qt)**
| Qs(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(uint8_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
| res_pred(uint8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |

**HVX_VectorPred Q6_Qh_vshuffe_QwQw(HVX_VectorPred Qs, HVX_VectorPred Qt)**
| Qs(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(uint8_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... |
| res_pred(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |

**HVX_Vector Q6_Vb_vshuff_Vb(HVX_Vector Vu)**
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| res(uint8_t) | 0 | 64 | 1 | 65 | 2 | 66 | 3 | 67 | ... | 60 | 124 | 61 | 125 | 62 | 126 | 63 | 127 |

**HVX_Vector Q6_Vh_vshuff_Vh(HVX_Vector Vu)**
| Vu(uint16_t) | 256 | 770 | 1284 | 1798 | 2312 | 2826 | 3340 | 3854 | ... | 29040 | 29554 | 30068 | 30582 | 31096 | 31610 | 32124 | 32638 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| res(uint16_t) | 256 | 16704 | 770 | 17218 | 1284 | 17732 | 1798 | 18246 | ... | 14648 | 31096 | 15162 | 31610 | 15676 | 32124 | 16190 | 32638 |

**HVX_Vector Q6_Vb_vshuffe_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 0 | 128 | 2 | 130 | 4 | 132 | 6 | 134 | ... | 120 | 248 | 122 | 250 | 124 | 252 | 126 | 254 |

**HVX_Vector Q6_Vb_vshuffo_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 1 | 129 | 3 | 131 | 5 | 133 | 7 | 135 | ... | 121 | 249 | 123 | 251 | 125 | 253 | 127 | 255 |

**HVX_Vector Q6_Vh_vshuffe_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res(uint16_t) | 0 | 64 | 2 | 66 | 4 | 68 | 6 | 70 | ... | 56 | 120 | 58 | 122 | 60 | 124 | 62 | 126 |

**HVX_Vector Q6_Vh_vshuffo_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res(uint16_t) | 1 | 65 | 3 | 67 | 5 | 69 | 7 | 71 | ... | 57 | 121 | 59 | 123 | 61 | 125 | 63 | 127 |

**HVX_VectorPair Q6_Wb_vshuffoe_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 128 | 2 | 130 | 4 | 132 | 6 | 134 | ... | 120 | 248 | 122 | 250 | 124 | 252 | 126 | 254 |
| res_pair.hi(uint8_t) | 1 | 129 | 3 | 131 | 5 | 133 | 7 | 135 | ... | 121 | 249 | 123 | 251 | 125 | 253 | 127 | 255 |

**HVX_VectorPair Q6_Wh_vshuffoe_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res_pair.lo(uint16_t) | 0 | 64 | 2 | 66 | 4 | 68 | 6 | 70 | ... | 56 | 120 | 58 | 122 | 60 | 124 | 62 | 126 |
| res_pair.hi(uint16_t) | 1 | 65 | 3 | 67 | 5 | 69 | 7 | 71 | ... | 57 | 121 | 59 | 123 | 61 | 125 | 63 | 127 |

![plot](./data/shuffle_instruction.png)

> 如上图所示,  vshuff_VVR做的大概是上面的操作, 通过来Rt来控制洗牌的粒度. (可以是1B/2B/4B/8B/16B/32B/64B)  (当然也可以是128B或更大, 这种情况下相当于做了个无效操作, 此时res.lo = Vv, res.hi = Vu)<br>
><br>
> 当然, 上面展示的也只是Rt为2的n次方时的情况, 我们这里记为 shuff(1), shuff(2), ...<br>
><br>
> 当Rt不是2的n次方时:<br>
> - shuff(3)   = shuff(1) + shuff(2)  : 相当于先做了一个shuff(1), 再做了一个shuff(2)<br>
> - shuff(11) = shuff(1) + shuff(2) + shuff(8) 同理, 先小后大进行拆分<br>
><br>
>   我们详细分析一下下面的情况:<br>
> - shuff(127) = shuff(1) + shuff(2) + ... + shuff(64) : 此时完成的是一次完美洗牌<br>
>   > [ABCDEFGH]  + [abcdefgh]  -> [AaBbCcDdEeFfGgHh]<br>
> - shuff(-1) = shuff(127), 因为内部的处理是通过bit位来进行判断的.<br>
><br>
> **总结:**<br>
> ​      Rt为2的n次方时, (1, 2, 4, 8, ...), 实现的是奇偶分离的洗牌, Rt为粒度;<br>
> ​      Rt为负的2的年n次方时(-1, -2, -4), 实现的是完美洗牌, |Rt|为粒度;<br>
> ​      其他使用情况可能需要一些理解进行灵活运用.<br>

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = 1)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 128 | 2 | 130 | 4 | 132 | 6 | 134 | 8 | 136 | 10 | 138 | 12 | 140 | 14 | 142 | 16 | 144 | 18 | 146 | 20 | 148 | 22 | 150 | 24 | 152 | 26 | 154 | 28 | 156 | 30 | 158 | 32 | 160 | 34 | 162 | 36 | 164 | 38 | 166 | 40 | 168 | 42 | 170 | 44 | 172 | 46 | 174 | 48 | 176 | 50 | 178 | 52 | 180 | 54 | 182 | 56 | 184 | 58 | 186 | 60 | 188 | 62 | 190 | ... | 64 | 192 | 66 | 194 | 68 | 196 | 70 | 198 | 72 | 200 | 74 | 202 | 76 | 204 | 78 | 206 | 80 | 208 | 82 | 210 | 84 | 212 | 86 | 214 | 88 | 216 | 90 | 218 | 92 | 220 | 94 | 222 | 96 | 224 | 98 | 226 | 100 | 228 | 102 | 230 | 104 | 232 | 106 | 234 | 108 | 236 | 110 | 238 | 112 | 240 | 114 | 242 | 116 | 244 | 118 | 246 | 120 | 248 | 122 | 250 | 124 | 252 | 126 | 254 |
| res_pair.hi(uint8_t) | 1 | 129 | 3 | 131 | 5 | 133 | 7 | 135 | 9 | 137 | 11 | 139 | 13 | 141 | 15 | 143 | 17 | 145 | 19 | 147 | 21 | 149 | 23 | 151 | 25 | 153 | 27 | 155 | 29 | 157 | 31 | 159 | 33 | 161 | 35 | 163 | 37 | 165 | 39 | 167 | 41 | 169 | 43 | 171 | 45 | 173 | 47 | 175 | 49 | 177 | 51 | 179 | 53 | 181 | 55 | 183 | 57 | 185 | 59 | 187 | 61 | 189 | 63 | 191 | ... | 65 | 193 | 67 | 195 | 69 | 197 | 71 | 199 | 73 | 201 | 75 | 203 | 77 | 205 | 79 | 207 | 81 | 209 | 83 | 211 | 85 | 213 | 87 | 215 | 89 | 217 | 91 | 219 | 93 | 221 | 95 | 223 | 97 | 225 | 99 | 227 | 101 | 229 | 103 | 231 | 105 | 233 | 107 | 235 | 109 | 237 | 111 | 239 | 113 | 241 | 115 | 243 | 117 | 245 | 119 | 247 | 121 | 249 | 123 | 251 | 125 | 253 | 127 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = 2)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 128 | 129 | 4 | 5 | 132 | 133 | 8 | 9 | 136 | 137 | 12 | 13 | 140 | 141 | 16 | 17 | 144 | 145 | 20 | 21 | 148 | 149 | 24 | 25 | 152 | 153 | 28 | 29 | 156 | 157 | 32 | 33 | 160 | 161 | 36 | 37 | 164 | 165 | 40 | 41 | 168 | 169 | 44 | 45 | 172 | 173 | 48 | 49 | 176 | 177 | 52 | 53 | 180 | 181 | 56 | 57 | 184 | 185 | 60 | 61 | 188 | 189 | ... | 64 | 65 | 192 | 193 | 68 | 69 | 196 | 197 | 72 | 73 | 200 | 201 | 76 | 77 | 204 | 205 | 80 | 81 | 208 | 209 | 84 | 85 | 212 | 213 | 88 | 89 | 216 | 217 | 92 | 93 | 220 | 221 | 96 | 97 | 224 | 225 | 100 | 101 | 228 | 229 | 104 | 105 | 232 | 233 | 108 | 109 | 236 | 237 | 112 | 113 | 240 | 241 | 116 | 117 | 244 | 245 | 120 | 121 | 248 | 249 | 124 | 125 | 252 | 253 |
| res_pair.hi(uint8_t) | 2 | 3 | 130 | 131 | 6 | 7 | 134 | 135 | 10 | 11 | 138 | 139 | 14 | 15 | 142 | 143 | 18 | 19 | 146 | 147 | 22 | 23 | 150 | 151 | 26 | 27 | 154 | 155 | 30 | 31 | 158 | 159 | 34 | 35 | 162 | 163 | 38 | 39 | 166 | 167 | 42 | 43 | 170 | 171 | 46 | 47 | 174 | 175 | 50 | 51 | 178 | 179 | 54 | 55 | 182 | 183 | 58 | 59 | 186 | 187 | 62 | 63 | 190 | 191 | ... | 66 | 67 | 194 | 195 | 70 | 71 | 198 | 199 | 74 | 75 | 202 | 203 | 78 | 79 | 206 | 207 | 82 | 83 | 210 | 211 | 86 | 87 | 214 | 215 | 90 | 91 | 218 | 219 | 94 | 95 | 222 | 223 | 98 | 99 | 226 | 227 | 102 | 103 | 230 | 231 | 106 | 107 | 234 | 235 | 110 | 111 | 238 | 239 | 114 | 115 | 242 | 243 | 118 | 119 | 246 | 247 | 122 | 123 | 250 | 251 | 126 | 127 | 254 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = 3)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 128 | 1 | 129 | 4 | 132 | 5 | 133 | 8 | 136 | 9 | 137 | 12 | 140 | 13 | 141 | 16 | 144 | 17 | 145 | 20 | 148 | 21 | 149 | 24 | 152 | 25 | 153 | 28 | 156 | 29 | 157 | 32 | 160 | 33 | 161 | 36 | 164 | 37 | 165 | 40 | 168 | 41 | 169 | 44 | 172 | 45 | 173 | 48 | 176 | 49 | 177 | 52 | 180 | 53 | 181 | 56 | 184 | 57 | 185 | 60 | 188 | 61 | 189 | ... | 64 | 192 | 65 | 193 | 68 | 196 | 69 | 197 | 72 | 200 | 73 | 201 | 76 | 204 | 77 | 205 | 80 | 208 | 81 | 209 | 84 | 212 | 85 | 213 | 88 | 216 | 89 | 217 | 92 | 220 | 93 | 221 | 96 | 224 | 97 | 225 | 100 | 228 | 101 | 229 | 104 | 232 | 105 | 233 | 108 | 236 | 109 | 237 | 112 | 240 | 113 | 241 | 116 | 244 | 117 | 245 | 120 | 248 | 121 | 249 | 124 | 252 | 125 | 253 |
| res_pair.hi(uint8_t) | 2 | 130 | 3 | 131 | 6 | 134 | 7 | 135 | 10 | 138 | 11 | 139 | 14 | 142 | 15 | 143 | 18 | 146 | 19 | 147 | 22 | 150 | 23 | 151 | 26 | 154 | 27 | 155 | 30 | 158 | 31 | 159 | 34 | 162 | 35 | 163 | 38 | 166 | 39 | 167 | 42 | 170 | 43 | 171 | 46 | 174 | 47 | 175 | 50 | 178 | 51 | 179 | 54 | 182 | 55 | 183 | 58 | 186 | 59 | 187 | 62 | 190 | 63 | 191 | ... | 66 | 194 | 67 | 195 | 70 | 198 | 71 | 199 | 74 | 202 | 75 | 203 | 78 | 206 | 79 | 207 | 82 | 210 | 83 | 211 | 86 | 214 | 87 | 215 | 90 | 218 | 91 | 219 | 94 | 222 | 95 | 223 | 98 | 226 | 99 | 227 | 102 | 230 | 103 | 231 | 106 | 234 | 107 | 235 | 110 | 238 | 111 | 239 | 114 | 242 | 115 | 243 | 118 | 246 | 119 | 247 | 122 | 250 | 123 | 251 | 126 | 254 | 127 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = 32)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 |
| res_pair.hi(uint8_t) | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = -1)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 128 | 1 | 129 | 2 | 130 | 3 | 131 | 4 | 132 | 5 | 133 | 6 | 134 | 7 | 135 | 8 | 136 | 9 | 137 | 10 | 138 | 11 | 139 | 12 | 140 | 13 | 141 | 14 | 142 | 15 | 143 | 16 | 144 | 17 | 145 | 18 | 146 | 19 | 147 | 20 | 148 | 21 | 149 | 22 | 150 | 23 | 151 | 24 | 152 | 25 | 153 | 26 | 154 | 27 | 155 | 28 | 156 | 29 | 157 | 30 | 158 | 31 | 159 | ... | 32 | 160 | 33 | 161 | 34 | 162 | 35 | 163 | 36 | 164 | 37 | 165 | 38 | 166 | 39 | 167 | 40 | 168 | 41 | 169 | 42 | 170 | 43 | 171 | 44 | 172 | 45 | 173 | 46 | 174 | 47 | 175 | 48 | 176 | 49 | 177 | 50 | 178 | 51 | 179 | 52 | 180 | 53 | 181 | 54 | 182 | 55 | 183 | 56 | 184 | 57 | 185 | 58 | 186 | 59 | 187 | 60 | 188 | 61 | 189 | 62 | 190 | 63 | 191 |
| res_pair.hi(uint8_t) | 64 | 192 | 65 | 193 | 66 | 194 | 67 | 195 | 68 | 196 | 69 | 197 | 70 | 198 | 71 | 199 | 72 | 200 | 73 | 201 | 74 | 202 | 75 | 203 | 76 | 204 | 77 | 205 | 78 | 206 | 79 | 207 | 80 | 208 | 81 | 209 | 82 | 210 | 83 | 211 | 84 | 212 | 85 | 213 | 86 | 214 | 87 | 215 | 88 | 216 | 89 | 217 | 90 | 218 | 91 | 219 | 92 | 220 | 93 | 221 | 94 | 222 | 95 | 223 | ... | 96 | 224 | 97 | 225 | 98 | 226 | 99 | 227 | 100 | 228 | 101 | 229 | 102 | 230 | 103 | 231 | 104 | 232 | 105 | 233 | 106 | 234 | 107 | 235 | 108 | 236 | 109 | 237 | 110 | 238 | 111 | 239 | 112 | 240 | 113 | 241 | 114 | 242 | 115 | 243 | 116 | 244 | 117 | 245 | 118 | 246 | 119 | 247 | 120 | 248 | 121 | 249 | 122 | 250 | 123 | 251 | 124 | 252 | 125 | 253 | 126 | 254 | 127 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = -2)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 128 | 129 | 2 | 3 | 130 | 131 | 4 | 5 | 132 | 133 | 6 | 7 | 134 | 135 | 8 | 9 | 136 | 137 | 10 | 11 | 138 | 139 | 12 | 13 | 140 | 141 | 14 | 15 | 142 | 143 | 16 | 17 | 144 | 145 | 18 | 19 | 146 | 147 | 20 | 21 | 148 | 149 | 22 | 23 | 150 | 151 | 24 | 25 | 152 | 153 | 26 | 27 | 154 | 155 | 28 | 29 | 156 | 157 | 30 | 31 | 158 | 159 | ... | 32 | 33 | 160 | 161 | 34 | 35 | 162 | 163 | 36 | 37 | 164 | 165 | 38 | 39 | 166 | 167 | 40 | 41 | 168 | 169 | 42 | 43 | 170 | 171 | 44 | 45 | 172 | 173 | 46 | 47 | 174 | 175 | 48 | 49 | 176 | 177 | 50 | 51 | 178 | 179 | 52 | 53 | 180 | 181 | 54 | 55 | 182 | 183 | 56 | 57 | 184 | 185 | 58 | 59 | 186 | 187 | 60 | 61 | 188 | 189 | 62 | 63 | 190 | 191 |
| res_pair.hi(uint8_t) | 64 | 65 | 192 | 193 | 66 | 67 | 194 | 195 | 68 | 69 | 196 | 197 | 70 | 71 | 198 | 199 | 72 | 73 | 200 | 201 | 74 | 75 | 202 | 203 | 76 | 77 | 204 | 205 | 78 | 79 | 206 | 207 | 80 | 81 | 208 | 209 | 82 | 83 | 210 | 211 | 84 | 85 | 212 | 213 | 86 | 87 | 214 | 215 | 88 | 89 | 216 | 217 | 90 | 91 | 218 | 219 | 92 | 93 | 220 | 221 | 94 | 95 | 222 | 223 | ... | 96 | 97 | 224 | 225 | 98 | 99 | 226 | 227 | 100 | 101 | 228 | 229 | 102 | 103 | 230 | 231 | 104 | 105 | 232 | 233 | 106 | 107 | 234 | 235 | 108 | 109 | 236 | 237 | 110 | 111 | 238 | 239 | 112 | 113 | 240 | 241 | 114 | 115 | 242 | 243 | 116 | 117 | 244 | 245 | 118 | 119 | 246 | 247 | 120 | 121 | 248 | 249 | 122 | 123 | 250 | 251 | 124 | 125 | 252 | 253 | 126 | 127 | 254 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = -3)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 128 | 2 | 130 | 1 | 129 | 3 | 131 | 4 | 132 | 6 | 134 | 5 | 133 | 7 | 135 | 8 | 136 | 10 | 138 | 9 | 137 | 11 | 139 | 12 | 140 | 14 | 142 | 13 | 141 | 15 | 143 | 16 | 144 | 18 | 146 | 17 | 145 | 19 | 147 | 20 | 148 | 22 | 150 | 21 | 149 | 23 | 151 | 24 | 152 | 26 | 154 | 25 | 153 | 27 | 155 | 28 | 156 | 30 | 158 | 29 | 157 | 31 | 159 | ... | 32 | 160 | 34 | 162 | 33 | 161 | 35 | 163 | 36 | 164 | 38 | 166 | 37 | 165 | 39 | 167 | 40 | 168 | 42 | 170 | 41 | 169 | 43 | 171 | 44 | 172 | 46 | 174 | 45 | 173 | 47 | 175 | 48 | 176 | 50 | 178 | 49 | 177 | 51 | 179 | 52 | 180 | 54 | 182 | 53 | 181 | 55 | 183 | 56 | 184 | 58 | 186 | 57 | 185 | 59 | 187 | 60 | 188 | 62 | 190 | 61 | 189 | 63 | 191 |
| res_pair.hi(uint8_t) | 64 | 192 | 66 | 194 | 65 | 193 | 67 | 195 | 68 | 196 | 70 | 198 | 69 | 197 | 71 | 199 | 72 | 200 | 74 | 202 | 73 | 201 | 75 | 203 | 76 | 204 | 78 | 206 | 77 | 205 | 79 | 207 | 80 | 208 | 82 | 210 | 81 | 209 | 83 | 211 | 84 | 212 | 86 | 214 | 85 | 213 | 87 | 215 | 88 | 216 | 90 | 218 | 89 | 217 | 91 | 219 | 92 | 220 | 94 | 222 | 93 | 221 | 95 | 223 | ... | 96 | 224 | 98 | 226 | 97 | 225 | 99 | 227 | 100 | 228 | 102 | 230 | 101 | 229 | 103 | 231 | 104 | 232 | 106 | 234 | 105 | 233 | 107 | 235 | 108 | 236 | 110 | 238 | 109 | 237 | 111 | 239 | 112 | 240 | 114 | 242 | 113 | 241 | 115 | 243 | 116 | 244 | 118 | 246 | 117 | 245 | 119 | 247 | 120 | 248 | 122 | 250 | 121 | 249 | 123 | 251 | 124 | 252 | 126 | 254 | 125 | 253 | 127 | 255 |

**HVX_VectorPair Q6_W_vshuff_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt = -32)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 |
| res_pair.hi(uint8_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | ... | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |


### deal

**HVX_Vector Q6_Vb_vdeal_Vb(HVX_Vector Vu)**
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| res(uint8_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | ... | 113 | 115 | 117 | 119 | 121 | 123 | 125 | 127 |

**HVX_Vector Q6_Vb_vdeale_VbVb(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | ... | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | ... | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 0 | 4 | 8 | 12 | 16 | 20 | 24 | 28 | 32 | 36 | 40 | 44 | 48 | 52 | 56 | 60 | 64 | 68 | 72 | 76 | 80 | 84 | 88 | 92 | 96 | 100 | 104 | 108 | 112 | 116 | 120 | 124 | 2 | 6 | 10 | 14 | 18 | 22 | 26 | 30 | ... | 224 | 228 | 232 | 236 | 240 | 244 | 248 | 252 | 130 | 134 | 138 | 142 | 146 | 150 | 154 | 158 | 162 | 166 | 170 | 174 | 178 | 182 | 186 | 190 | 194 | 198 | 202 | 206 | 210 | 214 | 218 | 222 | 226 | 230 | 234 | 238 | 242 | 246 | 250 | 254 |

**HVX_Vector Q6_Vh_vdeal_Vh(HVX_Vector Vu)**
| Vu(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| res(uint16_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | ... | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 |

> deal: 发牌. <br> 
> 可以想象成一副牌发给两个人的操作.<br>  
> vdeal:  一副牌均匀发给两个人, 那么一人都是偶数牌, 一人都是奇数牌;<br> 
> vdeale: 一副牌均匀发给两人人, 但是只发偶数牌(even), 所以一人手里拿到的牌的序列都是4k, 一人手里都是 2*(2k + 1)<br> 

> 当Rt是正的2的n次方时, deal_VVR的指令与deal_VV的指令是等价的, 我们记为 deal(Rt)<br>
> 当Rt不是正的2的n次方时, 或者为负数的时候, deal_VVR的指令同样可以拆解成多个2的n次方的deal指令, 但是顺序是从大到小的, 与shuff_VVR的指令顺序相反<br>
> e.g.<br>
> - deal(3) = deal(2) + deal(1) : 相当于先做了一个deal(2), 再做了一个deal(1)<br>
> - deal(11) = deal(8) + deal(2) + deal(1)<br>
> 因此, 当Rt为负数的时候, 结果也跟shuff_VVR的指令完全不一样. 具体见测试结果<br>
> **总结**:<br>
> vdeal_VVR 与 vshuff_VVR互为逆运算, 适用于Rt为任何值的时候.<br>

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =1)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 128 | 2 | 130 | 4 | 132 | 6 | 134 | 8 | 136 | 10 | 138 | 12 | 140 | 14 | 142 | 16 | 144 | 18 | 146 | 20 | 148 | 22 | 150 | 24 | 152 | 26 | 154 | 28 | 156 | 30 | 158 | 32 | 160 | 34 | 162 | 36 | 164 | 38 | 166 | 40 | 168 | 42 | 170 | 44 | 172 | 46 | 174 | 48 | 176 | 50 | 178 | 52 | 180 | 54 | 182 | 56 | 184 | 58 | 186 | 60 | 188 | 62 | 190 | ... | 64 | 192 | 66 | 194 | 68 | 196 | 70 | 198 | 72 | 200 | 74 | 202 | 76 | 204 | 78 | 206 | 80 | 208 | 82 | 210 | 84 | 212 | 86 | 214 | 88 | 216 | 90 | 218 | 92 | 220 | 94 | 222 | 96 | 224 | 98 | 226 | 100 | 228 | 102 | 230 | 104 | 232 | 106 | 234 | 108 | 236 | 110 | 238 | 112 | 240 | 114 | 242 | 116 | 244 | 118 | 246 | 120 | 248 | 122 | 250 | 124 | 252 | 126 | 254 |
| res_pair.hi(uint8_t) | 1 | 129 | 3 | 131 | 5 | 133 | 7 | 135 | 9 | 137 | 11 | 139 | 13 | 141 | 15 | 143 | 17 | 145 | 19 | 147 | 21 | 149 | 23 | 151 | 25 | 153 | 27 | 155 | 29 | 157 | 31 | 159 | 33 | 161 | 35 | 163 | 37 | 165 | 39 | 167 | 41 | 169 | 43 | 171 | 45 | 173 | 47 | 175 | 49 | 177 | 51 | 179 | 53 | 181 | 55 | 183 | 57 | 185 | 59 | 187 | 61 | 189 | 63 | 191 | ... | 65 | 193 | 67 | 195 | 69 | 197 | 71 | 199 | 73 | 201 | 75 | 203 | 77 | 205 | 79 | 207 | 81 | 209 | 83 | 211 | 85 | 213 | 87 | 215 | 89 | 217 | 91 | 219 | 93 | 221 | 95 | 223 | 97 | 225 | 99 | 227 | 101 | 229 | 103 | 231 | 105 | 233 | 107 | 235 | 109 | 237 | 111 | 239 | 113 | 241 | 115 | 243 | 117 | 245 | 119 | 247 | 121 | 249 | 123 | 251 | 125 | 253 | 127 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =2)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 128 | 129 | 4 | 5 | 132 | 133 | 8 | 9 | 136 | 137 | 12 | 13 | 140 | 141 | 16 | 17 | 144 | 145 | 20 | 21 | 148 | 149 | 24 | 25 | 152 | 153 | 28 | 29 | 156 | 157 | 32 | 33 | 160 | 161 | 36 | 37 | 164 | 165 | 40 | 41 | 168 | 169 | 44 | 45 | 172 | 173 | 48 | 49 | 176 | 177 | 52 | 53 | 180 | 181 | 56 | 57 | 184 | 185 | 60 | 61 | 188 | 189 | ... | 64 | 65 | 192 | 193 | 68 | 69 | 196 | 197 | 72 | 73 | 200 | 201 | 76 | 77 | 204 | 205 | 80 | 81 | 208 | 209 | 84 | 85 | 212 | 213 | 88 | 89 | 216 | 217 | 92 | 93 | 220 | 221 | 96 | 97 | 224 | 225 | 100 | 101 | 228 | 229 | 104 | 105 | 232 | 233 | 108 | 109 | 236 | 237 | 112 | 113 | 240 | 241 | 116 | 117 | 244 | 245 | 120 | 121 | 248 | 249 | 124 | 125 | 252 | 253 |
| res_pair.hi(uint8_t) | 2 | 3 | 130 | 131 | 6 | 7 | 134 | 135 | 10 | 11 | 138 | 139 | 14 | 15 | 142 | 143 | 18 | 19 | 146 | 147 | 22 | 23 | 150 | 151 | 26 | 27 | 154 | 155 | 30 | 31 | 158 | 159 | 34 | 35 | 162 | 163 | 38 | 39 | 166 | 167 | 42 | 43 | 170 | 171 | 46 | 47 | 174 | 175 | 50 | 51 | 178 | 179 | 54 | 55 | 182 | 183 | 58 | 59 | 186 | 187 | 62 | 63 | 190 | 191 | ... | 66 | 67 | 194 | 195 | 70 | 71 | 198 | 199 | 74 | 75 | 202 | 203 | 78 | 79 | 206 | 207 | 82 | 83 | 210 | 211 | 86 | 87 | 214 | 215 | 90 | 91 | 218 | 219 | 94 | 95 | 222 | 223 | 98 | 99 | 226 | 227 | 102 | 103 | 230 | 231 | 106 | 107 | 234 | 235 | 110 | 111 | 238 | 239 | 114 | 115 | 242 | 243 | 118 | 119 | 246 | 247 | 122 | 123 | 250 | 251 | 126 | 127 | 254 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =3)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 2 | 128 | 130 | 4 | 6 | 132 | 134 | 8 | 10 | 136 | 138 | 12 | 14 | 140 | 142 | 16 | 18 | 144 | 146 | 20 | 22 | 148 | 150 | 24 | 26 | 152 | 154 | 28 | 30 | 156 | 158 | 32 | 34 | 160 | 162 | 36 | 38 | 164 | 166 | 40 | 42 | 168 | 170 | 44 | 46 | 172 | 174 | 48 | 50 | 176 | 178 | 52 | 54 | 180 | 182 | 56 | 58 | 184 | 186 | 60 | 62 | 188 | 190 | ... | 64 | 66 | 192 | 194 | 68 | 70 | 196 | 198 | 72 | 74 | 200 | 202 | 76 | 78 | 204 | 206 | 80 | 82 | 208 | 210 | 84 | 86 | 212 | 214 | 88 | 90 | 216 | 218 | 92 | 94 | 220 | 222 | 96 | 98 | 224 | 226 | 100 | 102 | 228 | 230 | 104 | 106 | 232 | 234 | 108 | 110 | 236 | 238 | 112 | 114 | 240 | 242 | 116 | 118 | 244 | 246 | 120 | 122 | 248 | 250 | 124 | 126 | 252 | 254 |
| res_pair.hi(uint8_t) | 1 | 3 | 129 | 131 | 5 | 7 | 133 | 135 | 9 | 11 | 137 | 139 | 13 | 15 | 141 | 143 | 17 | 19 | 145 | 147 | 21 | 23 | 149 | 151 | 25 | 27 | 153 | 155 | 29 | 31 | 157 | 159 | 33 | 35 | 161 | 163 | 37 | 39 | 165 | 167 | 41 | 43 | 169 | 171 | 45 | 47 | 173 | 175 | 49 | 51 | 177 | 179 | 53 | 55 | 181 | 183 | 57 | 59 | 185 | 187 | 61 | 63 | 189 | 191 | ... | 65 | 67 | 193 | 195 | 69 | 71 | 197 | 199 | 73 | 75 | 201 | 203 | 77 | 79 | 205 | 207 | 81 | 83 | 209 | 211 | 85 | 87 | 213 | 215 | 89 | 91 | 217 | 219 | 93 | 95 | 221 | 223 | 97 | 99 | 225 | 227 | 101 | 103 | 229 | 231 | 105 | 107 | 233 | 235 | 109 | 111 | 237 | 239 | 113 | 115 | 241 | 243 | 117 | 119 | 245 | 247 | 121 | 123 | 249 | 251 | 125 | 127 | 253 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =16)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 |
| res_pair.hi(uint8_t) | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =-1)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | 64 | 66 | 68 | 70 | 72 | 74 | 76 | 78 | 80 | 82 | 84 | 86 | 88 | 90 | 92 | 94 | 96 | 98 | 100 | 102 | 104 | 106 | 108 | 110 | 112 | 114 | 116 | 118 | 120 | 122 | 124 | 126 | ... | 128 | 130 | 132 | 134 | 136 | 138 | 140 | 142 | 144 | 146 | 148 | 150 | 152 | 154 | 156 | 158 | 160 | 162 | 164 | 166 | 168 | 170 | 172 | 174 | 176 | 178 | 180 | 182 | 184 | 186 | 188 | 190 | 192 | 194 | 196 | 198 | 200 | 202 | 204 | 206 | 208 | 210 | 212 | 214 | 216 | 218 | 220 | 222 | 224 | 226 | 228 | 230 | 232 | 234 | 236 | 238 | 240 | 242 | 244 | 246 | 248 | 250 | 252 | 254 |
| res_pair.hi(uint8_t) | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | 65 | 67 | 69 | 71 | 73 | 75 | 77 | 79 | 81 | 83 | 85 | 87 | 89 | 91 | 93 | 95 | 97 | 99 | 101 | 103 | 105 | 107 | 109 | 111 | 113 | 115 | 117 | 119 | 121 | 123 | 125 | 127 | ... | 129 | 131 | 133 | 135 | 137 | 139 | 141 | 143 | 145 | 147 | 149 | 151 | 153 | 155 | 157 | 159 | 161 | 163 | 165 | 167 | 169 | 171 | 173 | 175 | 177 | 179 | 181 | 183 | 185 | 187 | 189 | 191 | 193 | 195 | 197 | 199 | 201 | 203 | 205 | 207 | 209 | 211 | 213 | 215 | 217 | 219 | 221 | 223 | 225 | 227 | 229 | 231 | 233 | 235 | 237 | 239 | 241 | 243 | 245 | 247 | 249 | 251 | 253 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =-2)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 4 | 5 | 8 | 9 | 12 | 13 | 16 | 17 | 20 | 21 | 24 | 25 | 28 | 29 | 32 | 33 | 36 | 37 | 40 | 41 | 44 | 45 | 48 | 49 | 52 | 53 | 56 | 57 | 60 | 61 | 64 | 65 | 68 | 69 | 72 | 73 | 76 | 77 | 80 | 81 | 84 | 85 | 88 | 89 | 92 | 93 | 96 | 97 | 100 | 101 | 104 | 105 | 108 | 109 | 112 | 113 | 116 | 117 | 120 | 121 | 124 | 125 | ... | 128 | 129 | 132 | 133 | 136 | 137 | 140 | 141 | 144 | 145 | 148 | 149 | 152 | 153 | 156 | 157 | 160 | 161 | 164 | 165 | 168 | 169 | 172 | 173 | 176 | 177 | 180 | 181 | 184 | 185 | 188 | 189 | 192 | 193 | 196 | 197 | 200 | 201 | 204 | 205 | 208 | 209 | 212 | 213 | 216 | 217 | 220 | 221 | 224 | 225 | 228 | 229 | 232 | 233 | 236 | 237 | 240 | 241 | 244 | 245 | 248 | 249 | 252 | 253 |
| res_pair.hi(uint8_t) | 2 | 3 | 6 | 7 | 10 | 11 | 14 | 15 | 18 | 19 | 22 | 23 | 26 | 27 | 30 | 31 | 34 | 35 | 38 | 39 | 42 | 43 | 46 | 47 | 50 | 51 | 54 | 55 | 58 | 59 | 62 | 63 | 66 | 67 | 70 | 71 | 74 | 75 | 78 | 79 | 82 | 83 | 86 | 87 | 90 | 91 | 94 | 95 | 98 | 99 | 102 | 103 | 106 | 107 | 110 | 111 | 114 | 115 | 118 | 119 | 122 | 123 | 126 | 127 | ... | 130 | 131 | 134 | 135 | 138 | 139 | 142 | 143 | 146 | 147 | 150 | 151 | 154 | 155 | 158 | 159 | 162 | 163 | 166 | 167 | 170 | 171 | 174 | 175 | 178 | 179 | 182 | 183 | 186 | 187 | 190 | 191 | 194 | 195 | 198 | 199 | 202 | 203 | 206 | 207 | 210 | 211 | 214 | 215 | 218 | 219 | 222 | 223 | 226 | 227 | 230 | 231 | 234 | 235 | 238 | 239 | 242 | 243 | 246 | 247 | 250 | 251 | 254 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =-3)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 4 | 2 | 6 | 8 | 12 | 10 | 14 | 16 | 20 | 18 | 22 | 24 | 28 | 26 | 30 | 32 | 36 | 34 | 38 | 40 | 44 | 42 | 46 | 48 | 52 | 50 | 54 | 56 | 60 | 58 | 62 | 64 | 68 | 66 | 70 | 72 | 76 | 74 | 78 | 80 | 84 | 82 | 86 | 88 | 92 | 90 | 94 | 96 | 100 | 98 | 102 | 104 | 108 | 106 | 110 | 112 | 116 | 114 | 118 | 120 | 124 | 122 | 126 | ... | 128 | 132 | 130 | 134 | 136 | 140 | 138 | 142 | 144 | 148 | 146 | 150 | 152 | 156 | 154 | 158 | 160 | 164 | 162 | 166 | 168 | 172 | 170 | 174 | 176 | 180 | 178 | 182 | 184 | 188 | 186 | 190 | 192 | 196 | 194 | 198 | 200 | 204 | 202 | 206 | 208 | 212 | 210 | 214 | 216 | 220 | 218 | 222 | 224 | 228 | 226 | 230 | 232 | 236 | 234 | 238 | 240 | 244 | 242 | 246 | 248 | 252 | 250 | 254 |
| res_pair.hi(uint8_t) | 1 | 5 | 3 | 7 | 9 | 13 | 11 | 15 | 17 | 21 | 19 | 23 | 25 | 29 | 27 | 31 | 33 | 37 | 35 | 39 | 41 | 45 | 43 | 47 | 49 | 53 | 51 | 55 | 57 | 61 | 59 | 63 | 65 | 69 | 67 | 71 | 73 | 77 | 75 | 79 | 81 | 85 | 83 | 87 | 89 | 93 | 91 | 95 | 97 | 101 | 99 | 103 | 105 | 109 | 107 | 111 | 113 | 117 | 115 | 119 | 121 | 125 | 123 | 127 | ... | 129 | 133 | 131 | 135 | 137 | 141 | 139 | 143 | 145 | 149 | 147 | 151 | 153 | 157 | 155 | 159 | 161 | 165 | 163 | 167 | 169 | 173 | 171 | 175 | 177 | 181 | 179 | 183 | 185 | 189 | 187 | 191 | 193 | 197 | 195 | 199 | 201 | 205 | 203 | 207 | 209 | 213 | 211 | 215 | 217 | 221 | 219 | 223 | 225 | 229 | 227 | 231 | 233 | 237 | 235 | 239 | 241 | 245 | 243 | 247 | 249 | 253 | 251 | 255 |

**HVX_VectorPair Q6_W_vdeal_VVR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt =-16)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | ... | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | ... | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | 136 | 137 | 138 | 139 | 140 | 141 | 142 | 143 | 160 | 161 | 162 | 163 | 164 | 165 | 166 | 167 | 168 | 169 | 170 | 171 | 172 | 173 | 174 | 175 | 192 | 193 | 194 | 195 | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | 204 | 205 | 206 | 207 | 224 | 225 | 226 | 227 | 228 | 229 | 230 | 231 | 232 | 233 | 234 | 235 | 236 | 237 | 238 | 239 |
| res_pair.hi(uint8_t) | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | ... | 144 | 145 | 146 | 147 | 148 | 149 | 150 | 151 | 152 | 153 | 154 | 155 | 156 | 157 | 158 | 159 | 176 | 177 | 178 | 179 | 180 | 181 | 182 | 183 | 184 | 185 | 186 | 187 | 188 | 189 | 190 | 191 | 208 | 209 | 210 | 211 | 212 | 213 | 214 | 215 | 216 | 217 | 218 | 219 | 220 | 221 | 222 | 223 | 240 | 241 | 242 | 243 | 244 | 245 | 246 | 247 | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
### pack 
> 这里的pack是指将两个向量合并成一个向量, 但是合并的方式不同;<br> 
> packe: 将两个向量的低位合并.(有half和word两种单位)<br> 
> packo: 将两个向量的高位合并.(有half和word两种单位)<br> 
> pack_sat: 向量的降级. (h -> b | h -> ub | w -> h | w -> uh)<br> 

**HVX_Vector Q6_Vb_vpacke_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 64
    Vd.ub[i]      = Vv.uh[i].ub[0];
    Vd.ub[i + 64] = Vu.uh[i].ub[0];
```

| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | ... | 240 | 242 | 244 | 246 | 248 | 250 | 252 | 254 |

**HVX_Vector Q6_Vb_vpacko_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 64
    Vd.ub[i]      = Vv.uh[i].ub[1];
    Vd.ub[i + 64] = Vu.uh[i].ub[1];
```

| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res(uint8_t) | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | ... | 241 | 243 | 245 | 247 | 249 | 251 | 253 | 255 |

**HVX_Vector Q6_Vh_vpacke_VwVw(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 32
    Vd.uh[i]      = Vv.uw[i].uh[0];
    Vd.uh[i + 32] = Vu.uw[i].uh[0];
```

| Vu(uint16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res(uint16_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | ... | 112 | 114 | 116 | 118 | 120 | 122 | 124 | 126 |

**HVX_Vector Q6_Vh_vpacko_VwVw(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 32
    Vd.uh[i]      = Vv.uw[i].uh[1];
    Vd.uh[i + 32] = Vu.uw[i].uh[1];
```

| Vu(uint16_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res(uint16_t) | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | ... | 113 | 115 | 117 | 119 | 121 | 123 | 125 | 127 |

**HVX_Vector Q6_Vb_vpack_VhVh_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 64
    Vd.b[i]      = sat8(Vv.h[i]);
    Vd.b[i + 64] = sat8(Vu.h[i]) ;
```
| Vu(int16_t) | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | ... | 124 | 125 | 126 | 127 | 128 | 129 | 130 | 131 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | -132 | -131 | -130 | -129 | -128 | -127 | -126 | -125 | ... | -76 | -75 | -74 | -73 | -72 | -71 | -70 | -69 |
| res(int8_t) | -128 | -128 | -128 | -128 | -128 | -127 | -126 | -125 | ... | 124 | 125 | 126 | 127 | 127 | 127 | 127 | 127 |

**HVX_Vector Q6_Vub_vpack_VhVh_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 64
    Vd.ub[i]      = usat8(Vv.h[i]);
    Vd.ub[i + 64] = usat8(Vu.h[i]);
```
| Vu(int16_t) | 196 | 197 | 198 | 199 | 200 | 201 | 202 | 203 | ... | 252 | 253 | 254 | 255 | 256 | 257 | 258 | 259 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | -4 | -3 | -2 | -1 | 0 | 1 | 2 | 3 | ... | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 |
| res(uint8_t) | 0 | 0 | 0 | 0 | 0 | 1 | 2 | 3 | ... | 252 | 253 | 254 | 255 | 255 | 255 | 255 | 255 |

**HVX_Vector Q6_Vh_vpack_VwVw_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 32
    Vd.h[i]      = sat16(Vv.w[i]);
    Vd.h[i + 32] = sat16(Vu.w[i]) ;
```
| Vu(int32_t) | 32740 | 32741 | 32742 | 32743 | 32744 | 32745 | 32746 | 32747 | ... | 32764 | 32765 | 32766 | 32767 | 32768 | 32769 | 32770 | 32771 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(int32_t) | -32772 | -32771 | -32770 | -32769 | -32768 | -32767 | -32766 | -32765 | ... | -32748 | -32747 | -32746 | -32745 | -32744 | -32743 | -32742 | -32741 |
| res(int16_t) | -32768 | -32768 | -32768 | -32768 | -32768 | -32767 | -32766 | -32765 | ... | 32764 | 32765 | 32766 | 32767 | 32767 | 32767 | 32767 | 32767 |

**HVX_Vector Q6_Vuh_vpack_VwVw_sat(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
for i = 0 : 32
    Vd.ub[i]      = usat16(Vv.w[i]);
    Vd.ub[i + 32] = usat16(Vu.w[i]);
```
| Vu(int32_t) | 65508 | 65509 | 65510 | 65511 | 65512 | 65513 | 65514 | 65515 | ... | 65532 | 65533 | 65534 | 65535 | 65536 | 65537 | 65538 | 65539 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(int32_t) | -4 | -3 | -2 | -1 | 0 | 1 | 2 | 3 | ... | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 |
| res(uint16_t) | 0 | 0 | 0 | 0 | 0 | 1 | 2 | 3 | ... | 65532 | 65533 | 65534 | 65535 | 65535 | 65535 | 65535 | 65535 |
### unpack 

**HVX_VectorPair Q6_Wuh_vunpack_Vub(HVX_Vector Vu)**
```cpp
for i = 0 : 128
    Vdd.uh[i] = Vu.ub[i];
```
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| res_pair(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |

other instructions:<br>
> **HVX_VectorPair Q6_Wh_vunpack_Vb(HVX_Vector Vu)**<br>
> **HVX_VectorPair Q6_Wuw_vunpack_Vuh(HVX_Vector Vu)**<br>
> **HVX_VectorPair Q6_Ww_vunpack_Vh(HVX_Vector Vu)**<br>

**HVX_VectorPair Q6_Wh_vunpackoor_WhVb(HVX_VectorPair Vxx, HVX_Vector Vu)**
```cpp
for i = 0 : 128
    Vdd.uh[i] = Vxx.uh[i] | (Vu.ub[i] << 8);
```
| Vxx(uint16_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... | 7fff | 7fff | 7fff | 7fff | 7fff | 7fff | 7fff | 7fff |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | ff | 0 | ff | 0 | ff | 0 | ff | 0 | ... | ff | 0 | ff | 0 | ff | 0 | ff | 0 |
| res_pair(uint16_t) | ff01 | 1 | ff01 | 1 | ff01 | 1 | ff01 | 1 | ... | ffff | 7fff | ffff | 7fff | ffff | 7fff | ffff | 7fff |

**Other Instruction:** <br>
**HVX_VectorPair Q6_Ww_vunpackoor_WwVh(HVX_VectorPair Vxx, HVX_Vector Vu)**

<br>
<br>

### Other
**HVX_VectorPair Q6_W_vcombine_VV(HVX_Vector Vu, HVX_Vector Vv)**
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.lo | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_pair.hi | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |

**HVX_Vector Q6_V_vdelta_VV(HVX_Vector Vu, HVX_Vector Vv)**
```
//PseudoCode:
for (offset = VWIDTH/2; (offset>>=1) > 0; )
{
    for (k = 0; k < VWIDTH; k++)
    {
        Vd.ub[k] = (Vv.ub[k] & offset) ? Vu.ub[k ^ offset] : Vu.ub[k]; 
    }
    // copy Vd to Vu 
    for (k = 0; k < VWIDTH; k++)
    {
        Vu.ub[k] = Vd.ub[k];
    }
}
```

**HVX_Vector Q6_V_vrdelta_VV(HVX_Vector Vu, HVX_Vector Vv)**
```
//PseudoCode:
for (offset = 1; offset < VWIDTH; offset <<= 1){
    for (k = 0; k < VWIDTH; k++) {
        Vd.ub[k] = (Vv.ub[k]&offset) ? Vu.ub[k ^ offset] : Vu.ub[k];
    }
    for (k = 0; k < VWIDTH; k++) {
        Vu.ub[k] = Vd.ub[k];
    }
}
```

**说明:**<br>"
> 1. Vu是输入向量，Vv是控制向量，Vd是输出向量<br>
> 2. vdelta和vrdelta的区别是, offset循环的方向不同个, delta是从大到小, rdelta是从小到大<br>
> 3. 核心代码是: Vd.ub[k] = (Vv.ub[k] & offset) ? Vu.ub[k ^ offset] : Vu.ub[k]; <br>
> > 我们这里仅讨论Vu的第一个元素 k=0 的情况, offset的值可能是1/2/4/8/16/32/64<br>
> > 当offset=4时, 0 ^ offset = 4, 4 ^ offest = 0. 此时idx0, 4是互相联系的, 同理(1,5)(2,6)(3,7)(8,12)...也都是互相联系的<br> 
> > 此时对应控制向量Vv.ub[0]的第三个bit, 如果是1, 则取Vu.ub[4], 否则取Vu.ub[0]<br>
> > 所以, Vv的每个ub控制对应的Vu的ub的取值, 而每个bit对应不同offset的取值行为<br>
> 4. Vv长度为VWIDTH(128), 每个ub中有7个bit可以进行有效控制(最高位128超出了VWIDTH, 无效), 所以, 共有128*2^7=16384种控制行为(存在多个组合导出同样的结果)<br>
> 5. delta指令和rdelta指令是互逆的, 前提是没有发生数据的拷贝, 只有数据的交换 <br>

下面我们来看一些可能的使用场景(以长度为8做例子来讲解):<br>
- 1. 向量反转<br>
> vector: [0123 4567] control_mask: [0b000, 0b000, 0b000, 0b000, 0b000, 0b000, 0b000, 0b000] // 初期状态 <br> 
> vector: [4567 0123] control_mask: [0b100, 0b100, 0b100, 0b100, 0b100, 0b100, 0b100, 0b100] // offset=4时, 全部元素都启动交换<br>
> vector: [6745 2301] control_mask: [0b110, 0b110, 0b110, 0b110, 0b110, 0b110, 0b110, 0b110] // offset=2时, 全部元素都启动交换<br>
> vector: [7654 3210] control_mask: [0b111, 0b111, 0b111, 0b111, 0b111, 0b111, 0b111, 0b111] // offset=1时, 全部元素都启动交换<br>
> 注: 我们这里的offset是从大到小, 所以应该调用delta指令, 当然用rdelta指令也是可以的.<br>
> 而且, 可以用类似的方法进行局部反转或者全局反转<br>

- 2. 复制元素<br>
> vector: [0123 4567] control_mask: [0b000, 0b000, 0b000, 0b000, 0b000, 0b000, 0b000, 0b000] // 初期状态 <br>
> vector: [0023 4567] control_mask: [0b000, 0b001, 0b000, 0b000, 0b000, 0b000, 0b000, 0b000] // offset=1时, 第2位元素启动复制<br>
> vector: [0000 4567] control_mask: [0b000, 0b001, 0b010, 0b010, 0b000, 0b000, 0b000, 0b000] // offset=2时, 3,4位元素启动复制<br>
> vector: [0000 0000] control_mask: [0b000, 0b001, 0b010, 0b010, 0b100, 0b100, 0b100, 0b100] // offset=4时, 5,6,7,8位元素启动复制<br>
> 注: 我们这里的offest是从小到大, 所以应该调用rdelta指令, 当然用delta指令也是可以的, 只不过顺序要调整一下. <br>
> 而且, 类似方法可以复制任何一个或任意一段元素<br>


    ```
    int ctrl_reverse[32] __attribute__((aligned(128))) = {
        0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff,
        0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff,
        0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff,
        0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff, 0x7fffffff,
    }
    Vv = *(HVX_Vector*)ctrl_reverse;
    HVX_Vector res = Q6_V_vdelta_VV(Vu, Vv);
    ```
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 255 | 255 | 255 | 127 | 255 | 255 | 255 | 127 | ... | 255 | 255 | 255 | 127 | 255 | 255 | 255 | 127 |
| res(uint8_t) | 255 | 254 | 253 | 252 | 251 | 250 | 249 | 248 | ... | 135 | 134 | 133 | 132 | 131 | 130 | 129 | 128 |

    ```
    int ctrl_replicate[32] __attribute__((aligned(128))) = {
        0x00010202, 0x04040404, 0x08080808, 0x08080808, 0x10101010, 0x10101010, 0x10101010, 0x10101010,
        0x20202020, 0x20202020, 0x20202020, 0x20202020, 0x20202020, 0x20202020, 0x20202020, 0x20202020,
        0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040,
        0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040, 0x40404040,
    }
    Vv = *(HVX_Vector*)ctrl_replicate;
    HVX_Vector res = Q6_V_vrdelta_VV(Vu, Vv);
    ```
| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... | 248 | 249 | 250 | 251 | 252 | 253 | 254 | 255 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 2 | 2 | 1 | 0 | 4 | 4 | 4 | 4 | ... | 64 | 64 | 64 | 64 | 64 | 64 | 64 | 64 |
| res(uint8_t) | 131 | 131 | 131 | 131 | 131 | 131 | 131 | 131 | ... | 131 | 131 | 131 | 131 | 131 | 131 | 131 | 131 |

**HVX_Vector Q6_Vw_vinsert_VwR(HVX_Vector Vu, Word32 Rt)** <br>
> replace first word into Rt<br>
> here, we set Rt to 0x7fffffff <br>

| Vu(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(uint32_t) | 2147483647 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |

**HVX_VectorPair Q6_W_vswap_QVV(HVX_VectorPred Qt, HVX_Vector Vu, HVX_Vector Vv)** <br>
> swap the elements of Vu and Vv according to Qt, put Vv into res_pair.lo, put Vu into res_pair.hi <br>

| Vu(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| Qt(uint8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
| res_pair.lo | 128 | 1 | 130 | 3 | 132 | 5 | 134 | 7 | ... |
| res_pair.hi | 0 | 129 | 2 | 131 | 4 | 133 | 6 | 135 | ... |


**HVX_VectorPair Q6_Ww_vasrinto_WwVwVw(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv)** <br>
> 不是非常理解这条指令的用途.
```cpp
// Pseudocode
for (i = 0; i < VELEM(32); i++) {
    shift  = (Vu.w[i] << 32);
    mask   = (((Vxx.v[0].w[i]) << 32) | Vxx.v[0].w[i]);
    lomask = (((1) << 32) - 1);
    count  = -(0x40 & Vv.w[i]) + (Vv.w[i] & 0x3f);
    result = (count == -0x40) ? 0 : (((count < 0) ? ((shift << -(count)) | (mask & (lomask << - (count)))) : 
                                                    ((shift >>   count)  | (mask & (lomask >>    count)))));
    Vxx.v[1].w[i] = ((result >> 32) & 0xffffffff);
    Vxx.v[0].w[i] = (result & 0xffffffff) ;
}
```

| Vxx.lo(uint32_t) | f0f0f0f0 | f0f0f0f0 | f0f0f0f0 | f0f0f0f0 | f0f0f0f0 | f0f0f0f0 | f0f0f0f0 | f0f0f0f0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint32_t) | ff00ff00 | ff00ff00 | ff00ff00 | ff00ff00 | ff00ff00 | ff0ff00 | ff00ff00 | ff00ff00 | ... |
| Vv(int32_t) | 0 | -4 | -16 | -32 | 4 | 16 | 32 | 63 | ... |
| res_pair.lo(uint32_t) | f0f0f0f0 | f0f0f0f0 | f0f00000 | 0 | f0f0f0 | ff00f0f0 | ff00ff00 | ffffffff | ... |
| res_pair.hi(uint32_t) | ff00ff00 | f00ff000 | ff00f0f0 | f0f0f0f0 | fff00ff0 | ff0 | ffffffff | ffffffff | ... |

## Logic Operation

### compare

**HVX VectorPred Q6_Q_vcmp_eq_VbVb(HVX_Vector Vu,HVX_Vector Vv)**
```cpp
Compare Equal
Vd.b[i] = (Vu.b[i] == Vv.b[i]) ? 1 : 0;
```
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | ... |

**HVX VectorPred Q6_Q_vcmp_gt_VbVb(HVX_Vector Vu,HVX_Vector Vv)**
```cpp
Compare Greater Than
Qd.bit[i] = (Vu.b[i] > Vv.b[i])
```
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 | ... |

**Other Instruction:** <br>
> **HVX VectorPred Q6_Q_vcmp_eq_VhVh(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_eq_VwVw(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VhVh(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VwVw(HVX_Vector Vu,HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VubVub(HVX_VectorVu, HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VuhVuh(HVX_VectorVu, HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VuwVuw(HVX_VectorVu, HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VhfVhf(HVX_VectorVu, HVX_Vector Vv)** <br>
> **HVX VectorPred Q6_Q_vcmp_gt_VsfVsf(HVX_VectorVu, HVX_Vector Vv)** <br>

**HVX VectorPred Q6_Q_vcmp_eqand_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Qd.bit[i] = Qx.bit[i] & (Vu.b[i] == Vv.b[i])
```
| Qx(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | ... |

**HVX VectorPred Q6_Q_vcmp_eqor_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Qd.bit[i] = Qx.bit[i] | (Vu.b[i] == Vv.b[i])
```

| Qx(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 1 | 0 | 1 | 1 | 1 | 0 | 1 | 0 | ... |

**HVX VectorPred Q6_Q_vcmp_eqxacc_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Qd.bit[i] = Qx.bit[i] ^ (Vu.b[i] == Vv.b[i])
```
| Qx(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 | ... |

**Other Instructions:** <br>
> **HVX_VectorPred Q6_Q_vcmp_eqand_QVhVh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_eqand_QVwVw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_eqor_QVhVh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_eqor_QVwVw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_eqxacc_QVhVh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_eqxacc_QVwVw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>

**HVX VectorPred Q6_Q_vcmp_gtand_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Qd.bit[i] = Qx.bit[i] & (Vu.b[i] > Vv.b[i])
```
| Qx(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | ... |

**HVX VectorPred Q6_Q_vcmp_gtor_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Qd.bit[i] = Qx.bit[i] | (Vu.b[i] > Vv.b[i])
```
| Qx(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 1 | 0 | 1 | 0 | 1 | 1 | 1 | 1 | ... |

**HVX VectorPred Q6_Q_vcmp_gtxacc_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
```cpp
Qd.bit[i] = Qx.bit[i] ^ (Vu.b[i] > Vv.b[i])
```

| Qx(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
| Vv(int8_t) | -2 | -1 | 0 | 0 | 1 | 1 | 2 | 3 | ... |
| res(int8_t) | 1 | 0 | 1 | 0 | 1 | 1 | 0 | 1 | ... |

**Other Instructions:** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVhVh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVhfVhf(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVsfVsf(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVubVub(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVuhVuh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVuwVuw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtand_QVwVw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVhVh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVhfVhf(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVsfVsf(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVubVub(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVuhVuh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVuwVuw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtor_QVwVw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVbVb(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVhVh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVhfVhf(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVsfVsf(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVubVub(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVuhVuh(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVuwVuw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_VectorPred Q6_Q_vcmp_gtxacc_QVwVw(HVX_VectorPred Qx,HVX_Vector Vu, HVX_Vector Vv)** <br>

### Bit Operation
- pred & pred 

**HVX_VectorPred Q6_Q_not_Q(HVX_VectorPred Qs)**
> !Qs 逻辑非

| Qs(int8_t) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_pred(int8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |

**HVX_VectorPred Q6_Q_and_QQn(HVX_VectorPred Qs,HVX_VectorPred Qt)**
> Qs & (!Qt) 逻辑与非

| Qs(int8_t) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(int8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
| res_pred(int8_t) | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | ... |

**HVX_VectorPred Q6_Q_and_QQ(HVX_VectorPred Qs,HVX_VectorPred Qt)**
> Qs & Qt 逻辑与

| Qs(int8_t) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(int8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
| res_pred(int8_t) | 0 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | ... |

**HVX_VectorPred Q6_Q_or_QQn(HVX_VectorPred Qs,HVX_VectorPred Qt)**
> Qs | (!Qt) 逻辑或非

| Qs(int8_t) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(int8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
| res_pred(int8_t) | 0 | 1 | 1 | 1 | 0 | 1 | 1 | 1 | ... |

**HVX_VectorPred Q6_Q_or_QQ(HVX_VectorPred Qs,HVX_VectorPred Qt)**
> Qs | Qt 逻辑或

| Qs(int8_t) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(int8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
| res_pred(int8_t) | 1 | 1 | 0 | 1 | 1 | 1 | 0 | 1 | ... |

**HVX_VectorPred Q6_Q_xor_QQ(HVX_VectorPred Qs,HVX_VectorPred Qt)**
> Qs ^ Qt 逻辑异或

| Qs(int8_t) | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Qt(int8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
| res_pred(int8_t) | 1 | 0 | 0 | 1 | 1 | 0 | 0 | 1 | ... |
- pred & vec 

**HVX_Vector Q6_V_vand_QnV(HVX_VectorPred Qt, HVX_Vector Vu)** <br>
> Qt为0时，res_vec.b[i] = Vu.b[i]，否则res_vec.b[i] = 0 <br>

| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res_vec(uint8_t) | 0 | 0 | 2 | 3 | 0 | 0 | 6 | 7 | ... |

**HVX_Vector Q6_V_vand_QV(HVX_VectorPred Qt, HVX_Vector Vu)** <br>
> Qt为0时，res_vec.b[i] = 0，否则res_vec.b[i] = Vu.b[i] <br>

| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res_vec(uint8_t) | 0 | 1 | 0 | 0 | 4 | 5 | 0 | 0 | ... |

**HVX_Vector Q6_V_vmux_QVV(HVX_VectorPred Qt, HVX_Vector Vu, HVX_Vector Vv)** <br>
> Qt为0时，res_vec.b[i] = Vv.b[i]，否则res_vec.b[i] = Vu.b[i] <br>

| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| Vv(uint8_t) | 128 | 129 | 130 | 131 | 132 | 133 | 134 | 135 | ... |
| res_vec(uint8_t) | 0 | 1 | 130 | 131 | 4 | 5 | 134 | 135 | ... |

**HVX_Vector Q6_V_vand_QnR(HVX_VectorPred Qt, Word32 Rt)**<br>
> Qt为0时，res_vec.b[i] = Rt.b[i & 0x3]，否则res_vec.b[i] = 0 <br>

Rt: 0x04030201<br>
| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_vec(uint8_t) | 0 | 0 | 3 | 4 | 0 | 0 | 3 | 4 | ... |

**HVX_Vector Q6_V_vand_QR(HVX_VectorPred Qt, Word32 Rt)**<br>
> Qt为0时，res_vec.b[i] = 0，否则res_vec.b[i] = Rt.b[i & 0x3] <br>

Rt: 0x04030201<br>
| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_vec(uint8_t) | 1 | 2 | 0 | 0 | 1 | 2 | 0 | 0 | ... |

**HVX_Vector Q6_V_vandor_VQnR(HVX_Vector Vu, HVX_VectorPred Qt, Word32 Rt)**<br>
> Qt为0时，res_vec.b[i] = Vu.b[i] | Rt.b[i & 0x3], 否则res_vec.b[i] = 0 <br>

Rt: 0x00f00fff<br>
| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res_vec(uint8_t) | 0 | 1 | 242 | 3 | 4 | 5 | 246 | 7 | ... |

**HVX_Vector Q6_V_vandor_VQR(HVX_Vector Vu, HVX_VectorPred Qt, Word32 Rt)**<br>
> Qt为1时，res_vec.b[i] = Vu.b[i] | Rt.b[i & 0x3], 否则res_vec.b[i] = 0 <br>

Rt: 0x00f00fff<br>
| Qt(uint8_t) | 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
| res_vec(uint8_t) | 255 | 15 | 2 | 3 | 255 | 15 | 6 | 7 | ... |

**HVX_VectorPred Q6_Q_vand_VR(HVX_Vector Vu, Word32 Rt)**<br>
> Vv.b[i] & Rt.b[i & 0x3] != 0时，res_pred.b[i] = 1，否则res_pred.b[i] = 0 <br>

Rt = Rt = 0xff0ff000 <br>
| Vu(uint8_t) | ff | f | f0 | 0 | 0 | f | f0 | ff | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_pred(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | ... |

**HVX_VectorPred Q6_Q_vandor_QVR(HVX_VectorPred Qx, HVX_Vector Vu, Word32 Rt)**<br>
> Vv.b[i] & Rt.b[i & 0x3] != 0时 或者 对应Qx不为0，res_pred.b[i] = 1，否则res_pred.b[i] = 0 <br>

Rt = Rt = 0xff0ff000 <br>
| Qt(uint8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | ff | f | f0 | 0 | 0 | f | f0 | ff | ... |
| res_pred(uint8_t) | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 1 | ... |
- vec & vec -> vec

**HVX_Vector Q6_V_vand_VV(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
向量与操作
Vd.b[i] = Vu.b[i] & Vv.b[i]  
```

| Vu(uint8_t) | ff | f0 | f | 0 | ff | f0 | f | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | ff | ff | ff | ff | 0 | 0 | 0 | 0 | ... |
| res_vec(uint8_t) | ff | f0 | f | 0 | 0 | 0 | 0 | 0 | ... |

**HVX_Vector Q6_V_vnot_V(HVX_Vector Vu)**
```cpp
向量非操作
Vd.b[i] = !Vu.b[i]
```

| Vu(uint8_t) | ff | f0 | f | 0 | ff | f0 | f | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_vec(uint8_t) | 0 | f | f0 | ff | 0 | f | f0 | ff | ... |

**HVX_Vector Q6_V_vor_VV(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
向量或操作
Vd.b[i] = Vu.b[i] | Vv.b[i]
```

| Vu(uint8_t) | ff | f0 | f | 0 | ff | f0 | f | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | ff | ff | ff | ff | 0 | 0 | 0 | 0 | ... |
| res_vec(uint8_t) | ff | ff | ff | ff | ff | f0 | f | 0 | ... |

**HVX_Vector Q6_V_vxor_VV(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
向量异或操作
Vd.b[i] = Vu.b[i] ^ Vv.b[i]
```

| Vu(uint8_t) | ff | f0 | f | 0 | ff | f0 | f | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint8_t) | ff | ff | ff | ff | 0 | 0 | 0 | 0 | ... |
| res_vec(uint8_t) | 0 | f | f0 | ff | ff | f0 | f | 0 | ... |

**HVX_Vector Q6_Vh_vadd_vclb_VhVh(HVX_Vector Vu, HVX_Vector Vv)**
```cpp
clb: Count Leading Bit (0 or 1)
Vd.h[i] = Vv.h[i] + clb(Vu.h[i])
```

| Vu(int16_t) | 7fff | 7fff | ffff | fff | f000 | ff00 | 0 | fffe | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | -32768 | 32767 | -3 | -2 | -1 | 0 | 1 | 2 | ... |
| res_vec(int16_t) | -32767 | -32768 | 13 | 2 | 3 | 8 | 17 | 17 | ... |

**HVX_Vector Q6_Vh_vnormamt_Vh(HVX_Vector Vu)**
```cpp
clb(Vu.h[i]) - 1
```

| Vu(int16_t) | 7fff | 7fff | ffff | fff | f000 | ff00 | 0 | fffe | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_vec(int16_t) | 0 | 0 | 15 | 3 | 3 | 7 | 15 | 14 | ... |

**HVX_Vector Q6_Vh_vpopcount_Vh(HVX_Vector Vu)**
```cpp
count number of bit one
```

| Vu(int16_t) | 7fff | 7fff | ffff | fff | f000 | ff00 | 0 | fffe | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_vec(int16_t) | 15 | 15 | 16 | 12 | 4 | 8 | 0 | 15 | ... |

**HVX_Vector Q6_Vuh_vcl0_Vuh(HVX_Vector Vu)**
```cpp
cl0: Count Leading Zero
```
| Vu(uint16_t) | 7fff | 7fff | ffff | fff | f000 | ff00 | 0 | fffe | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_vec(uint16_t) | 1 | 1 | 0 | 4 | 0 | 0 | 16 | 0 | ... |

**Other Instructions:** <br>
> **HVX_Vector Q6_Vuw_vcl0_Vuw(HVX_Vector Vu)** <br>
> **HVX_Vector Q6_Vw_vadd_vclb_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vw_vnormamt_Vw(HVX_Vector Vu)** <br>

**HVX_Vector Q6_V_equals_V(HVX_Vector Vu)**
```cpp
向量拷贝
```
| Vu(int8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res(int8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |

**Other Instructions: ** <br>
> **HVX_Vector Q6_Vhf_equals_Vqf16(HVX_Vector Vu)** <br>
> **HVX_Vector Q6_Vhf_equals_Wqf32(HVX_VectorPair Vuu)** <br>
> **HVX_Vector Q6_Vsf_equals_Vqf32(HVX_Vector Vu)** <br>

## Vector Transfer
### data type upgrade 

**HVX_VectorPair Q6_Wh_vsxt_Vb(HVX_Vector Vu)** <br>
> sign extend: 在高位补符号位 <br>

| Vu(int8_t) | -3 | -2 | -1 | 0 | 1 | 2 | 3 | 4 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_pair.lo(int16_t) | -3 | -1 | 1 | 3 | 8 | 10 | 12 | 14 | ... |
| res_pair.hi(int16_t) | -2 | 0 | 2 | 4 | 9 | 11 | 13 | 15 | ... |

**HVX_VectorPair Q6_Wuh_vzxt_Vub(HVX_Vector Vu)** <br>
> zero extend: 在高位补0 <br>

| Vu(int8_t) | 0 | 1 | 2 | 3 | 127 | 128 | 254 | 255 | ... |
|---|---|---|---|---|---|---|---|---|---|
| res_pair.lo(int16_t) | 0 | 2 | 127 | 254 | 9 | 11 | 13 | 15 | ... |
| res_pair.hi(int16_t) | 1 | 3 | 128 | 255 | 0 | 0 | 0 | 0 | ... |

**Other Instructions: ** <br>
> **HVX_VectorPair Q6_Wuw_vzxt_Vuh(HVX_Vector Vu)** <br>
> **HVX_VectorPair Q6_Ww_vsxt_Vh(HVX_Vector Vu)** <br>

- shift right and downgrade 

    > 右移Rt位, 且数据类型降级;
    > - [u]h -> [u]b: Rt范围是 0 ~ 7  (内部通过 Rt & 0x7的方式实现)
    > - [u]w -> [u]h: Rt范围是 0 ~ 15 (内部通过 Rt & 0xf的方式实现)
    > rnd模式: 需要先 + 1 << (Rt - 1), 然后再右移Rt位

    在下面的测试中, 设置 **Rt = 2**. <br>

**HVX_Vector Q6_Vb_vasr_VhVhR_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(int16_t) | -6 | -8 | -9 | -1024 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 6 | 8 | 9 | 512 | 0 | 0 | 0 | 0 | ... |
| res(int8_t) | 1 | -2 | 2 | -2 | 2 | -3 | 127 | -128 | ... |

**HVX_Vector Q6_Vb_vasr_VhVhR_rnd_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(int16_t) | -6 | -8 | -9 | -1024 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 6 | 8 | 9 | 512 | 0 | 0 | 0 | 0 | ... |
| res(int8_t) | 2 | -1 | 2 | -2 | 2 | -2 | 127 | -128 | ... |

**HVX_Vector Q6_Vub_vasr_VhVhR_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(int16_t) | -6 | -8 | -9 | -1024 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 6 | 8 | 9 | 512 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 1 | 0 | 2 | 0 | 2 | 0 | 128 | 0 | ... |

**HVX_Vector Q6_Vub_vasr_VhVhR_rnd_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(int16_t) | -6 | -8 | -9 | -1024 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 6 | 8 | 9 | 512 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 2 | 0 | 2 | 0 | 2 | 0 | 128 | 0 | ... |

**HVX_Vector Q6_Vub_vasr_VuhVuhR_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(uint16_t) | 3 | 4 | 5 | 6 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 1017 | 1018 | 1019 | 1024 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 254 | 0 | 254 | 1 | 254 | 1 | 255 | 1 | ... |

**HVX_Vector Q6_Vub_vasr_VuhVuhR_rnd_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(uint16_t) | 3 | 4 | 5 | 6 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 1017 | 1018 | 1019 | 1024 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 254 | 1 | 255 | 1 | 255 | 1 | 255 | 2 | ... |

**Other Instructions:** <br>
> **HVX_Vector Q6_Vh_vasr_VwVwR_rnd_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>
> **HVX_Vector Q6_Vuh_vasr_VwVwR_rnd_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>
> **HVX_Vector Q6_Vuh_vasr_VuwVuwR_rnd_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>
> **HVX_Vector Q6_Vh_vasr_VwVwR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>
> **HVX_Vector Q6_Vh_vasr_VwVwR_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>
> **HVX_Vector Q6_Vuh_vasr_VuwVuwR_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>
> **HVX_Vector Q6_Vuh_vasr_VwVwR_sat(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)** <br>

<br>
<br>

-**round sat and donwgrade**

**HVX_Vector Q6_Vb_vround_VhVh_sat(HVX_Vector Vu, HVX_Vector Vv)** <br>
> 将Vv.h[i]和Vu.h[i]分别做roudning(先+128, 然后>>8) + sat , 然后交替放在结果向量中 <br>

| Vu(int16_t) | -383 | -384 | -640 | -641 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 127 | 128 | 639 | 640 | 0 | 0 | 0 | 0 | ... |
| res(int8_t) | 0 | -1 | 1 | -1 | 2 | -2 | 3 | -3 | ... |

**HVX_Vector Q6_Vub_vround_VhVh_sat(HVX_Vector Vu, HVX_Vector Vv)**
> 将Vv.h[i]和Vu.h[i]分别做roudning(先+128, 然后>>8) + sat(负数饱和成0) , 然后交替放在结果向量中 <br>

| Vu(int16_t) | -383 | -384 | -640 | -641 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 127 | 128 | 639 | 640 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 0 | 0 | 1 | 0 | 2 | 0 | 3 | 0 | ... |

**HVX_Vector Q6_Vub_vround_VuhVuh_sat(HVX_Vector Vu, HVX_Vector Vv)** <br>
> 将Vv.uh[i]和Vu.uh[i]分别做roudning(先+128, 然后>>8) + sat , 然后交替放在结果向量中 <br>

| Vu(uint16_t) | 127 | 128 | 383 | 384 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 65150 | 65151 | 65152 | 65535 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 254 | 0 | 254 | 1 | 255 | 1 | 255 | 2 | ... |

**Other Instructions: ** <br>
> **HVX_Vector Q6_Vh_vround_VwVw_sat(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vuh_vround_VuwVuw_sat(HVX_Vector Vu, HVX_Vector Vv)** <br>
> **HVX_Vector Q6_Vuh_vround_VwVw_sat(HVX_Vector Vu, HVX_Vector Vv)** <br>

### sat 

**HVX_Vector Q6_Vh_vsat_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> int32饱和成int16 <br>

| Vu(int32_t) | 32772 | 32773 | 32774 | 32775 | 32776 | 32777 | 32778 | 32779 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int32_t) | 32739 | 32740 | 32741 | 32742 | 32743 | 32744 | 32745 | 32746 | ... |
| res(int16_t) | 32739 | 32767 | 32740 | 32767 | 32741 | 32767 | 32742 | 32767 | ... |

**HVX_Vector Q6_Vub_vsat_VhVh(HVX_Vector Vu, HVX_Vector Vv)** <br>
> int16饱和成uint8 <br>

| Vu(int16_t) | -32768 | -129 | -128 | -1 | 0 | 0 | 0 | 0 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int16_t) | 32767 | 129 | 128 | 127 | 0 | 0 | 0 | 0 | ... |
| res(uint8_t) | 255 | 0 | 129 | 0 | 128 | 0 | 127 | 0 | ... |

**HVX_Vector Q6_Vuh_vsat_VuwVuw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> uint32饱和成uint16 <br>

| Vu(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(uint32_t) | 65533 | 65534 | 65535 | 65536 | 0 | 0 | 0 | 0 | ... |
| res(uint16_t) | 65533 | 0 | 65534 | 1 | 65535 | 2 | 65535 | 3 | ... |

**HVX_Vector Q6_Vw_vsatdw_VwVw(HVX_Vector Vu, HVX_Vector Vv)** <br>
> 把Vu.w[i]作为高位, Vv.w[i]作为低位, 生成一个int64, 然后进行饱和 <br>

| Vu(int32_t) | 0 | 0 | 0 | -1 | -1 | -1 | -1 | -2 | ... |
|---|---|---|---|---|---|---|---|---|---|
| Vv(int32_t) | 1 | 2147483647 | -2147483648 | -1 | -2147483648 | 2147483647 | 0 | 0 | ... |
| res(int32_t) | 1 | 2147483647 | 2147483647 | -1 | -2147483648 | -2147483648 | -2147483648 | -2147483648 | ... |

## Scatter & Gather

### gather
> - void Q6_vgather_AQRMWw(HVX_Vector* A, HVX_VectorPred Qs, HVX_Vector* Rb, Word32 Mu, HVX_VectorPair Vvv)
> - void Q6_vgather_ARMWw(HVX_Vector* A, HVX_Vector* Rb, Word32 Mu, HVX_VectorPair Vvv)
> - void Q6_vgather_AQRMVh(HVX_Vector* A, HVX_VectorPred Qs, HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv)
> - void Q6_vgather_AQRMVw(HVX_Vector* A, HVX_VectorPred Qs, HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv)
> - void Q6_vgather_ARMVh(HVX_Vector* A, HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv)
> - void Q6_vgather_ARMVw(HVX_Vector* A, HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv)
### scatter
> - void Q6_vscatter_QRMWwV(HVX_VectorPred Qs, HVX_Vector* Rb, Word32 Mu, HVX_VectorPair Vvv, HVX_Vector Vw)
> - void Q6_vscatteracc_RMWwV(HVX_Vector* Rb, Word32 Mu, HVX_VectorPair Vvv, HVX_Vector Vw)
> - void Q6_vscatter_RMWwV(HVX_Vector* Rb, Word32 Mu, HVX_VectorPair Vvv, HVX_Vector Vw)
> - void Q6_vscatter_QRMVhV(HVX_VectorPred Qs, HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv, HVX_Vector Vw)
> - void Q6_vscatter_QRMVwV(HVX_VectorPred Qs, HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv, HVX_Vector Vw)
> - void Q6_vscatteracc_RMVhV(HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv, HVX_Vector Vw)
> - void Q6_vscatter_RMVhV(HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv, HVX_Vector Vw)
> - void Q6_vscatteracc_RMVwV(HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv, HVX_Vector Vw)
> - void Q6_vscatter_RMVwV(HVX_Vector* Rb, Word32 Mu, HVX_Vector Vv, HVX_Vector Vw)

## Lut
**HVX_Vector Q6_Vb_vlut32_VbVbI(HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**<br>

**指令解释**: <br>
> Vu: idx向量(以ub作为单位), Vv: 数据向量(以Byte作为单位), Iu3: 模式向量(0~7)<br>
> 我们把 Iu3 写为 ABC, 每个字母代表1个bit, 从高位到低位 <br>
> 把Vu的某个idx(Vu.ub[i]), 写为 abcdefgh, 每个字母代表1个bit, 从高位到低位 <br>
> 向量Vv的长度为128B, 但是打表的时候是以h(2B)为单位来访问的, 然后再选择低位或是高位Byte <br>
> idx对应的访问数据为: Vu.uh[cdefgh].ub[b] <br>
> 这样我们能完全访问向量的128B的每个Byte了, 但是指令为什么叫lut32呢? <br>
> > 因为我们对idx还做了相关的限制, 只有当ABC == abc的时候, 才能正常访问, 否则返回0 <br>
> Iu3 = 0 (0b000)时, idx只能访问 Vv.uh[0  ~ 31].ub[0] <br>
> Iu3 = 1 (0b001)时, idx只能访问 Vv.uh[32 ~ 63].ub[0] <br>
> Iu3 = 2 (0b010)时, idx只能访问 Vv.uh[0  ~ 31].ub[1] <br>
> Iu3 = 3 (0b011)时, idx只能访问 Vv.uh[32 ~ 63].ub[1] <br> 

| Vv(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| Iu3 = 0(uint8_t) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Iu3 = 1(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 64 | 66 | 68 | 70 | 72 | 74 | 76 | 78 | 80 | 82 | 84 | 86 | 88 | 90 | 92 | 94 | 96 | 98 | 100 | 102 | 104 | 106 | 108 | 110 | 112 | 114 | 116 | 118 | 120 | 122 | 124 | 126 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Iu3 = 2(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Iu3 = 3(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 65 | 67 | 69 | 71 | 73 | 75 | 77 | 79 | 81 | 83 | 85 | 87 | 89 | 91 | 93 | 95 | 97 | 99 | 101 | 103 | 105 | 107 | 109 | 111 | 113 | 115 | 117 | 119 | 121 | 123 | 125 | 127 |
| Iu3 = 4(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

 > 在Iu3 = 4(0b100)的时候, 最高位为1, Vu中的idx的最高位都是0, 不匹配, 所以结果全为0 <br> 


由于idx的解析并不是向量中对应的Byte的idx, 如果想要得到对应的结果, 需要对Vv进行一次转换 <br> 
Vv = Q6_Vb_vshuff_Vb(Vv) <br> 
| Vv(uint8_t) | 0 | 64 | 1 | 65 | 2 | 66 | 3 | 67 | 4 | 68 | 5 | 69 | 6 | 70 | 7 | 71 | 8 | 72 | 9 | 73 | 10 | 74 | 11 | 75 | 12 | 76 | 13 | 77 | 14 | 78 | 15 | 79 | 16 | 80 | 17 | 81 | 18 | 82 | 19 | 83 | 20 | 84 | 21 | 85 | 22 | 86 | 23 | 87 | 24 | 88 | 25 | 89 | 26 | 90 | 27 | 91 | 28 | 92 | 29 | 93 | 30 | 94 | 31 | 95 | ... | 32 | 96 | 33 | 97 | 34 | 98 | 35 | 99 | 36 | 100 | 37 | 101 | 38 | 102 | 39 | 103 | 40 | 104 | 41 | 105 | 42 | 106 | 43 | 107 | 44 | 108 | 45 | 109 | 46 | 110 | 47 | 111 | 48 | 112 | 49 | 113 | 50 | 114 | 51 | 115 | 52 | 116 | 53 | 117 | 54 | 118 | 55 | 119 | 56 | 120 | 57 | 121 | 58 | 122 | 59 | 123 | 60 | 124 | 61 | 125 | 62 | 126 | 63 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| Iu3 = 0(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Iu3 = 1(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Iu3 = 2(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Iu3 = 3(uint8_t) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |

**HVX_Vector Q6_Vb_vlut32_VbVbR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**<br>
当Rt在0 ~ 7范围内时, 结果与Q6_Vb_vlut32_VbVbI相同; <br> 
 当Rt > 7时, 内部实际上做了一个 Rt & 0x7 的操作来剪裁 (见下面结果, Rt = 0 与 Rt = 8 的结果是相同的) <br>
| Vv(uint8_t) | 0 | 64 | 1 | 65 | 2 | 66 | 3 | 67 | 4 | 68 | 5 | 69 | 6 | 70 | 7 | 71 | 8 | 72 | 9 | 73 | 10 | 74 | 11 | 75 | 12 | 76 | 13 | 77 | 14 | 78 | 15 | 79 | 16 | 80 | 17 | 81 | 18 | 82 | 19 | 83 | 20 | 84 | 21 | 85 | 22 | 86 | 23 | 87 | 24 | 88 | 25 | 89 | 26 | 90 | 27 | 91 | 28 | 92 | 29 | 93 | 30 | 94 | 31 | 95 | ... | 32 | 96 | 33 | 97 | 34 | 98 | 35 | 99 | 36 | 100 | 37 | 101 | 38 | 102 | 39 | 103 | 40 | 104 | 41 | 105 | 42 | 106 | 43 | 107 | 44 | 108 | 45 | 109 | 46 | 110 | 47 | 111 | 48 | 112 | 49 | 113 | 50 | 114 | 51 | 115 | 52 | 116 | 53 | 117 | 54 | 118 | 55 | 119 | 56 | 120 | 57 | 121 | 58 | 122 | 59 | 123 | 60 | 124 | 61 | 125 | 62 | 126 | 63 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| Rt = 0(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Rt = 8(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

**HVX_Vector Q6_Vb_vlut32_VbVbR_nomatch(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**<br>
| Vv(uint8_t) | 0 | 64 | 1 | 65 | 2 | 66 | 3 | 67 | 4 | 68 | 5 | 69 | 6 | 70 | 7 | 71 | 8 | 72 | 9 | 73 | 10 | 74 | 11 | 75 | 12 | 76 | 13 | 77 | 14 | 78 | 15 | 79 | 16 | 80 | 17 | 81 | 18 | 82 | 19 | 83 | 20 | 84 | 21 | 85 | 22 | 86 | 23 | 87 | 24 | 88 | 25 | 89 | 26 | 90 | 27 | 91 | 28 | 92 | 29 | 93 | 30 | 94 | 31 | 95 | ... | 32 | 96 | 33 | 97 | 34 | 98 | 35 | 99 | 36 | 100 | 37 | 101 | 38 | 102 | 39 | 103 | 40 | 104 | 41 | 105 | 42 | 106 | 43 | 107 | 44 | 108 | 45 | 109 | 46 | 110 | 47 | 111 | 48 | 112 | 49 | 113 | 50 | 114 | 51 | 115 | 52 | 116 | 53 | 117 | 54 | 118 | 55 | 119 | 56 | 120 | 57 | 121 | 58 | 122 | 59 | 123 | 60 | 124 | 61 | 125 | 62 | 126 | 63 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| Rt = 0(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| Rt = 1(uint8_t) | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| Rt = 2(uint8_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 |
| Rt = 3(uint8_t) | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | ... | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| Rt = 4(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| Rt = 5(uint8_t) | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| Rt = 6(uint8_t) | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 |
| Rt = 7(uint8_t) | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | ... | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |

**HVX_Vector  Q6_Vb_vlut32or_VbVbVbI(HVX_Vector Vx, HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**<br>

> 由于前面的指令都只是对32个idx范围进行操作, 如果idx的范围比较广, 需要连续取值, 那就需要用到下面的指令了 <br>

```c
    HVX_Vector res_or  = Q6_Vb_vlut32_VbVbI(Vu, Vv, 0);
    res_or = Q6_Vb_vlut32or_VbVbVbI(res_or, Vu, Vv, 1);
    res_or = Q6_Vb_vlut32or_VbVbVbI(res_or, Vu, Vv, 2);
    res_or = Q6_Vb_vlut32or_VbVbVbI(res_or, Vu, Vv, 3);
```

| Vv(uint8_t) | 0 | 64 | 1 | 65 | 2 | 66 | 3 | 67 | 4 | 68 | 5 | 69 | 6 | 70 | 7 | 71 | 8 | 72 | 9 | 73 | 10 | 74 | 11 | 75 | 12 | 76 | 13 | 77 | 14 | 78 | 15 | 79 | 16 | 80 | 17 | 81 | 18 | 82 | 19 | 83 | 20 | 84 | 21 | 85 | 22 | 86 | 23 | 87 | 24 | 88 | 25 | 89 | 26 | 90 | 27 | 91 | 28 | 92 | 29 | 93 | 30 | 94 | 31 | 95 | ... | 32 | 96 | 33 | 97 | 34 | 98 | 35 | 99 | 36 | 100 | 37 | 101 | 38 | 102 | 39 | 103 | 40 | 104 | 41 | 105 | 42 | 106 | 43 | 107 | 44 | 108 | 45 | 109 | 46 | 110 | 47 | 111 | 48 | 112 | 49 | 113 | 50 | 114 | 51 | 115 | 52 | 116 | 53 | 117 | 54 | 118 | 55 | 119 | 56 | 120 | 57 | 121 | 58 | 122 | 59 | 123 | 60 | 124 | 61 | 125 | 62 | 126 | 63 | 127 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vu(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |
| res_or(uint8_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 | ... | 64 | 65 | 66 | 67 | 68 | 69 | 70 | 71 | 72 | 73 | 74 | 75 | 76 | 77 | 78 | 79 | 80 | 81 | 82 | 83 | 84 | 85 | 86 | 87 | 88 | 89 | 90 | 91 | 92 | 93 | 94 | 95 | 96 | 97 | 98 | 99 | 100 | 101 | 102 | 103 | 104 | 105 | 106 | 107 | 108 | 109 | 110 | 111 | 112 | 113 | 114 | 115 | 116 | 117 | 118 | 119 | 120 | 121 | 122 | 123 | 124 | 125 | 126 | 127 |

**HVX_VectorPair Q6_Wh_vlut16_VbVhI(HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**<br>


> 上面的vlut32是对ub(1B)进行操作的, 这里的vlut16是对uh(2B)进行操作的, 长度基本是一样的 <br>
> 不同的是, 输出是pair向量, 奇数的idx(Vu.uh[i].ub[0])放在pair的低位, 偶数的idx(Vu.uh[i].ub[1])放在pair的高位 <br>
> 如果 idx = [abcdefgh], Iu3 = [BCD], 输出结果为 ([bcd] == [BCD]) ? Vv.w[defgh].h[c] : 0 <br>
    

| Vu(uint8_t) | 0 | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 4 | 5 | 5 | 6 | 6 | 7 | 7 | 8 | 8 | 9 | 9 | 10 | 10 | 11 | 11 | 12 | 12 | 13 | 13 | 14 | 14 | 15 | 15 | ... | 48 | 48 | 49 | 49 | 50 | 50 | 51 | 51 | 52 | 52 | 53 | 53 | 54 | 54 | 55 | 55 | 56 | 56 | 57 | 57 | 58 | 58 | 59 | 59 | 60 | 60 | 61 | 61 | 62 | 62 | 63 | 63 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res.lo(uint16_t, Rt = 0) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.hi(uint16_t, Rt = 0) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.lo(uint16_t, Rt = 1) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.hi(uint16_t, Rt = 1) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.lo(uint16_t, Rt = 2) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.hi(uint16_t, Rt = 2) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.lo(uint16_t, Rt = 3) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 |
| res.hi(uint16_t, Rt = 3) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 |
| res.lo(uint16_t, Rt = 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res.hi(uint16_t, Rt = 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

> 当Rt = 4时, 因为idx与Iu3不匹配, 导致所有的结果为0 <br>

**HVX_VectorPair Q6_Wh_vlut16_VbVhR(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
与上面的指令基本相同 

**HVX_VectorPair Q6_Wh_vlut16_VbVhR_nomatch(HVX_Vector Vu, HVX_Vector Vv, Word32 Rt)**
| Vu(uint8_t) | 0 | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 4 | 5 | 5 | 6 | 6 | 7 | 7 | 8 | 8 | 9 | 9 | 10 | 10 | 11 | 11 | 12 | 12 | 13 | 13 | 14 | 14 | 15 | 15 | ... | 48 | 48 | 49 | 49 | 50 | 50 | 51 | 51 | 52 | 52 | 53 | 53 | 54 | 54 | 55 | 55 | 56 | 56 | 57 | 57 | 58 | 58 | 59 | 59 | 60 | 60 | 61 | 61 | 62 | 62 | 63 | 63 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res.lo(uint16_t, Rt = 0) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | ... | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 |
| res.hi(uint16_t, Rt = 0) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | ... | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 |
| res.lo(uint16_t, Rt = 1) | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | ... | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 |
| res.hi(uint16_t, Rt = 1) | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | ... | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 | 32 | 34 | 36 | 38 | 40 | 42 | 44 | 46 | 48 | 50 | 52 | 54 | 56 | 58 | 60 | 62 |
| res.lo(uint16_t, Rt = 2) | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | ... | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 |
| res.hi(uint16_t, Rt = 2) | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | ... | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 | 1 | 3 | 5 | 7 | 9 | 11 | 13 | 15 | 17 | 19 | 21 | 23 | 25 | 27 | 29 | 31 |
| res.lo(uint16_t, Rt = 3) | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | ... | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 |
| res.hi(uint16_t, Rt = 3) | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | ... | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 | 33 | 35 | 37 | 39 | 41 | 43 | 45 | 47 | 49 | 51 | 53 | 55 | 57 | 59 | 61 | 63 |
| res.lo(uint16_t, Rt = 4) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | ... | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 |
| res.hi(uint16_t, Rt = 4) | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | ... | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 | 0 | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | 18 | 20 | 22 | 24 | 26 | 28 | 30 |

 Rt = 0 与 Rt = 4的结果是一样的 

**HVX_VectorPair Q6_Wh_vlut16or_WhVbVhI(HVX_VectorPair Vxx, HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**


```c
    Vv = Q6_Vh_vshuff_Vh(Vv);
    res_pair0 = Q6_Wh_vlut16_VbVhI(Vu, Vv, 0);
    res_pair1 = Q6_Wh_vlut16or_WhVbVhI(res_pair0, Vu, Vv, 1);
    res_pair2 = Q6_Wh_vlut16or_WhVbVhI(res_pair1, Vu, Vv, 2);
    res_pair3 = Q6_Wh_vlut16or_WhVbVhI(res_pair2, Vu, Vv, 3);
```
    
| Vu(uint8_t) | 0 | 0 | 1 | 1 | 2 | 2 | 3 | 3 | 4 | 4 | 5 | 5 | 6 | 6 | 7 | 7 | 8 | 8 | 9 | 9 | 10 | 10 | 11 | 11 | 12 | 12 | 13 | 13 | 14 | 14 | 15 | 15 | ... | 48 | 48 | 49 | 49 | 50 | 50 | 51 | 51 | 52 | 52 | 53 | 53 | 54 | 54 | 55 | 55 | 56 | 56 | 57 | 57 | 58 | 58 | 59 | 59 | 60 | 60 | 61 | 61 | 62 | 62 | 63 | 63 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Vv(uint16_t) | 0 | 32 | 1 | 33 | 2 | 34 | 3 | 35 | 4 | 36 | 5 | 37 | 6 | 38 | 7 | 39 | 8 | 40 | 9 | 41 | 10 | 42 | 11 | 43 | 12 | 44 | 13 | 45 | 14 | 46 | 15 | 47 | ... | 16 | 48 | 17 | 49 | 18 | 50 | 19 | 51 | 20 | 52 | 21 | 53 | 22 | 54 | 23 | 55 | 24 | 56 | 25 | 57 | 26 | 58 | 27 | 59 | 28 | 60 | 29 | 61 | 30 | 62 | 31 | 63 |
| res_pair0.lo(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res_pair0.hi(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res_pair1.lo(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res_pair1.hi(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res_pair2.lo(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res_pair2.hi(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| res_pair3.lo(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |
| res_pair3.hi(uint16_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 | ... | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 | 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 | 51 | 52 | 53 | 54 | 55 | 56 | 57 | 58 | 59 | 60 | 61 | 62 | 63 |

**HVX_VectorPair Q6_Wh_vlut16or_VbVhI(HVX_Vector Vu, HVX_Vector Vv, Word32 Iu3)**

> same as Q6_Wh_vlut16or_WhVbVhI


## Other
### store
> - void Q6_vmem_QnRIV_nt(HVX_VectorPred Qv, HVX_Vector* A, HVX_Vector Vs)
> - void Q6_vmem_QnRIV(HVX_VectorPred Qv, HVX_Vector* A, HVX_Vector Vs)
> - void Q6_vmem_QRIV_nt(HVX_VectorPred Qv, HVX_Vector* A, HVX_Vector Vs)
> - void Q6_vmem_QRIV(HVX_VectorPred Qv, HVX_Vector* A, HVX_Vector Vs)

### splat

**HVX_Vector Q6_V_vsplat_R(Word32 Rt)**<br>
Rt = 0x12345678 <br> 
```cpp
Vd.w[i] = Rt;
```

| res(uint8_t) | 78 | 56 | 34 | 12 | 78 | 56 | 34 | 12 | ... |
|---|---|---|---|---|---|---|---|---|---|

**HVX_Vector Q6_Vb_vsplat_R(Word32 Rt)**<br>
Rt = 0x12 <br> 
```cpp
Vd.b[i] = Rt & 0xff;
```

| res(uint8_t) | 12 | 12 | 12 | 12 | 12 | 12 | 12 | 12 | ... |
|---|---|---|---|---|---|---|---|---|---|

**HVX_Vector Q6_Vh_vsplat_R(Word32 Rt)**<br>
Rt = 0x1234 <br> 
```cpp
Vd.h[i] = Rt & 0xffff;
```

| res(uint8_t) | 34 | 12 | 34 | 12 | 34 | 12 | 34 | 12 | ... |
|---|---|---|---|---|---|---|---|---|---|
### setq 

**HVX_VectorPred Q6_Q_vsetq_R(Word32 Rt)**  (Rt = 0 ~ 127) <br>
> 前Rt个bit为1，后面的bit为0. (最少0个1, 最多127个1) <br>
Rt = 3 <br> 

| res_pred(uint8_t) | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

**HVX_VectorPred Q6_Q_vsetq2_R(Word32 Rt)** (Rt = 1 ~ 128) <br>
> 前Rt个bit为1，后面的bit为0 (最少1个1, 最多128个1)<br>
Rt = 3 <br> 

| res_pred(uint8_t) | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | ... | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
### prefixsum

Init HVX_VectorPred Qv of all ones. <br>

| Qv(uint8_t) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | ... | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

**HVX_Vector Q6_Vb_prefixsum_Q(HVX_VectorPred Qv)**<br>
> 从左到右，每个char的值为前面bit的和. <br>

| res(uint8_t) | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | ... | 121 | 122 | 123 | 124 | 125 | 126 | 127 | 128 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

**HVX_Vector Q6_Vh_prefixsum_Q(HVX_VectorPred Qv)**<br>
> 从左到右，每个short的值为前面bit的和. <br>

| res(uint16_t) | 2 | 4 | 6 | 8 | 10 | 12 | 14 | 16 | ... | 114 | 116 | 118 | 120 | 122 | 124 | 126 | 128 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

**HVX_Vector Q6_Vw_prefixsum_Q(HVX_VectorPred Qv)**<br>
> 从左到右，每个int的值为前面bit的和. <br>

| res(uint32_t) | 4 | 8 | 12 | 16 | 20 | 24 | 28 | 32 | ... | 100 | 104 | 108 | 112 | 116 | 120 | 124 | 128 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

**Word32 Q6_R_vextract_VR(HVX_Vector Vu, Word32 Rs)**<br>
> 从Vu中取出第((Rs&127) / 4)个word. <br>

| Vu(uint32_t) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | ... | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

Q6_R_vextract_VR(Vu, 0) = 0 <br>
Q6_R_vextract_VR(Vu, 3) = 0 <br>
Q6_R_vextract_VR(Vu, 4) = 1 <br>
Q6_R_vextract_VR(Vu, 124) = 31 <br>
Q6_R_vextract_VR(Vu, 127) = 31 <br>
Q6_R_vextract_VR(Vu, 128) = 0 <br>
