# ABACUS2

3.脚本名：`ABACUS2Designseq.py`<br>
功能：设计序列
输入：pdb文件
```
pdbname='top_0.pdb'	            #输入文件
outname ='SCUBAdesign'	        #输出文件名：SCUBAdesign-00n.pdb
logfile='design.log'	        #log文件
n= 2	#设计序列产生数
resfile='resfile_A'	            #resfile
Vdw=1	                        #范德华系数
Designseq(pdbname,outname,logfile,n,vdw,designresfile=resfile)	#运行设计序列
# Designseq全部参数：Designseq(pdbname, outname,logfile, n=1,vdw=1,div=1,nat=-100, resfile='', parafile='',aapropfile='')	#div:设计序列之间的最大相似度，1表示没有相似性限制；
#nat：与输入的pdb序列相似性（0-1之间，值越大相似性越高，默认-100不开启不用改）
```
输出：设计的n个<mark>序列结构pdb</mark>：SCUBAdesign-001.pdb；<mark>序列信息</mark>和ABACUS2<mark>能量信息</mark>：design.log
