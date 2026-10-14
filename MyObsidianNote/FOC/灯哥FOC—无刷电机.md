*mos管控制方向——无刷电机、mos管的开关决定电机的转*
# 算法
## 力矩环
### 克拉克变换
#### 正变换
**def.三相波形抽象映射到二维坐标**
![[file-20260908201316422.png|412]]![[file-20260908201440237.png|402]]
显然，针对α-β坐标系中α轴，有：
$I_{\alpha}= i_a-\sin 30^\circ i_b-\cos 60^\circ i_c$
$I_{\alpha}= i_a-\frac{1}{2}i_b-\frac{1}{2}i_c$
$I_{\beta}= \cos 30^\circ i_b-\cos 30^\circ i_c$
所以可得**克拉克变换**：（基本形式）
$$
\begin{bmatrix}
I_{\alpha}\\
I_{\beta}
\end{bmatrix}
=
\begin{bmatrix}
1 & -\frac{1}{2} & -\frac{1}{2}\\
0 & \frac{\sqrt{3}}{2} & -\frac{\sqrt{3}}{2}
\end{bmatrix}
\begin{bmatrix}
i_a\\
i_b\\
i_c
\end{bmatrix}
$$
这个是等幅值形式：
$$
\begin{bmatrix}
I_{\alpha}\\
I_{\beta}
\end{bmatrix}
=
\frac{2}{3}
\begin{bmatrix}
1 & -\frac{1}{2} & -\frac{1}{2}\\
0 & \frac{\sqrt{3}}{2} & -\frac{\sqrt{3}}{2}
\end{bmatrix}
\begin{bmatrix}
i_a\\
i_b\\
i_c
\end{bmatrix}
$$
用α相电流输入1A电流的特例来举例，当电流输入时候，根据基尔霍夫电流定律,尽管矢量a与α轴重合，但是由于b,c相电流投影的存在，导致在a相输入1A电流，反应在α轴上的电流并不是等赋值的1A
因此，为了让式子等辐值，即使得a相1A时，反应在α轴上的电流也是1A，我们就得乘上系数2/3
![[file-20260908204155968.png|446]]
可以看出，在式子中，我们消去了变量icic​，这是因为由于基尔霍夫电流定律的存在，我们并不需要知道所有三相电流，我们只需要知道两相电流就能够求解得到另外一相的电流，反映在硬件上，**我们就可以省去一路的电流传感器！节省了成本！**
#### 逆变换（等幅值）
$i_a=I_{\alpha}$
$i_b=\frac{\sqrt{3}I_{\beta}-I_{\alpha}}{2}$
$i_c=\frac{-I_{\alpha}-\sqrt{3}I_{\beta}}{2}$
### 帕克变换
*电机旋转时能够将电机转换成数学定义的模型*
#### 坐标系
##### QD坐标系
在转子上跟着转子一起动的坐标系
![[file-20260908210512343.png|300]]
![[file-20260908211405322.png|306]]
#### 正变换
$i_d=i_{\alpha}\cos\theta+i_{\beta}\sin\theta$
$i_q=-i_{\alpha}\sin\theta+i_{\beta}\cos\theta$
所以可得
$$
\begin{bmatrix}
i_d\\
i_q
\end{bmatrix}
=
\begin{bmatrix}
\cos\theta & \sin\theta\\
-\sin\theta & \cos\theta
\end{bmatrix}
\begin{bmatrix}
i_{\alpha}\\
i_{\beta}
\end{bmatrix}
$$
#### 逆变换
$i_{\alpha}=i_d\cos\theta-i_q\sin\theta$
$i_{\beta}=i_q\cos\theta+i_d\sin\theta$
一般id=0，id为0是因为右手定则，发现id没有驱动能力，驱动力由iq决定
### 三相电压矢量
用欧姆定律从电流乘电阻直接获得
两种变换无区别
![[file-20260909153256064.png|596]]
#### 输出方式
输出连续的电压信号
**单片机**
>控制PWM调整占空比模拟出变化的电压输出
#### 电角度与机械角度
电角度=机械角度 x 极**对**数
* **电机的极数**：电动机的磁极数
![[file-20260909160255120.png|553]]
## 代码相关
// constrain：将控制变量限制在允许范围内，后续主要用于电压、电流、PI输出等限幅。
ledcSetup(2, 30000, 8);   //频道 频率 精度
