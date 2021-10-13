# Lab颜色空间

### 通道 <a href="tong-dao" id="tong-dao"></a>

Lab是由一个亮度通道（channel）和两个颜色通道组成的。在Lab颜色空间中，每个颜色用L、a、b三个数字表示，各个分量的含义是这样的： \
\- **L\***代表**亮度** \
\- **a\***代表**从绿色到红色**的分量 \
\- **b\***代表**从蓝色到黄色**的分量

 Lab是基于**人对颜色的感觉**来设计的，更具体地说，它是**感知均匀**（**perceptual uniform**）的。Perceptual uniform的意思是，如果数字（即前面提到的L、a、b这三个数）变化的幅度一样，那么它给人带来视觉上的变化幅度也差不多。

![](<../.gitbook/assets/image (9).png>)

### 数值范围 <a href="shu-zhi-fan-wei" id="shu-zhi-fan-wei"></a>

理论上说，**L\***、**a\***、**b\***都是实数，不过实际一般限定在一个整数范围内： \
\- **L\***越大，亮度越高。L\*为0时代表黑色，为100时代表白色。 \
\- **a\***和**b\***为0时都代表灰色。 \
\- **a\***从负数变到正数，对应颜色从绿色变到红色。 \
\- **b\***从负数变到正数，对应颜色从蓝色变到黄色。 \
\- 我们在实际应用中常常将颜色通道的范围-100\~+100或-128\~127之间。

### RGB2lab

[https://gist.github.com/mattatz/44f081cac87e2f7c8980](https://gist.github.com/mattatz/44f081cac87e2f7c8980)

```
float3 rgb2xyz( float3 c ) {
    float3 tmp;
    tmp.x = ( c.r > 0.04045 ) ? pow( ( c.r + 0.055 ) / 1.055, 2.4 ) : c.r / 12.92;
    tmp.y = ( c.g > 0.04045 ) ? pow( ( c.g + 0.055 ) / 1.055, 2.4 ) : c.g / 12.92,
    tmp.z = ( c.b > 0.04045 ) ? pow( ( c.b + 0.055 ) / 1.055, 2.4 ) : c.b / 12.92;
    const float3x3 mat = float3x3(
		0.4124, 0.3576, 0.1805,
        0.2126, 0.7152, 0.0722,
        0.0193, 0.1192, 0.9505 
	);
    return 100.0 * mul(tmp, mat);
}

float3 xyz2lab( float3 c ) {
    float3 n = c / float3(95.047, 100, 108.883);
    float3 v;
    v.x = ( n.x > 0.008856 ) ? pow( n.x, 1.0 / 3.0 ) : ( 7.787 * n.x ) + ( 16.0 / 116.0 );
    v.y = ( n.y > 0.008856 ) ? pow( n.y, 1.0 / 3.0 ) : ( 7.787 * n.y ) + ( 16.0 / 116.0 );
    v.z = ( n.z > 0.008856 ) ? pow( n.z, 1.0 / 3.0 ) : ( 7.787 * n.z ) + ( 16.0 / 116.0 );
    return float3(( 116.0 * v.y ) - 16.0, 500.0 * ( v.x - v.y ), 200.0 * ( v.y - v.z ));
}

float3 rgb2lab( float3 c ) {
    float3 lab = xyz2lab( rgb2xyz( c ) );
    return float3( lab.x / 100.0, 0.5 + 0.5 * ( lab.y / 127.0 ), 0.5 + 0.5 * ( lab.z / 127.0 ));
}
```
