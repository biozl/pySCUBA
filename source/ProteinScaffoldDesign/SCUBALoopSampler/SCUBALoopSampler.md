2.脚本名：`SCUBALoopSampler.py&SCUBALoopReader.py`<br>
功能：对选定区域的loop采样和读取重采样loop的结构

`SCUBALoopSampler.py`<br>
输入文件：pdb文件（必须），参数文件（可选）
脚本示例：
```
parfile = 'ls.par'	# 设定参数文件，如果没有，会自动生成模板参数文件并命名为此，用于保存用户的参数
ipara, sdpara, lpspara = loopsamplerinit(parfile, jobname='auto')	#对参数初始化并返回三组参数：相互作用参数ipara，随机动力学sdpara，loop采样参数lpspara。可对这三组参数调用和修改，如下

#相互作用参数部分一般不需要改动
ipara.weight_coval=1.0          #共价能量项权重
ipara.weight_localhb=0.6        #主链氢键相互作用权重
ipara.weight_localstr=0.5       #主链连续二面角能量项权重
ipara.weight_steric=1.0         #主链原子空间排斥项权重
ipara.weight_phipsi=2.0         #主链单一位点二面角能量项权重
ipara.weight_rotamer=2.4        #侧链构象能量项权重
ipara.weight_scpacking=3.1      #侧链范德华相互作用能量项权重
ipara.weight_sitepair=0.32      #主链位点相互作用能量项权重

#相互作用参数部分一般不需要改动
ipara.weight_coval=1.0          #共价能量项权重
ipara.weight_localhb=0.6        #主链氢键相互作用权重
ipara.weight_localstr=0.5       #主链连续二面角能量项权重
ipara.weight_steric=1.0         #主链原子空间排斥项权重
ipara.weight_phipsi=2.0         #主链单一位点二面角能量项权重
ipara.weight_rotamer=2.4        #侧链构象能量项权重
ipara.weight_scpacking=3.1      #侧链范德华相互作用能量项权重
ipara.weight_sitepair=0.32      #主链位点相互作用能量项权重

#SD参数：用户可能要改动temperatures和gamma
sdpara.doshake=1                #shake failure检查
sdpara.printsteps=500           #输出当前温度和能量的间隔步数
sdpara.randomseed=512           #随机动力学模拟的随机数种子
sdpara.newnbliststeps=100       #重新计算邻居列表的间隔步数
sdpara.newsscodesteps=500       #重新计算二级结构类型的间隔步数
sdpara.temperatures=[0.2]       #Loop区采样温度，注意以列表形式输入[]
sdpara.gamma=5                  #摩擦系数
sdpara.timestep=0.002           #时间步长，单位是皮秒

#以下是loop采样参数	
lpspara.pdbstart = lspdb	    #输入的pdb文件 。必须
lpspara.maxsdsteps=2000	        #每轮采样的SD最大步数
lpspara.enedecay=0.98	        #计算平均能量的衰减系数
lpspara.sdenevarcut=10	        #停止SD的平均能量变化阈值
lpspara.helixinloop=0	        #生成带小helix的loop
lpspara.reconstructnum = 80	    #进行蒙特卡洛loop采样的轮数
lpspara.findlooptimes = 30	    #搜索每一段loop长度和始末位置的次数，建议30-50次
lpspara.loopsearchmode=
'auto'/’short’/’fixloop’/’fixshortloop’	#loop位置和长度的搜索模式。搜索Loop模式的4种模式：fixshortloop：在指定loop区域寻找最短loop；fixloop：在指定loop区域从最短loop的长度n，比较n+1，n+2长度的loop平局能量，取能量最低的loop长度；short：给定loop区的两端都可以往前或往后移动一位残基，然后各自寻找最短长度的loop，比较能量取最低能的长度和loop位置；auto：给定loop区的两端都可以往前或往后移动一位残基，然后寻找从最短长度到最短长度+2长度的最低平均能量的loop参数，使用该参数的loop长度和位置。
注：有可能找不到loop或者找到了但是连不上因为多个loop可能有位阻。
lpspara.readloopnum = 2	                #采样后，读取非冗余结构的数量
lpspara.readrmsd = '0.6'/’auto’/’min’	#非冗余的RMSD标准。可以直接输入数字（单位Å），或者使用以下公式：
0.3+0.2*(A/D-4.0) （单位Å）。其中A表示所有被采用Loop的总残基数，D的取值依赖于以下两种输入（1）auto: D= 1；（2）min：D=被采样的最短Loop的残基数。
lpspara.outloopfile = 'outloops.dat'	#用于保存loop坐标的文件
loops = "A 73 81 0 A 104 112 5 A 128 138 0 "	#需要采样的loop 。如果你没有给定已经定义好的参数文件，则是必须的。格式：每4个string为一组，中间以空格给开；每组第一个字符A表示所在链，第一个数字73表示loop起点，第二个数字81表示终点，第四个数字如果是0，表示需要搜索这个段loop的长度和起始终点位置，如果是其他数字如5，则按照这个长度连接loop并采样。
setloop(lpspara, loops)	                #设置loop
SCUBALoopSampler(parfile,lpspara,ipara,sdpara,loops)	# 进行loop采样,并修改保存lsparfile
SCUBALoopReader(parfile)	            #循环采样结束后读出采样的loop,可以再次修改读取参数使用SCUBALoopReader单独读取
```

输出文件：
SCUBALoopSampler(parfile,lpspara,ipara,sdpara,loops)输出：保存的<mark>参数文件ls.par</mark>;<mark>loop坐标文件</mark>outloops.dat；<mark>新loop文件</mark>newloop；<mark>初始loop坐标文件</mark>startloops.dat，如果实际loop采样的长度和初始pdb的loop不同，则这个文件保存的loop坐标是随机生成的，用于计算非冗余rmsd。
SCUBALoopReader(parfile)函数输出：用户指定数量的<mark>非冗余结构top_n.pdb</mark>以及它们的非冗余loop坐标文件和非冗余rmsd信息文件。

`SCUBALoopReader.py`<br>
功能：读取loop采样后的pdb结构
输入文件：需要SCUBALoopSampler产生参数文件ls.par  ; 新loop文件newloop；loop坐标文件outloops.dat；初始loop坐标文件startloops.dat。
脚本示例：
```
lpspara, sl = loopreaderinit(‘ls.par’)	# 读取参数，返回loop采样参数lpspara和loop保存参数sl
lpspara.readloopnum = 4	                # 读取非冗余结构的数量
lpspara.readrmsd = '0.4'	            # 非冗余的RMSD标准
LoopReader(lpspara,sl)	                # 读loop构象替换到初始结构上，产生top_n.pdb和相关文件
```

另，此时用户也可以直接修改ls.par文件中的
```
ReadLoopNum = 2
ReadRMSD = 0.6
```
两个值，在python中运行：SCUBALoopReader(parfile)
输出文件：同上