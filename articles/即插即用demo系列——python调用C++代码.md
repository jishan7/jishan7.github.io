# 即插即用demo系列——python调用C++代码

python部分：pycall.py
```python
# coding=gbk

from ctypes import *

clib = cdll.LoadLibrary('/home/linyc/pythonctype/libpyctype.so')
print "完成装载"

path = '/home/'
clib.init(c_char_p(path))
print "完成init"

def testpyctype():
    showstr = 'test'
    number = 123

    # 返回的就是一个c_char_p类型
    retstr = create_string_buffer('\000'*100000)

    ret = clib.process(c_char_p(showstr), number, retstr)
    print 'process status = %s' % ret

    toprintstr = retstr.value

    return toprintstr

if __name__ == '__main__':
    ret = testpyctype()
    print ret
```

C++部分：proc.cpp

```CPP
#include <string>
#include <iostream>

using namespace std;

extern "C"
{
    void init(char * c_pt_path);
    int process(char * c_pt_showstr, int number, char * retstr);
}

// 供给python调用的初始化函数
void init(char * c_pt_path){
    string tmp1 = c_pt_path;
    ETSPATH = tmp1;
}

// 给 python 调用的主函数
int process(char * c_pt_showstr, int number, char * retstr){
    int state = 0;  // 0表示失败
    string showstr  = c_pt_showstr;

    cout<<showstr<<endl;
    cout<<number<<endl;

    string ret = "ok";
    strcpy(retstr, ret.c_str());
    state = 1;

    return state; // 1表示成功
}

```

注意，cpp代码写好后，要再编译好.so库并命名为libpyctype.so，放在和py代码同一个目录下，这样py代码才能调用cpp里的函数

另外，cpp会将函数名给改了以便支持重载，对于提供给py调用的函数，你要特别声明这个函数不要被改名，也就是extern c。
