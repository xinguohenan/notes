在现在的nlp_demo中，

1.train.py 处理的是日期转换问题，数据是按照字符为单位传入模型的。对于summary 场景，我们可以把单词转换成id, 然后用word_to_id, id_to_word的形式传入模型。
[train.py](https://github.com/xinguohenan/nlp_demo/blob/main/ch08/train.py)
1. loss函数是直接计算的严格相等。对日期这是有效的，但是对于summary，我们应该用RoughScore去计算loss。
[xsum.py](https://github.com/xinguohenan/nlp_demo/blob/main/dataset/xsum.py)
