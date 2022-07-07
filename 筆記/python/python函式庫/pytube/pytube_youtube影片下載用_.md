###### tags: `python_函式庫`
<h1>pytube_youtube影片下載用:</h1>

>[name=羅世勇]
> [time=Thu, Jul 7, 2022 4:58 PM]

## pytube簡介:
&emsp;&emsp;pytube是專門用來讀取**youtube**影片資訊的函式庫，他將複雜的功能整合在了一起，方便使用者使用。
&emsp;&emsp;pytube的開放原始碼庫: <https://github.com/pytube/pytube>
&emsp;&emsp;可參考資料:<https://pytube.io/en/latest/>

&emsp;&emsp;函示庫安裝:**pip install pytube**

---
## 基本用法:

### 從yt影片網址取得影片數據:
&emsp;&emsp;使用方法:[可參考資料網址](https://pytube.io/en/latest/api.html?highlight=download#youtube-object)
```python
    from pytube import YouTube    #也可以只用import pytube，這樣單純乾淨一點
    yt = YouTube("https://youtu.be/0VwWD4uf0qg")    #載入youtube影片ID
    print(yt)                      #顯示影片ID
    print(yt.title)                #顯示標題
    print(str(yt.length)+'sec')    #顯示影片長度(sec)
    print(yt.thumbnail_url)        #顯示縮圖網址
    print(yt.publish_date)         #顯示影片發布"日期"
    print(yt.author)               #顯示頻道名
    print("\n影片說明:\n"+yt.description)#影片說明
    #....有一堆暫時省略，可參考上面網址
```
&emsp;&emsp;輸出結果:
![](https://i.imgur.com/R4cOzPt.png)

### 擷取影音流:
&emsp;&emsp;使用前先簡單說明一下yt影片的架構，yt的影片流有很多種格式，其中有的影片流包含影片及聲音、有的只有聲音或影片而已，例如2K影像yt為了提高觀看體驗通常會將影片及聲音拆開用短封包的方式穩定傳輸影音，又例如360p的影片，使用者網路差不太可能特別要求要高音質的，yt就會給他配較低音質的聲音，繚解後可利用以下方法去取得yt可被截取的影音流格式。
```python
from pytube import YouTube
print(yt.streams.all())                    #顯示所有可用影音流
#因為資料太亂了這裡只顯示所有可用影音流
#print(yt.streams.filter(res='720p'))       #顯示所有720p的資源
#print(yt.streams.filter(subtype='mp4'))    #顯示所有格式為mp4的
#print(yt.streams.filter(only_audio=True))  #顯示所有音訊流
```
&emsp;&emsp;輸出結果(已整理過):
![](https://i.imgur.com/XoqPqTu.png)

### 下載影片:
&emsp;&emsp;在清楚上面架構後，已知vcodec為影片的編解碼模式及acodec為聲音的編解碼模式，可以發現後5個為聲音檔，前三個為同時具有影音的檔案。
&emsp;&emsp;使用:
**p.s:自訂的檔名不能有 / or \ 他會判定成路經**，若用預設檔名，預設為影片標題影片可以有斜線。
**p.s:get_by_resolution()方法為下載影音用，後面的畫質必須要影音皆可編解碼的**
```python
from pytube import YouTube
#下載360p影音
yt.streams.get_by_resolution('360p').download('video/',filename='影片名360p.mp4')
print('360p的影片已下載好\n')
#下載720p影音
yt.streams.get_by_resolution('720p').download('video/',filename='影片名720p.mp4')
print('720p的影片已下載好\n')
#自動下載(最高畫質影音)
yt.streams.get_highest_resolution().download('video/')
print('已將最高畫質下載好了\n')
```
### 下載音樂:
&emsp;&emsp;使用:
```python
from pytube import YouTube
import os
#下載音樂(因為預設副檔名為mp4，所以檔名加上.mp3)
yt.streams.get_audio_only().download('music/',filename=yt.title+'.mp3')
print('已將音樂下載好了\n')

#特殊情況檔名有斜線、空白時，影片的標題無法做儲存所以改成下列方法:
music_file=yt.streams.get_audio_only().download('music/')
#更改檔案副檔名從mp4 to mp3
base, ext = os.path.splitext(music_file)
new_file = base + '.mp3'
os.rename(music_file, new_file)
print('已將音樂下載好了\n')

#或縮減一點:
music_file=yt.streams.get_audio_only().download('music/')
os.rename(music_file,(yt.title+'mp3'))
print('已將音樂下載好了\n')
```

### 處理播放清單:
&emsp;&emsp;使用:Playlist函式會自動將播放清單內所有影片網址組成tuple(元組、數組)，只要再用Youtube函式就能做上面那些操作了。
```python
from pytube import YouTube
from pytube import Playlist
yt_playlist=Playlist("https://youtube.com/playlist?list=PLJvnvm6opLtg_I_D4sBQSTDaSMn_8Jugm")
print(yt_playlist.video_urls) #顯示該播放清單所有影片網址
#利用for loop將tuple內網址逐個讀取
for yt_url in yt_playlist.video_urls:
    yt=YouTube(yt_url,on_progress_callback=on_progress)
    yt.streams.get_highest_resolution().download('video/')
    print(yt.title+'已下載完成\n')
```
## 補充資料:

### 顯示下載進度:
```python
from pytube import YouTube
from pytube.cli import on_progress#必要函式庫
#只要在加上on_progress_callback=on_progress就可以了
yt = YouTube("https://youtu.be/0VwWD4uf0qg",on_progress_callback=on_progress)
yt.streams.get_highest_resolution().download('video/')

```
![](https://i.imgur.com/RVEr5NA.png)
![](https://i.imgur.com/lSbnHhY.png)

---

## 應用:

### 成品1:
```python
from pytube import YouTube
from pytube import Playlist
from pytube.cli import on_progress
import os 
#影片下載
def download_video(yt_url):
    for video_url in yt_url:
        yt=YouTube(video_url,on_progress_callback=on_progress)
        yt.streams.get_highest_resolution().download('video/')
        print(yt.title+'已下載\n')
#音樂下載
def download_music(yt_url):
    for music_url in yt_url:
        yt=YouTube(music_url,on_progress_callback=on_progress)
        music_file=yt.streams.get_audio_only().download('music/')
        base, ext = os.path.splitext(music_file)
        new_file = base + '.mp3'
        os.rename(music_file, new_file)
        print(yt.title+'已下載\n')
#主函式:
while(True):
    print("youtube影片/音樂下載:\n")
    url=input('請貼上網址:\n')
    #簡易判斷是否為yt網址
    if 'https://youtu' in url or 'https://www.youtu' in url:
        #判斷是否為播放清單
        if 'playlist' in url:
            print('確認為播放清單:')
            download_model=input('輸入1下載音樂，輸入任意為下載影片:')
            if download_model=='1':
                print('確認下載音樂')
                download_music(Playlist(url))
            else:
                print('確認下載影片')
                download_video(Playlist(url))
        #為單一影片
        else:
            print('確認為單一網址')
            download_model=input('輸入1下載音樂，輸入任意為下載影片:')
            if download_model=='1':
                print('確認下載音樂')
                #因為播放清單資料以tuple保存，為避免再寫一個子程式處理，浪費空間將他也以tuple保存
                download_music({url})
            else:
                print('確認下載影片')
                download_video({url})
    else:
        print("網址錯誤")
    input("---下載完成----")
```
---

---

---

---


