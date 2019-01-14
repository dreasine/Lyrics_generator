# 从零开始用深度学习生成林夕曲风的歌词


## 爬取歌詞
首先尋找需要爬取的歌词语料，我选择了网易云上数量最多的林夕歌单，截止目前收录了3126首林夕的作品。
使用文件夾裏的get_lyrics.ipynb把歌單裏的歌詞爬取到一個文檔裏。
![](https://i.imgur.com/lEV3GIJ.png)


## 數據預處理
得到歌詞預料後需要對數據進行清洗，主要進行如下操作。
* 簡繁轉換。（用OpenCC實現）
```
opencc -i lx_songlist_preprocess.txt -o lx_songlist_preprocess_zh.txt -c zht2zhs.ini
```
* 刪除作詞、作曲、演唱等雜訊。
* 刪除文本中的空行。


處理後大約能得到923kB的corpus。

## 對model進行訓練
### 環境要求
```
tensorflow==1.8.0
```
使用github上的project [Tom-Chang-Deep-Lyrics](https://github.com/fukuball/Tom-Chang-Deep-Lyrics) 的model：Character-level language models for text generation based-on LSTM.
Config.py裏可以調對應的參數。


```bash
python train.py lyrics/lx_songlist_preprocess.txt
```

## 生成歌詞

運行generate.ipynb，修改start_sentence就可以生成不同的歌詞。在writing文件夾裏有之前生成的樣例歌詞。以下是部分結果展示：

### 如果
先用“如果”開頭測試一下：
```
如果碰见我
难道你 只需要一个
回头或能面对
如何自爱 如何自欺
难道自己 如何自欺
难道在你在你在你是谁是谁是谁
谁是你最有感觉留下过去
我的身体 你是我最佳听众
好像 很像 心灵相通
我就是你永远也不值得
我要他身边在乎
你的幸福 我是我最佳听众
好像 很像 心灵相通
我的心 你的眼泪
流浪在我的心 为了一直到你的家
越爱越深 越风靡越风流
是你所有的 我就是我一样
忘掉道行 是我的心
我的情人 
```

### 忘掉種過的花
然後用黃偉文熟悉的作品《喜帖街》的第一句歌詞作爲開頭測試，看是否會有不一樣的感覺：
```
忘掉种过的花
你的爱还是始终
为何为你 为爱而为爱为错 为了什么 期待你
我的心 你的心 我很懂
痛就痛 痛苦快乐吧 也要开心
我的身体跟你思想
在你面前不用口是心非
在最寂寞的关系 永远在左右
事过情迁后 升华以后）升华眼泪后
思念是最漫长的享受（漫长的享受（漫长的享受）
无人的快乐 是我的心情
在我耳边的我 没有所谓
只有你的心跳 还能是真的明白
我的眼泪 埋藏在我的心
我的眼泪 埋藏在我的心
我的眼泪 
```

### 生似蜉蝣
最後用林夕自己的作品裏的一句話
```
生似蜉蝣
只想你幸福不担心的经过
不必记得失去时期
然后可上天下地
记得三岁何处看戏
不淮记得你
不淮再展一条愁眉
然后跟你玩妒忌
爱不到你还要爱美
期待我情深不死
不淮记得失眠时期
然后可上天下地
记得三岁何处看戏
不淮记得你
合着眼睛 怀着笑意的决定
我的情绪是你的心情
在最寂寞的关心 永远在左右
事过情迁后 升华以后）升华眼泪后
思念是你的心
我的心 我的心
我的眼泪能流浪
流过的泪水 把你的问性
```

可以看出還是會有一些重復，語義不詳，或是直接使用大段現有歌詞文本的情況出現，可能還是因爲data不夠多的問題，本來考慮用林夕的書來作爲補充語料pretrain，後來發現林夕文章的風格和詞作的差異太大效果反而不好，遂放棄。之後可能會試試其他風格較明顯的樂隊的詞作來看看是否能生成文風之間的區別，比如五月天或蘇打綠，還可以嘗試加入一些詩詞的語料作爲輔助。