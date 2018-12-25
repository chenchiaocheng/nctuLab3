# Route Configuration

This repository is a lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program with Ryu SDN framework to build a simple software-defined network and compare the different between two forwarding rules.

---
## Objectives

1. Learn how to build a simple software-defined networking with Ryu SDN framework
2. Learn how to add forwarding rule into each OpenFlow switch

---
## Execution

> TODO:
> * How to run your program?
>> 1. Change the directory into /root/Route_Configuration/src/
>> 2. Run topology with SimpleController.py
>> a. Run topo.py in one terminal
>> `sudo mn --custom topo.py --topo topo --link tc --controller remote`
>> b. Run SimpleController.py in another terminal
>> `sudo ryu-manager SimpleController.py --observe-links`
>> c. 測量頻寬
>> `mininet> h1 iperf -s -u -i 1 > ./out/result1 &`
>> `mininet> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
>> d. leave topo.py first
>> `mininet> exit`
>> e. leave SimpleController.py
>> `Ctrl-z`
>> `mn -c`
>> 3. Run topology with Controller.py
>> a. Run topo.py in one terminal
>> `sudo mn --custom topo.py --topo topo --link tc --controller remote`
>> b. Run Controller.py in another terminal
>> `sudo ryu-manager Controller.py --observe-links`
>> c. 測量頻寬
>> `mininet> h1 iperf -s -u -i 1 > ./out/result2 &`
>> `mininet> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
>> d. leave topo.py first
>> `mininet> exit`
>> e. leave Controller.py
>> `Ctrl-z`
>> `mn -c`
> * What is the meaning of the executing command (both Mininet and Ryu controller)?
> 1. Mininet:
>> * h1 iperf -s : 選擇h1作為server端，以server模式啟動 
>> * -u : 使用UDP(預設為TCP)
>> * -i 1 : 報告時間間隔1秒
>> * -p : 指定服務器端使用的端口或客戶端所連接的接口
>> * ./out/result1 & : 輸出位址
>> * h2 iperf -c 10.0.1 : 選擇h2作為client端，並且連接IP位址10.0.0.1的服務端
>>> `mininet> h1 iperf -s -u -i 1 -p 5566 > ./out/result1 &`
>>> `mininet> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
>>> `mininet> h1 iperf -s -u -i 1 -p 5566 > ./out/result2 &`
>>> `mininet> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
>> * mn -- custom 路徑/檔案名稱.py -- topo 拓樸名稱 : 執行topo.py
>> * tc : 速度限制
>> * --controller remote : 外部controller控制
>>> `sudo mn --custom SimpleTopo.py --topo topo --link tc --controller remote`
>>> `sudo mn --custom topo.py --topo topo --link tc --controller remote`
> 2. Ryu controller
>> * ryu-manager yourapp.py : 執行自己撰寫的python程式
>> * --observe-links : 顯示連結間的訊息 
>>> `sudo ryu-manager SimpleController.py --observe-links`
>>> `sudo ryu-manager Controller.py --observe-links`
> * Show the screenshot of using iPerf command in Mininet (both `SimpleController.py` and `controller.py`)
> SimpleController.py :
![](https://i.imgur.com/KBdQQWR.png)
> controller.py : 
![](https://i.imgur.com/byRMDWj.png)


---
## Description

### Tasks

> TODO:
> * Describe how you finish this work in detail

1. Environment Setup
> * **Step1. Join the lab on GitHub**
>> * 加入lab
https://classroom.github.com/a/RHNMq4Td 
>> * 進入GitHub group https://github.com/nctucn
> * **Step2. Login to your contaier using SSH**
>> * 開啟Pietty，並輸入IP address : 140.113.195.69跟port : 16021
>> 
>>> ![](https://i.imgur.com/dKmam6e.png)
>> * 登入->Login: root  Passward: cn2018
> * **Step3. Clone your GitHub repository**
>> * 把資料夾從GitHub上抓下來
>> `git clone https://github.com/nctucn/lab2-chenchiaocheng.git`
> * **Step4. Run Mininet for testing**
>> * 運行Mininet 
>> `sudo mn`
>> * 如果出現: You may wish to try "service openvswitch-switch start".則將Open vSwitch啟動，輸入:
>> `sudo service openvswitch-switch start`
>> `sudo mn`
2. Example of Ryu SDN
> * **執行 Example of Ryu SDN**
>> * 照著上述登入方法登入container in two terminals
>> * 先進到/lab2-chenchiaocheng/src/路徑中
>> * 在第一個terminal用Mininet執行SimpleTopo.py
>> `sudo mn --custom SimpleTopo.py --topo topo --link tc --controller remote`
>> * 如果出現: File exists，輸入下面指令清除先前的資料
>> `sudo mn -c`
>> * 在第二個terminal用Ryu manager執行SimpleController.py
>> `sudo ryu-manager SimpleController.py --observe-links`
>> * 離開 Ryu controller
>>> * 先離開SimpleTopo.py
>>> `mininet> exit` 
>>> * 再離開SimpleController.py
>>> `Ctrl-z`
>>> `mn -c`
3. Mininet Topology
> * **Step1. Build the topology via Mininet**
>> * 先複製SimpleTopo.py並命名為topo.py
>> `cp SimpleTopo.py topo.py`
>> * 根據topo.png在topo.py加上限制(bandwidth, delay, and loss rate)
> * **Step2. Run Mininet topology and controller**
>> * 在第一個terminal用Mininet執行topo.py
>> `sudo mn --custom topo.py --topo topo --link tc --controller remote`
>> * 如果出現: File exists，輸入下面指令清除先前的資料
>> `sudo mn -c`
>> * 在第二個terminal用Ryu manager執行SimpleController.py
>> `sudo ryu-manager SimpleController.py --observe-links`

>> * 測試h1到h2的連線
>> `mininet> h1 ping h2`
4. Ryu Controller
> * **Finish controller.py**
>> * 先複製SimpleController.py並命名為controller.py
>> `cp SimpleController.py controller.py`
>> * 根據下圖，修改controller.py裡的switch_features_handler(self, ev)
![](https://i.imgur.com/OWzXsxD.png)
5. Measurement
> * **Step1. Measure the bandwidth of SimpleController.py**
>> * 在第一個terminal用Mininet執行topo.py
>> `sudo mn --custom topo.py --topo topo --link tc --controller remote`
>> * 在第二個terminal用Ryu manager執行SimpleController.py
>> `sudo ryu-manager SimpleController.py --observe-links`
>> ![](https://i.imgur.com/0ng00Of.png)
>> * 測量頻寬，並將結果存在./out/result1
>> `mininet> h1 iperf -s -u -i 1 -p 5566 > ./out/result1 &` 
>> `mininet> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
>> * 照前述方法離開topo.py and SimpleController.py
> * **Step2. Measure the bandwidth of controller.py**
>> * 在第一個terminal用Mininet執行topo.py
>> `sudo mn --custom topo.py --topo topo --link tc --controller remote`
>> * 在第二個terminal用Ryu manager執行controller.py
>> `sudo ryu-manager controller.py --observe-links`
>> ![](https://i.imgur.com/dXmcl7Y.png)

>> * 測量頻寬，並將結果存在./out/result2
>> `mininet> h1 iperf -s -u -i 1 -p 5566 > ./out/result2 &` 
>> `mininet> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566`
>> * 照前述方法離開topo.py and controller.py
### Discussion

> TODO:
> * Answer the following questions

1. Describe the difference between packet-in and packet-out in detail.
> packet-in: asynchronous(switch發起的訊息)，接收到封包時，轉送到controller
> packet-out: controller-to-switch(controller發起的訊息)，接收到來自controller的封包時，轉送到指定的port
2. What is “table-miss” in SDN?
> 在某個Flow Table找不到相對應的Flow Entry。當發生Table Miss時，可能1)直接丟掉 2)繼續轉發給後面的Flow Table 3) 封裝成packet-in 訊息發給 Remote Controller
3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?
> 因為要實作Ryu應用程式必須繼承app_manager.RyuApp，它用於加載Ryu應用程式，接受從APP發送過來的訊息，是base裡很重要的文件。
4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, CONFIG_DISPATCHER)
    ```
> set_ev_cls的第一個參數表示事件發生時應該調用的函數，第二個參數表示要接收哪一個階段的事件。
> CONFIG_DISPATCHER : 接收SwitchFeatures
> 所以此行的含義是，接收到switchfeatures消息後，執行switch_features_handler函數。
5. What is the meaning of “datapath” in `controller.py`?
> 運用OpenFlow的拓樸裡的switch
6. Why need to set "`ip_proto=17`" in the flow entry?
> 因為要用UDP傳送封包。
7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.
> SimpleController.py : 
> `0.0-10.0 sec 1.23 MBytes 1.03 Mbits/sec  0.009ms  16/  893(1.8%)`
> 10秒內傳了1.23 MBytes
> 平均傳送速度(頻寬) : 1.03 Mbits/sec
> 傳輸延遲 : 0.009ms
> 傳送893個datagram，遺失16個datagram => 1.8%遺失率
> controller.py : 
> `0.0-10.0 sec 1.23 MBytes 1.03 Mbits/sec  0.011ms  19/  893(2.1%)`
> 10秒內傳了1.23Mbytes
> 平均傳送速度(頻寬) : 1.03 Mbits/sec
> 傳輸延遲 : 0.011ms
> 傳送893個datagram，遺失19個datagram => 2.1%遺失率
8. Which forwarding rule is better? Why?
> SimpleController.py執行起來遺失率較controller.py低，且傳輸延遲也較小，因此我認為SimpleController.py比較好。
---
## References

> TODO: 
> * Please add your references in the following

* **Ryu SDN**
    * [Ryubook Documentation](https://osrg.github.io/ryu-book/en/html/)
    * [Ryubook [PDF]](https://osrg.github.io/ryu-book/en/Ryubook.pdf)
    * [Ryu 4.30 Documentation](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Ryu Controller Tutorial](http://sdnhub.org/tutorials/ryu/)
    * [OpenFlow 1.3 Switch Specification](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)
    * [Ryubook 說明文件](https://osrg.github.io/ryu-book/zh_tw/html/)
    * [GitHub - Ryu Controller 教學專案](https://github.com/OSE-Lab/Learning-SDN/blob/master/Controller/Ryu/README.md)
    * [Ryu SDN 指南 – Pengfei Ni](https://feisky.gitbooks.io/sdn/sdn/ryu.html)
    * [OpenFlow 通訊協定](https://osrg.github.io/ryu-book/zh_tw/html/openflow_protocol.html)
    * [十一月2017 – 阿寬的實驗室](https://ting-kuan.blog/2017/11/)
    * [OpenFlow Switch學習筆記(三)-Flow Tables](https://www.cnblogs.com/CasonChan/p/4620652.html)
* **Mininet**
    * [Mininet Walkthrough](http://mininet.org/walkthrough/)
    * [Introduction to Mininet](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Mininet Python API Reference Manual](http://mininet.org/api/annotated.html)
    * [A Beginner's Guide to Mininet](https://opensourceforu.com/2017/04/beginners-guide-mininet/)
    * [GitHub/OSE-Lab - 熟悉如何使用 Mininet](https://github.com/OSE-Lab/Learning-SDN/blob/master/Mininet/README.md)
    * [菸酒生的記事本 – Mininet 筆記](https://blog.laszlo.tw/?p=81)
    * [Hwchiu Learning Note – 手把手打造仿 mininet 網路](https://hwchiu.com/setup-mininet-like-environment.html)
    * [阿寬的實驗室 – Mininet 指令介紹](https://ting-kuan.blog/2017/11/09/%E3%80%90mininet%E6%8C%87%E4%BB%A4%E4%BB%8B%E7%B4%B9%E3%80%91/)
    * [Mininet 學習指南](https://www.sdnlab.com/11495.html)
    * [SDN 網絡系統之 Mininet 與 API 詳解](https://hk.saowen.com/a/14d7c14b01c541dd2e53991ea67f1c5ca0d6406bdcb07a9d44065b3d4a37f6d7)
    * [Mininet 初探數據中心網路](https://www.jianshu.com/p/b74f3f5cbe34)
    * [SDN(軟體定義網路)初體驗-Mininet](https://zhuanlan.zhihu.com/p/30935141)
* **Python**
    * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
    * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
* **Others**
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Markdown 語法說明](https://markdown.tw/#p)
    * [Markdown 風格](https://kingofamani.gitbooks.io/git-teach/content/chapter_6_gitbook/markdown.html?q=)
    * [如何溝通資料：輕量級標記式語言-使用 Markdown 協助寫作文字敘述](https://medium.com/datainpoint/communicating-md-e53a08e6652f)
    * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
    * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)

---
## Contributors

> TODO:
> * Please replace "`YOUR_NAME`" and "`YOUR_GITHUB_LINK`" into yours

* [Chen Chiao Cheng](https://github.com/chenchiaocheng)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3
