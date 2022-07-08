###### tags: `python_函式庫`
# qrcode函式庫:

> [name=羅 世 勇]
> [time=Thu, Jul 7, 2022 8:09 PM]

## qcode函式庫介紹:
&emsp;&emsp;用於快速生成qrcode的函式庫，相比MyQR更簡單
&emsp;&emsp;因為qrcode函式庫需要額外加裝Pillow函式庫才能輸出qrcode，(pillow為一基礎處理影像函式庫)
&emsp;&emsp;qrcode開放原始碼:https://github.com/lincolnloop/python-qrcode
&emsp;&emsp;函式庫安裝:**pip install qrcode**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**pip install pillow**

https://k0976670xxx.github.io/my_notes/
### qrcode函式庫基礎用法篇:
&emsp;&emsp;用法:夠簡單
```python
import qrcode
img = qrcode.make('https://k0976670xxx.github.io/my_notes/')
img.show()                # 顯示圖片
img.save('qrcode.jpg')    # 儲存圖片
```

&emsp;&emsp;輸出結果:![](https://i.imgur.com/zLQtiIJ.png =100x100)

### qrcode函式庫進階設定:

| 參數 | 說明 |
| --- | --- |
| box_size | 一個方塊的邊長為幾個像素，預設 10。 |
| border | 邊界(白框)，預設 4 ( 最小為 4 )。 |
| error_correction | 容錯率，數值為 ERROR_CORRECT_L ( 7% )、<br>ERROR_CORRECT_M ( 15%，預設值 )、<br>ERROR_CORRECT_Q ( 25% )、<br>ERROR_CORRECT_H ( 30% )。 |
| version | 尺寸大小 ( 重複排列次數 )，數值為 1～39，預設 1。 |

#### 使用1:
```python
import qrcode
qr = qrcode.QRCode(
    version=39,#尺寸大小 ( 重複排列次數 )，數值為 1～39。
    error_correction=qrcode.constants.ERROR_CORRECT_L,#容錯率
    box_size=10,#一個方塊的邊長為幾個像素
    border=4#白框
)
qr.add_data('https://k0976670xxx.github.io/my_notes/')   # 要轉換成 QRCode 的文字
qr.make(fit=True)          # 根據參數製作為 QRCode 物件
img = qr.make_image()      # 產生 QRCode 圖片
img.show()                 # 顯示圖片 
img.save('qrcode.png')     # 儲存圖片
```

&emsp;&emsp;輸出結果:嗯..他可以掃描這也是QRcode
![](https://i.imgur.com/GGGmEz0.png)
#### 使用2:設定顏色
```python
import qrcode
qr = qrcode.QRCode(
    version=10,#尺寸大小 ( 重複排列次數 )，數值為 1～39。
    error_correction=qrcode.constants.ERROR_CORRECT_L,#容錯率
    box_size=10,#一個方塊的邊長為幾個像素
    border=4#白框
)
qr.add_data('https://k0976670xxx.github.io/my_notes/')   # 要轉換成 QRCode 的文字
qr.make(fit=True)          # 根據參數製作為 QRCode 物件
img =qr.make_image(fill_color="red", back_color="black")# 產生 QRCode 圖片
img.show()                 # 顯示圖片
img.save('qrcode.png')     # 儲存圖片
```
&emsp;&emsp;輸出結果:![](https://i.imgur.com/I4slsLo.png =100x100)
#### 使用3:輸出svg(向量圖格式):
```python
import qrcode
import qrcode.image.svg
# 要轉換成QRCode的文字，原本的再加上image_factory=qrcode.image.svg.SvgPathImage
#可同樣用上面的進階操作，但不可設定顏色
img = qrcode.make('https://k0976670xxx.github.io/my_notes/', image_factory=qrcode.image.svg.SvgPathImage)    
#img.show()                # SVG 無法使用
img.save('qrcode.svg')     # 儲存圖片，副檔名為 SVG
```
#### 使用4:利用qrcode函式庫內建造型:
&emsp;&emsp;p.s:無法用於svg中
| 造型名稱 | 說明 |
| --- | --- |
| SquareModuleDrawer() | 預設方格 |
| GappedSquareModuleDrawer() | 小方格 |
| CircleModuleDrawer() | 圓點 |
| RoundedModuleDrawer() | 圓角方格 |
| VerticalBarsDrawer() | 直線 |
| HorizontalBarsDrawer() | 橫線 |
```python
import qrcode
from qrcode.image.styledpil import StyledPilImage
from qrcode.image.styles.moduledrawers import SquareModuleDrawer        #原始，方格
from qrcode.image.styles.moduledrawers import GappedSquareModuleDrawer  #小方塊
from qrcode.image.styles.moduledrawers import CircleModuleDrawer        #圓點
from qrcode.image.styles.moduledrawers import RoundedModuleDrawer       #圓角方格
from qrcode.image.styles.moduledrawers import VerticalBarsDrawer        #直線
from qrcode.image.styles.moduledrawers import HorizontalBarsDrawer      #橫線

qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,
    border=4
)
qr.add_data('https://k0976670xxx.github.io/my_notes/')
qr.make(fit=True)

img1 = qr.make_image(image_factory=StyledPilImage, module_drawer=SquareModuleDrawer())
img2 = qr.make_image(image_factory=StyledPilImage, module_drawer=GappedSquareModuleDrawer())
img3 = qr.make_image(image_factory=StyledPilImage, module_drawer=CircleModuleDrawer())
img4 = qr.make_image(image_factory=StyledPilImage, module_drawer=RoundedModuleDrawer())
img5 = qr.make_image(image_factory=StyledPilImage, module_drawer=VerticalBarsDrawer())
img6 = qr.make_image(image_factory=StyledPilImage, module_drawer=HorizontalBarsDrawer())
img1.save('qrcode1.png')
img2.save('qrcode2.png')
img3.save('qrcode3.png')
img4.save('qrcode4.png')
img5.save('qrcode5.png')
img6.save('qrcode6.png')
```
輸出:
|![](https://i.imgur.com/m83xvXt.png)<br>原始方格|![](https://i.imgur.com/Xmterds.png)<br>小方塊|![](https://i.imgur.com/NXcD88k.png)<br>原點|
|:-:|:-:|:-:|
|![](https://i.imgur.com/5Z3iosk.png)<br>圓角方格|![](https://i.imgur.com/a7c9u5A.png)<br>直線|![](https://i.imgur.com/93lVdOX.png)<br>橫線

#### 使用5:加入漸層

| 漸層填色方式 | 參數 | 說明 |
| --- | --- | --- |
| SolidFillColorMask() | (背景、填充) | 預設填滿顏色 |
| RadialGradiantColorMask() | (背景、中心、四周) | 圓形放射漸層 |
| SquareGradiantColorMask() | (背景、中心、四周) | 方形放射漸層 |
| VerticalGradiantColorMask() | (背景、上方、下方) | 垂直漸層 |
| HorizontalGradiantColorMask() | (背景、左側、右側) | 水平漸層 |
| ImageColorMask() | (背景、圖片位址) | 圖片填充 |
```python
import qrcode
from qrcode.image.styledpil import StyledPilImage
from qrcode.image.styles.moduledrawers import RoundedModuleDrawer
from qrcode.image.styles.colormasks import SolidFillColorMask, RadialGradiantColorMask, SquareGradiantColorMask, VerticalGradiantColorMask, HorizontalGradiantColorMask,ImageColorMask

qr = qrcode.QRCode(
    version=5,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,
    border=4
)
qr.add_data('https://k0976670xxx.github.io/my_notes/')
qr.make(fit=True)

img1 = qr.make_image(image_factory=StyledPilImage, color_mask=SolidFillColorMask((255,255,255),(255,0,0)), module_drawer=RoundedModuleDrawer())
img2 = qr.make_image(image_factory=StyledPilImage, color_mask=RadialGradiantColorMask((255,255,255),(255,0,0),(0,0,255)), module_drawer=RoundedModuleDrawer())
img3 = qr.make_image(image_factory=StyledPilImage, color_mask=SquareGradiantColorMask((255,255,255),(255,0,0),(0,0,255)), module_drawer=RoundedModuleDrawer())
img4 = qr.make_image(image_factory=StyledPilImage, color_mask=VerticalGradiantColorMask((255,255,255),(255,0,0),(0,0,255)), module_drawer=RoundedModuleDrawer())
img5 = qr.make_image(image_factory=StyledPilImage, color_mask=HorizontalGradiantColorMask((255,255,255),(255,0,0),(0,0,255)), module_drawer=RoundedModuleDrawer())
img6 = qr.make_image(image_factory=StyledPilImage, color_mask=ImageColorMask((255,255,255),"01.jpg"),module_drawer=RoundedModuleDrawer())

img1.save('qrcode1.png')
img2.save('qrcode2.png')
img3.save('qrcode3.png')
img4.save('qrcode4.png')
img5.save('qrcode5.png')
img6.save('qrcode6.png')
```
|![](https://i.imgur.com/Qo7Wbfn.png)|![](https://i.imgur.com/2HWorgV.png)|![](https://i.imgur.com/DlVFOCC.png)|
|-|-|-|
|![](https://i.imgur.com/zc2zY8b.png)|![](https://i.imgur.com/qck1zhd.png)|![](https://i.imgur.com/8UBLZ4i.png)|

#### 使用6:在中心加圖案(logo)
```python
import qrcode
from qrcode.image.styledpil import StyledPilImage
qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,#若想確保中心圖片清楚可以調高這個
    border=4
)
qr.add_data('https://k0976670xxx.github.io/my_notes/')
qr.make(fit=True)
img = qr.make_image(image_factory=StyledPilImage, embeded_image_path='chino.jpg')
img.save('qrcode.png')
```
&emsp;&emsp;輸出結果:![](https://i.imgur.com/HLRDy5z.png =200x200)




