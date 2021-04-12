# NS3紀錄
###### tags: `Master Report`


# 1.安裝
## 1.1.第一步 - 基本工具
>參考:https://www.youtube.com/watch?v=XbYPmrVdWJA
```lua=
sudo apt-get update
sudo apt-get install git mercurial
sudo apt-get install g++ 
sudo apt-get install make cmake
sudo apt-get install python3 python3-dev pkg-config sqlite3
sudo apt-get install python3-setuptools
sudo apt-get install gir1.2-goocanvas-2.0 python-gi python-gi-cairo python3-gi python3-gi-cairo python3-pygraphviz gir1.2-gtk-3.0 ipython3  
sudo apt-get install qt5-default
sudo apt-get install gdb
```
### 1.1.1.問題
這個安裝不了，網路上有找到一些方法也無法解決
>備註:這部分在後面有解決
```lua=
apt-get install qt5-defaul
```
![](https://i.imgur.com/8aFE7ME.png)


## 1.2.第二步 - 開始
這次我使用Bake下載ns，不建議使用另外兩種
>參考:https://www.nsnam.org/docs/tutorial/html/getting-started.html#building-ns-3
```lua=
cd..
mkdir workspace
cd workspace
git clone https://gitlab.com/nsnam/bake.git
```
![](https://i.imgur.com/SJC8tLj.png)
>備註:如果遇到git沒有安裝，指令如下
>apt-get install git-all

```lua=
cd bake
export BAKE_HOME=`pwd`
export PATH=$PATH:$BAKE_HOME:$BAKE_HOME/build/bin
export PYTHONPATH=$PYTHONPATH:$BAKE_HOME:$BAKE_HOME/build/lib
./bake.py configure -e ns-3.33
./bake.py check //檢查是否有足夠的工具來下載各種組件
```
![](https://i.imgur.com/exXxT5m.png)

```lua=
./bake.py download
```
![](https://i.imgur.com/fcrXSkw.png)
```
root@Arnosss:~/workspace/bake# ./bake.py download
 >> Searching for system dependency setuptools - OK
 >> Searching for system dependency gi-cairo - Problem                                            
 > Problem: Optional dependency, module "gi-cairo" not available                                  
   This may reduce the  functionality of the final build. 
   However, bake will continue since "gi-cairo" is not an essential dependency.
   For more information call bake with -v or -vvv, for full verbose mode.

 >> Searching for system dependency gir-bindings - OK
 >> Searching for system dependency pygobject - OK                                                
 >> Searching for system dependency pygraphviz - Problem                                          
 > Problem: Optional dependency, module "pygraphviz" not available                                
   This may reduce the  functionality of the final build. 
   However, bake will continue since "pygraphviz" is not an essential dependency.
   For more information call bake with -v or -vvv, for full verbose mode.

 >> Searching for system dependency python3-dev - OK
 >> Searching for system dependency python-dev - OK                                               
 >> Searching for system dependency qt - Problem                                                  
 > Problem: Optional dependency, module "qt" not available                                        
   This may reduce the  functionality of the final build. 
   However, bake will continue since "qt" is not an essential dependency.
   For more information call bake with -v or -vvv, for full verbose mode.

 >> Searching for system dependency g++ - OK
 >> Downloading pybindgen-0.21.0.post15+nga587377 (target directory:pybindgen) - OK               
 >> Downloading netanim-3.108 - OK                                                                
 >> Downloading ns-3.33 (target directory:ns-3.33) - OK  
```

```lua=
cd source
ls
```
![](https://i.imgur.com/K37IvLw.png)

```lua=
./bake.py build
```
![](https://i.imgur.com/CSdxhEh.png)


```lua=
./waf configure --enable-sudo --enable-examples --enable-tests //配置與構建
```
![](https://i.imgur.com/ROaWJNL.png)
![](https://i.imgur.com/gC4JLWr.png)
![](https://i.imgur.com/UQJHMBF.png)

```lua=
./test.py //運行該腳本測試ns
```

![](https://i.imgur.com/4Sdvpdc.png)

跑完後
![](https://i.imgur.com/Dtslva6.png)

```lua=
./waf --run hello-simulator //跑完該指令後的輸出為Hello Simulator，表示安裝成功
```
![](https://i.imgur.com/0XutpNc.png)

### 1.2.1問題


#### a ./bake.py download
![](https://i.imgur.com/wHgyR4T.png)

##### qt5-default
如果使用一般的指令下載，則無法:
```lua=
apt install qt5
```
![](https://i.imgur.com/AmOJZiB.png)

目前查詢到與qt5有關的下載指令是這個:
>參考:https://packages.debian.org/zh-tw/sid/qttools5-dev-tools
```lua=
apt-get install qttools5-dev-tools
```
##### b. pygraphviz
>參考:https://installlion.com/kali/kali/main/p/python3-pygraphviz/install/index.html
```lua=
apt-get install python3-pygraphviz
```
![](https://i.imgur.com/cSR0HOp.png)
且
```lua=
pip install pygraphviz
```
![](https://i.imgur.com/j0vHVko.png)

但是沒有因為這樣就解決問題
![](https://i.imgur.com/TKepqbR.png)

##### c. qt
在使用./bake.py download指令時，發現QT無法安裝則處理放案如下:
>參考:https://www.qt.io/
>安裝過程:https://www.youtube.com/watch?v=xXLPniXPAc0
由於網路上找到的解決方案幾乎都是針對較早的版本，所以我直接去官方網站下載即可，這個部分安裝花了一整個晚上，但後續有依照測試，正常。
![](https://i.imgur.com/5L1TTdM.png)

#### 1.2.2. ./waf configure
我們看到了這個PyViz visualizer not enabled (Missing python modules: pygraphviz)
![](https://i.imgur.com/XFLyPPM.png)
在上面的pygraphviz安裝後，在讓整個流程重新跑一次，重要的是要在該視窗update後，就有了!
![](https://i.imgur.com/nlZd0KP.png)

## 1.3. 最後在確認一次
```lua=
./waf
```
![](https://i.imgur.com/4XcaaXS.png)

# 2. 腳本測試
>參考:https://youtu.be/FEmvmeqOGKI
![](https://i.imgur.com/f5RTiPS.png)
```lua=
./waf --run scratch-simulator
```
![](https://i.imgur.com/og8XqOo.png)
```lua=
cd scratch
mkdir Example
cp ./../examples/tutorial/second.cc ./example/second.cc
ls Example
```
![](https://i.imgur.com/76dUCJN.png)


```lua=
cd ..
./waf
```
![](https://i.imgur.com/YJhGCvi.png)
```lua=
./waf --run Example
```
![](https://i.imgur.com/t7zP3fR.png)

```lun=
./waf --run Example --vis
```

## error
在某一次測試的時候，有出現無法使用的狀態，所以得強制將目錄刪除，但一般指令皆無法刪除，狀況如下:
### 1. 無法./waf
![](https://i.imgur.com/bksKZdo.png)

#### 處理方式:砍掉重練
以下為可使用語法:
```
rm -rf Example
```
下面是嘗試的過程，由於幾個刪除的語法嘗試後無法成功，所以一併紀錄上來:
![](https://i.imgur.com/KnVclPv.png)
![](https://i.imgur.com/GYlnMZK.png)
![](https://i.imgur.com/4YktlpZ.png)
![](https://i.imgur.com/qlcpWb3.png)

### 2. ./waf --run Example --vis
![](https://i.imgur.com/otMuWlA.png)

我在網路上查了，都是缺少visualizer，不過我已經在之前處理掉這個問題了。所以問題應該是出在別處。再透過網路上幾個ns3的操作影片中，發現我缺少的是類似顯示器的東西。

在我看的教學影片中，使用的是Xming，不過環境只能使用在Windows，所以在另一個教學影片中，找到了可以直接使用Python或是NetAnim來顯示。
#### qt4
>參考:https://packages.debian.org/zh-tw/jessie/qt4-dev-tools

## 遇到安裝錯誤的套件，導致無法正常update
![](https://i.imgur.com/9Pm4nou.png)
解決辦法:
```lua=
cd /etc/apt/sources.list.d
vim cuda.list
```
![](https://i.imgur.com/wjGAqDr.png)
```lua=
apt update
```
![](https://i.imgur.com/BwqqoWi.png)

:::success
目前正在安裝可視化工具
:::


