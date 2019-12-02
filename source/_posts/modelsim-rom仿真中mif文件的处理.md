---
title: modelsim rom仿真中mif文件的处理
date: 2018-11-14 10:45:11
tags: verilog,Modelsim仿真
categories: 专业相关
---

{% cq %}## 前言 {% endcq %}

modelsim仿真时不能直接加载mif文件，需要处理一下为hex文件。
### 我的使用条件
- Quartus13.0生成rom的IP核
- sin.mif文件
- 仿真过程：modelsim新建工程，run.do脚本仿真。
### 需要做的
- altera库文件加入编译库
- 加入convert_mif2hex.dll文件，并修改modelsim.ini文件
- 注意路径
<!-- more -->
---
{% cq %} ## 详细说明 {% endcq %}
这里按照我习惯的方式建立工程和文件路径
### 新建工程路径
![习惯路径安排](https://pic4markdown.oss-cn-beijing.aliyuncs.com/20181114114705866.png)
>*prj_name:工程命名;下面包含design、quartus_prj、sim三个文件夹和其他文件*
>*design文件夹下放所有设计文件*
>*quartus_prj文件下是quartus的工程，该文件夹下会新建一个**ipcore_dir**文件夹用来存放生成的ip核文件*
>*sim文件夹是modelsim仿真工程目录，会把测试文件tb_xxx.v、脚本run.do放在sim下*

### 库文件
*220model.v*、*altera_mf.v*库文件复制到**\prj_name\sim\altera_lib**下
*220model.v*、*altera_mf.v*一般在**D:\altera\13.1\quartus\eda\sim_lib**

### 修改modelsim.ini并添加convert_hex2ver.dll文件
下载convert_hex2ver.dll文件  [这里](http://pic4markdown.oss-cn-beijing.aliyuncs.com/%E5%85%B6%E4%BB%96/convert_hex2ver.rar)
1. 修改modelsim安装目录下的modelsim.ini文件，将其只读属性去掉，在vsim部里添加一行“Veriuser = xxx/convert_hex2ver.dll”，保存文件，将只读属性改回来。
2. 将下载的convert_hex2ver.dll文件复制到**D:\altera\13.1\modelsim_ase\win32aloem**

### 路径注意
- sin.mif文件复制到sim目录，**不用转化为hex文件**

{% cq %} ## 开始仿真 {% endcq %}
### 新建工程并编写自动执行脚本
modelsim中新建工程，工程目录为**\prj_name\sim**
run.do脚本
```
quit -sim   #退出当前仿真
.main clear

#建立几个库
vlib	./lib/
vlib	./lib/work_a/
vlib	./lib/design/
vlib	./lib/altera_lib/
vlib	./lib/ipcore_dir/

#将库和实际目录映射
vmap	base_space ./lib/work_a/
vmap	design	./lib/design/
vmap	altera_lib ./lib/altera_lib/
vmap	ipcore_dir ./lib/ipcore_dir/

#编译
vlog	-work base_space	./tb_xxx.v
vlog	-work design		./../design/xxx.v
vlog	-work design		./../design/xxx.v
vlog	-work altera_lib	./altera_lib/*.v
vlog	-work ipcore_dir	./../quartus_prj/ipcore_dir/rom_sin.v
#-t 运行仿真的时间精度是ns
#-L 是链接库关键字
vsim	-t ns  -voptargs=+acc	-L altera_lib -L ipcore_dir -L base_space -L design base_space.tb_xxx   #base_space.tb_xxx是测试文件里的顶层模块名

#添加波形
add wave	-divider {tb_dac900_1}
add wave	tb_dac900/rst_n
add wave	tb_dac900/clk_100M
add wave	tb_dac900/DA_out
#add wave	-divider {dac900}

#add wave	tb_dac900/dac900_inst/DAC900_Clk_r
#add wave	tb_dac900/dac900_inst/DAC900_Clk

#跑
run 20us
```
{% cq %} ## 完～ {% endcq %}
##参考
[ModelSim仿真Altera的ROM](https://www.cnblogs.com/kongqiweiliang/p/3265720.html)

    