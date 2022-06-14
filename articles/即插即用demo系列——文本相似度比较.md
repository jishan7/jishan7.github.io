# 即插即用demo系列——文本相似度比较

```python
# coding=GBK

from fuzzywuzzy import fuzz
from gensim import corpora, models, similarities   

def similar(query, choices, coding='utf8'):
    '''文本相似度'''
    return [fuzz.token_sort_ratio(query.decode(coding), i.decode(coding), False) for i in choices]
    
def similar2(query, choices, coding='utf8'):
    '''文本相似度比较'''
    texts = [[word for word in document.decode(coding).split()] for document in choices]
    dictionary = corpora.Dictionary(texts)
    corpus = [dictionary.doc2bow(text) for text in texts]
    tfidf = models.TfidfModel(corpus)
    corpus_tfidf = tfidf[corpus]
    index = similarities.MatrixSimilarity(corpus_tfidf)
    vec_bow  = dictionary.doc2bow(query.decode(coding).split())
    vec_tfidf = tfidf[vec_bow]
    sims = index[vec_tfidf]
    similarity = [int(i * 100) for i in list(sims)]
    return similarity

    
if __name__ == '__main__':
    print similar('测试 分词 fuzzy', ['分词 测试', '测试 分词     fuzzy', '今天 天气 很好', '没事 别 测试', 'NBA 篮球 好玩'], coding='gbk')
    print similar2('测试 分词 fuzzy', ['分词 测试', '测试 分词    fuzzy', '今天 天气 很好', '没事 别 测试', 'NBA 篮球 好玩'], coding='gbk')

```

打印两个结果
```
[63, 100, 21, 44, 20]
[54, 100, 0, 5, 0]
```

数字越大，相似度越大。100的时候，说明完全相同
