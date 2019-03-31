# 使用 C# 建立 Modbus tcp slave 在 linux 上运行

* Linux环境
  * virtualbox
  * ubuntu mate 18.10 x64
    * 先改root密码
    ```shell
    sudo passwd root
    ```
* 使用mono
  * 可理解为类似 java 的 runtime 机器
  * 使用root权限安装
    ```shell
    sudo -i
    apt install mono-devel
    ```
  * 运行
    * mono "exe位置" 后面加一个 & 表示让他背景run
    * sample
    ```shell
    sudo -i
    mono '/home/osboxes/Desktop/untitled folder 2/Debug/ConsoleApp4.exe'  &
    ```
  * 关防火墙 让端口通过
    ```shell
    sudo uwf disable
    ```
  * 测试环境
    * windows端 : 192.168.1.1
    * Linux端 : 192.168.1.10
    ```shell
    telnet 192.168.1.10 502
    ```
    * Modbuspoll 测试
![](.\\_modbuspoll.JPG)

* 编程环境
  * windows 10 x64
  * vs2017
  * C# .net framwork 4.5 console 
  * nuget : Nmodbus4
  * code :
    ```c#
    using System;
    using System.IO.Ports;
    using System.Net;
    using System.Net.Sockets;
    using System.Threading;
    using Modbus.Data;
    using Modbus.Device;
    using Modbus.Utility;


    namespace ConsoleApp_slave
    {
        class Program
        {
            static void Main(string[] args)
            {
                byte slaveId = 1;

                int port = 502;
                IPAddress address = new IPAddress(new byte[] { 192, 168, 1, 10 });
    
                TcpListener slaveTcpListener = new TcpListener(address, port);
                slaveTcpListener.Start();

                ModbusSlave slave = ModbusTcpSlave.CreateTcp(slaveId, slaveTcpListener);

                slave.DataStore = DataStoreFactory.CreateDefaultDataStore();
    
                slave.Listen();
    
                Thread.Sleep(Timeout.Infinite);

            }
        }
    }

    ```
