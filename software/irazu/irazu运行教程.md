

```bash
#本教程以Irazu软件提供的官方案例3：Staged Excavation of a Circular Tunnel为演示案例进行后续操作；

#1、通过Irazu读取（.fdem）文件，Export文件格式为 (.r3m)，将两个文件上传到超算自己对应的文件夹

#2、进入g01 节点,软件安装在g01节点，只能在这个上面计算
ssh g01

#3、设置license
export IRAZU_LICENSE_PATH=//share/soft/irazu/install_g01/license/

#4、安装GCC环境，这步不用修改
GCC_VERSION=9.2.0
export LD_LIBRARY_PATH=/share/soft/gcc-${GCC_VERSION}/lib64:${LD_LIBRARY_PATH}

#5、运行irazu进行计算
irazu_3d --in ~share/home/chenjx/FDEM/tunnel_test/tunnel_test_femdem.r3m --out ~share/home/chenjx/FDEM/tunnel_test/output
# in 是指读取文件的位置，out是生成结果文件存放的位置
#运行时对应修改文件位置 如：irazu_3d --in /share/home/wangbin/test_irazu/UCS_tutorial_femdem.r3m --out /share/home/wangbin/test_irazu/output

```
