# pubmed_spider: 从PubMed下载文献文本的爬虫程序

## 项目简介

有一天,我想将Zotero中收藏的文章添加到我的LLM知识库中，但我发现从Zotero检索所有PDF文件非常繁琐。

我们如何将这些文章添加到LLM知识库呢？由于Zotero可以将所有文章导出为CSV文件，我们可以解析这个文件来提取文章标题，然后使用这些标题从PubMed检索相应的内容。

这就是创建这个项目的初衷。

## 环境要求

你需要Python3和以下依赖库：

-  pip install lxml
-  pandas
-  numpy
-  requests
-  BeautifulSoup
-  lxml

## 使用方法

```
pubmed_spider.py <zotero导出的csv文件> [输出目录]
```

+ 第一个参数应该是你的文章收藏文件（CSV格式），必须包含带有文章标题的"Title"列。
+ 第二个参数是可选的，用于指定输出目录路径。默认路径为`saves`。

## 使用示例

假设我们有一个名为`population genetic.csv`的文章收藏文件，包含以下信息：

| Publication Year | Author | Title | Publication Title | DOI | Date |
|------------------|--------|-------|-------------------|-----|------|
| 2019 | "Speidel, Leo; Forest, Marie; Shi, Sinan; Myers, Simon R." | A method for genome-wide genealogy estimation for thousands of samples | Nature Genetics | 10.1038/s41588-019-0484-x | Sep-19 |
| 2021 | "Song, Weichen; Shi, Yueqi; Wang, Weidi; Pan, Weihao; Qian, Wei; Yu, Shunying; Zhao, Min; Lin, Guan Ning" | A selection pressure landscape for 870 human polygenic traits | Nature Human Behaviour | 10.1038/s41562-021-01231-4 | Dec-21 |
| 2016 | "Field, Yair; Boyle, Evan A; Telis, Natalie; Gao, Ziyue; Gaulton, Kyle J; Golan, David; Yengo, Loic; Rocheleau, Ghislain; Froguel, Philippe; McCarthy, Mark I; Pritchard, Jonathan K" | Detection of human adaptation during the past 2000 years | Science |  | 2016/11/11 |
| 2023 | "Luo, Huaxia; Zhang, Peng; Zhang, Wanyu; Zheng, Yu; Hao, Di; Shi, Yirong; Niu, Yiwei; Song, Tingrui; Li, Yanyan; Zhao, Shilei; Chen, Hua; Xu, Tao; He, Shunmin" | Recent positive selection signatures reveal phenotypic evolution in the Han Chinese population | Science Bulletin | 10.1016/j.scib.2023.08.027 | Aug-23 |

文件内容如下：

```
Publication Year,Author,Title,Publication Title,DOI,Date
2019,"Speidel, Leo; Forest, Marie; Shi, Sinan; Myers, Simon R.",A method for genome-wide genealogy estimation for thousands of samples,Nature Genetics,10.1038/s41588-019-0484-x,Sep-19
2021,"Song, Weichen; Shi, Yueqi; Wang, Weidi; Pan, Weihao; Qian, Wei; Yu, Shunying; Zhao, Min; Lin, Guan Ning",A selection pressure landscape for 870 human polygenic traits,Nature Human Behaviour,10.1038/s41562-021-01231-4,Dec-21
2016,"Field, Yair; Boyle, Evan A; Telis, Natalie; Gao, Ziyue; Gaulton, Kyle J; Golan, David; Yengo, Loic; Rocheleau, Ghislain; Froguel, Philippe; McCarthy, Mark I; Pritchard, Jonathan K",Detection of human adaptation during the past 2000 years,Science,,2016/11/11
2023,"Luo, Huaxia; Zhang, Peng; Zhang, Wanyu; Zheng, Yu; Hao, Di; Shi, Yirong; Niu, Yiwei; Song, Tingrui; Li, Yanyan; Zhao, Shilei; Chen, Hua; Xu, Tao; He, Shunmin",Recent positive selection signatures reveal phenotypic evolution in the Han Chinese population,Science Bulletin,10.1016/j.scib.2023.08.027,Aug-23
```

我们可以通过运行以下命令来检索所有文章的内容：


```
$> python pubmed_spider.py "population genetic.csv" "population genetic"
input  csv file = population genetic.csv
output dir path = population genetic
[0/4]Processing article: `A method for genome-wide genealogy estimation for thousands of samples`
writing file: autosave-31477933.txt
writing file: autosave-28739658.txt
writing file: autosave-18060068.txt
writing file: autosave-35846115.txt
writing file: autosave-39589398.txt
writing file: autosave-38242694.txt
writing file: autosave-20485442.txt
[1/4]Processing article: `A selection pressure landscape for 870 human polygenic traits`
writing file: autosave-34782732.txt
[2/4]Processing article: `Detection of human adaptation during the past 2000 years`
writing file: autosave-27738015.txt
[3/4]Processing article: `Recent positive selection signatures reveal phenotypic evolution in the Han Chinese population`
writing file: autosave-37661541.txt
$> ls "population genetic"
autosave-18060068.txt  autosave-27738015.txt  autosave-31477933.txt  autosave-35846115.txt  autosave-38242694.txt
autosave-20485442.txt  autosave-28739658.txt  autosave-34782732.txt  autosave-37661541.txt  autosave-39589398.txt
```

注意：由于PubMed查询时的模糊匹配，某些文章可能会映射到多个PMID，导致下载额外的文件。
但是，由于我们的目标是为LLM知识库收集所有相关的文章文本，这并不是一个大问题。


