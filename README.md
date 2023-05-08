# Final Report
## 實驗目的
能在板子上跑一個人臉偵測及辨識的程式，並且能夠偵測到人臉的小部分特徵，像是微笑或是眼睛
## 實驗步驟
### 1. Collect data
a.	讀取opencv原始的haarcascade_frontalface_default.xml使用cv2.CascadeClassifier當  作人臉辨識的model

b.	在螢幕前拍攝我們各自的人像，當偵測出人臉會自動轉為灰階並存取在dataset，每人各為100張臉的data，並進行編號，在這邊Ivy為1，Tiffany為2
### 2. Trainning
a.	使用LBPHFaceRecognizer為recongnizer 
```python 
recognizer = cv2.face.LBPHFaceRecognizer_create()
```
b.	對dataset中的人臉依照編號，建立每張圖片的id
c.	用recognizer train 並把 model存在trainer.yml中
### 3. Recognition
這是本次實驗要跑在板子上的部份，因此用C++寫
a.	首先，把opencv pretrained的人臉以及眼睛部分load進來，當成偵測的model
 
b.	再來，也是用LBPHFaceRecognizer為recongnizer ，並把train好的model load進來
 
c.	我們設計有三種模式，1為偵測任何人臉及眼睛部分，2為效能部分，偵測到組員其中一人後立即計算時間並停止，3則是停止此程式
d.	偵測人及眼睛

    1. 先把影像轉為灰階，再來偵測人臉的部分
    2. 偵測到人臉後，將之用一個長方形label起來
    3. 因為已知有臉，固可偵測眼睛
    4. 若偵測到眼睛，則亦把它用長方形label起來
    5. 再來把框出來的人臉丟到model中，可以得他的predict結果及信心程度
    6. 根據predict結果，標示出人臉是誰

e.	效能部分

    1. 由於這部分要計時，所以要記錄按下按鈕的時間為開始時間
    2. 先把影像轉為灰階，再來偵測人臉的部分
    3. 偵測到人臉後，將之用一個長方形label起來
    4. 因為已知有臉，固可偵測眼睛
    5. 若偵測到眼睛，則亦把它用長方形label起來
    6. 再來把框出來的人臉丟到model中，可以得他的predict結果及信心程度
    7. 如果predict結果為組員才可停止並記錄結束時間，若為unknown則繼續偵測
    8. 在人臉中標出組員名字以及歷經多少時間
f.	結束部分
直接結束即可
## 實驗結果
我們偵測出最快的結果為0.2秒就偵測到組員的臉
