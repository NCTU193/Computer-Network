

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

 ### Describe how to run your program
 First, execute `topo.py` using mininet. This file has the topology network which was defined by "topo.png" in one terminal. After that, we execute `SimpleController.py` and `controller.py` respectively in another terminal to compare their difference(different forwarding routes) using Ryu. Last step, go back to the first terminal and execute Iperf command to test the connection between Host1 and Host2. After that, we will get two results(`result1` and `result2`) and we should analyze these two results. 
  ### What is the meaning of the executing command (both Mininet and Ryu controller)?
  `[]` means arguments
  
| <font color=red >Mininet Command</font>  |meaning|
| ---- |----- |
| <font color=red>mn --custom [ ]</font> |assign specified .py file. ex: `topo.py`. |
|<font color=red>mn --topo [ ]</font>|Topo classes. ex:`tree`, `linear`. In this example we refer to the custom topo in last line of .py file assigned above. That's `topo`. |
|<font color=red>mn --controller [ ]</font>|assign controller type. ex:`remote`, `ovsc`.|
| <font color=red>mn -c</font>|clean the mininet|

|<font color=red> Ryu command</font>|meaning|
|-|-|
|<font color=red>ryu-manager [] --observe-links</font>| start ryu and observe the state of links in [] |



 ### Show the screenshot of using iPerf command in (both `SimpleController.py` and `controller.py`)
 **Result1(SimpleControl.py):**
 ![](https://i.imgur.com/uAERrMQ.png)
  
  **Result2(controller.py):**
![](https://i.imgur.com/QxMlp3I.png)



 



---
## Description

### Tasks


**1. Environment Setup**

This Step is the easiest. I first joined the class in Github and successfully connected to my containter with putty. After that, I clone the lab3 document from github then try to run mininet. When I try to execute mininet, I met some error. However, I followed the troubleshooting in slides and solved the problem.  

**2. Example of Ryu SDN**

In Task2, I follow the slides running `SimpleTopo.py` with mininet, and then open another terminal to execute `SimpleController.py`. After doing Task2, I realize that these two files are relevant. `SimpleController.py` can affect `SimpleTopo.py` using Ryu command. 
 
**3. Mininet Topology**

 Task3 started to enter the main topic of Lab3. First, I modified `SimpleTopo.py`, add some constraint between links refer to `topo.png` and save as `topo.py`. Because of the experience of Lab2, I didn't meet problem in task3.  
 
 **4. Ryu Controller**
 
 In Task4, I modified `SimpleController.py` and save as `controller.py`. After observing `SimpleController.py` carefully, I realize that the code we should modified in `controller.py` is the port number. We should modified the port number of links between switches and hosts according to **the picture's forwarding rules in slides page 35**. As for how did I know the port number of every links? I observed that there are some information in `topo.py`. When creating every links in `topo.py`, it added the source port number and destination port nubmer. Furthermore, we can type `links` in minnet CLI, it will show that every links' state and the port numbers they use. With these information, I finished the Task4.   
 
 **5. Measurement**
 
 In Task5, First I execute `topo.py` using mininet in one terminal and execute `SimpleController.py` using ryu-manager in another terminal. Then, I input iperf command in mininet CLI in `topo.py`. The iperf command are used to measure the connection condtion between host1 and host2. After finishing measurement, I execute `topo.py` again. Then, I execute `controller.py` in another terminal and then using iperf command in mininet CLI. After measuring, I got two results of connection condition of different forwarding rules. Then I can compare their difference.  
### Discussion

**1. Describe the difference between packet-in and packet-out in detail.**

**Ans**:Packet-in means switch transfer the packet to controller, packet-out means switch transfer the packet got from controller to the specified port assigned in controller. 

**2. What is “table-miss” in SDN?**

**Ans**:`table-miss` means that there are no corresponding flow entry in flow table if a switch got an packet and analyze it. 

**3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?**

**Ans**:Because the class want to use function in `app_manager.RyuApp`, the class inherit it and then the class can inherit function in `app_manager.RyuApp`. 

**4. Explain the following code in `controller.py`.**
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    ```  
    **Ans**:The code means the event the following function deal with *(first argument)* and in what condition the following function will execute. *(second argument)*, for example, `ofp_event.EventOFPPacketIn` means the following function deal with `packet-in` event. `MAIN_DISPATCHER` means the following function execute in the condition that Ryu and the switch has connected. 

**5. What is the meaning of “datapath” in `controller.py`?**

**Ans**:In Ryu, `datapath` means switch.  

**6. Why need to set "`ip_proto=17`" in the flow entry?**

**Ans**:`ip_proto` means ip-protocol, and `ip-proto=17` means transfer the data to `UDP` *(User Datagram Protocol)*. 

**7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.**

**Ans**:First, we call the result of `SimpleController.py` as result1 and the result of `controller.py` result2. We can observe that result1 lost more packets than result2. Result1 have 31 lost packets and result2 only have 14. Furthermore, result2's bandwitdh is larger than result1's.

**8. Which forwarding rule is better? Why?**

**Ans**:According to result1 and result2, we can say that the forwarding rules of `controller.py` is better than that of `SimpleController.py` because result2 have larger bandwidth and less lost packets than result1.

---
## References

* **Ryu SDN**
    * [IP協定wiki](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)
    * [開始使用Ryu](https://ryu-zhdoc.readthedocs.io/getting_started.html)
    * [Ryubook Documentation](https://osrg.github.io/ryu-book/en/html/)
    * [Ryubook [PDF]](https://osrg.github.io/ryu-book/en/Ryubook.pdf)
     * [Ryu 4.30 Documentation](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
     * [Ryu Controller Tutorial](http://sdnhub.org/tutorials/ryu/)
     * [OpenFlow 1.3 Switch Specification](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)
     * [Ryubook 說明文件](https://osrg.github.io/ryu-book/zh_tw/html/)
     * [GitHub - Ryu Controller 教學專案](https://github.com/OSE-Lab/Learning-SDN/blob/master/Controller/Ryu/README.md)
     * [Ryu SDN 指南 – Pengfei Ni](https://feisky.gitbooks.io/sdn/sdn/ryu.html)
     * [OpenFlow 通訊協定](https://osrg.github.io/ryu-book/zh_tw/html/openflow_protocol.html)
 * **Python**
     * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
     * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
 * **Others**
     * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
     * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
     * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)
 
 ---
## Contributors

<!-- > TODO:
> * Please replace "YOUR_NAME" and "YOUR_GITHUB_LINK" into yours
> * Please replace "`YOUR_NAME`" and "`YOUR_GITHUB_LINK`" into yours -->
* [張承遠](https://github.com/NCTU193)
* [David Lu](https://github.com/yungshenglu)
 
 ---
 ## License
 
 GNU GENERAL PUBLIC LICENSE Version 3
