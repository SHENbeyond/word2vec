word2vec使用方法：

1、将文本语料进行分词，以空格\tab隔开都可以;

2、将分好词的训练语料进行训练，假定语料名称为test.txt且在word2vec目录中。输入命令:
./word2vec -train test.txt -output vectors.bin -cbow 0 -size 200 -window 5 -negative 0 -hs 1 -sample 1e-3 -threads 12 -binary 1

解释：
表示的是输入文件是test.txt，输出文件是vectors.bin，不使用cbow模型，默认为Skip-Gram模型。 每个单词的向量维度是200，训练的窗口大小为5就是考虑一个词前五个和后五个词语（实际代码中还有一个随机选窗口的过程，窗口大小<=5）。不使用NEG方法，使用HS方法。-sampe指的是采样的阈值，如果一个词语在训练样本中出现的频率越大，那么就越会被采样。-binary为1指的是结果二进制存储，为0是普通存储（普通存储的时候是可以打开看到词语和对应的向量的）

补充：
除了以上命令中的参数，word2vec还有几个参数对我们比较有用比如-alpha设置学习速率，默认的为0.025.–min-count设置最低频率，默认是5，如果一个词语在文档中出现的次数小于5，那么就会丢弃。-classes设置聚类个数，看了一下源码用的是k-means聚类的方法。

架构：skip-gram（慢、对罕见字有利）vs CBOW（快）
训练算法：分层softmax（对罕见字有利）vs 负采样（对常见词和低纬向量有利）
欠采样频繁词：可以提高结果的准确性和速度（适用范围1e-3到1e-5）
文本（window）大小：skip-gram通常在10附近，CBOW通常在5附近

常用命令：
1、查找最相近的词：./distance vectors.bin
2、等价相互关系词：./word-analogy vectors.bin

已有的命令(demo):
1、训练词：./demo-word.sh
2、训练相关短语：./demo-phrases.sh(内含语令)
