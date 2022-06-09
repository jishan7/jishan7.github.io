# 即插即用demo系列——4舍6入5成双

```CPP
// 4舍6入5成双   modf+floor法
double get4s6r(double num){
    if (num <= 0.0){
        return 0.0;
    }

    double fraction, integer;

    fraction   = modf(num, &integer);   // 隔开整数和小数
    int fi     = int(floor(fraction * 1000));  // 小数点后三位的整型表示
    int third  = fi % 10;
    int second = fi % 100 / 10;

    double res = integer + double(fi - third) * 0.001;

    if (third < 5){
        return res;  //舍去
    }
    else if (third > 5){
        return res + (double)1/100; //进位
    }
    else
    {
        if (second & 1){  //奇数
            return res + (double)1/100;
        }
        else{
            return res; 
        }
    }
}
```
