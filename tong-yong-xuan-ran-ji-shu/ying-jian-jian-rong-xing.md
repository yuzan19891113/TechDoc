# 硬件兼容性

## shader相关

pow实现，pow\(x, y\)在ios机器上会分解为

$$
x^y = 2^(log_2^x * y)
$$

对于x为负的情况，部分低端ios机器会出现NAN值。

