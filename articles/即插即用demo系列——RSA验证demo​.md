# 即插即用demo系列——RSA验证​

注意：请先自行装好Crypto再尝试使用以下demo代码

[安装Crypto参考链接​](http://ljhzzyx.blog.163.com/blog/static/3838031220136592824697/)

demo代码如下：
```python
# coding=gbk
__author__ = 'linyc'
import os
import base64

from Crypto.PublicKey import RSA
from Crypto.Signature import PKCS1_v1_5 as PKCS
from Crypto.Hash import MD5 as HASH


# 组织加密明文形式  支持入参为列表、字典和字符串。列表和字典统一转化为特定格式的字符串
# 如果入参是字典，注意键值排序！网络传输可能会打乱key顺序，导致对应的字符串不同，验证会失败！
def _to_content(data, skip_keys):
    if isinstance(data, dict):
        ks = data.keys()
        ks.sort()
        content = '&'.join([i + '=' + data[i]
                            for i in ks
                            if data[i] and (i not in skip_keys) ])

    elif isinstance(data, list):
        content = '&&&'.join(data)

    else:
        content = data

    if isinstance(content, unicode):
        content = content.encode('utf-8')

    return content

# 签名验证，即获取sgn，自己再用公钥加密自己收到的内容，比对两个sgn
def _transfer_verify(data, sgn, public_signer):
    verf_skip_keys = {}
    content = _to_content(data, verf_skip_keys)
    h = HASH.new(content)
    sgn = base64.decodestring(sgn)
    return public_signer.verify(h, sgn)

# 签名，即用私钥，将自己要发送的内容加密成sgn
def _transfer_sign(data, private_signer):
    sign_skip_keys = {}
    content = _to_content(data, sign_skip_keys)
    h = HASH.new(content)
    sgn = private_signer.sign(h)
    return base64.encodestring(sgn).translate(None, ' \r\n')

# RSA加密
def _rsaAdd(message):
    data = message

    dpath = os.path.dirname(__file__)
    with open(os.path.join(dpath, 'private_key.pem')) as f:
        prikey = RSA.importKey(f.read())
        private_signer = PKCS.new(prikey)

    # 生成密文
    sgn = _transfer_sign(data, private_signer)

    return sgn

# RSA解密  如果验证通过，返回True
def _rsaRelease(sgn, data):
    dpath = os.path.dirname(__file__)
    with open(os.path.join(dpath, 'public_key.pem')) as f:
        pubkey = RSA.importKey(f.read())
        public_signer = PKCS.new(pubkey)

    return _transfer_verify(data, sgn, public_signer)

if __name__ == "__main__":
    data = '需要被验证的发送内容message'
    sgn = _rsaAdd(data)
    print sgn

    ismatch = _rsaRelease(sgn, data)
    print ismatch
```

执行截图：
<div align="center"><img src="https://img-blog.csdn.net/20160901143552384?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" /></div>
