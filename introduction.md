### What is a protocol:
Protocols are a set of rules for establishing a communication between two or many devices so that communication is established between them.

There are two types of protocols: 
1. On-Chip protocols   
    a. AMBA AHB   
    b. AMBA APB  
    c. AMBA AXI  
2. Peripheral Protocols
    a. UART  
    b. I2C  
    c. SPI  

### ON-CHIP PROTOCOLS:
AMBA (Advanced Microcontroller Bus Architecture) is a set of specifications developed by ARM to standardize the communication between different functional blocks of an SoC.  
AMBA consists of multiple protocols including:
1. AHB (Advanced High-performance Bus):  for high speed , high bandwidth communication
2. APB (Advanced Peripheral Bus):        for low power, low bandwidth peripherals 
3. AXI (Advanced eXtensible Interface):   for high performance, low latency, and high bandwidth applications

The AMBA AHB (Advanced High-Performance Bus) is a part of ARM AMBA (Advanced Microcontroller Bus Architecture) family.  
It is designed to suppport high performance, high bandwidth communcation between different components in a system on chip (SoC).  
AHB povides: 
1. single clock edge operation
2. burst transfers
3. pipelined structure   
making it suitable for high-speed designs.


### Overview of AHB protocols: 
AHB protocol defines the interactions between three components :  
1. Managers (Masters): initiates transactions by issuing read and write commands  
2. Subordinates (Slaves): responds to manager requests and provide or store data  
3. Interconnects: Handles data routing and arbitration between managers and subordinates    

masters and slaves are interchangeble depending on which component initiates the transaction

essential features of AHB protocol that optimize performance and efficiency of high speed designs:  
1. single clock edge operation: the protocol operates synchronously only on the : rising edge of a clock, ensuring predictable timing  
2. burst transfers: AHB supports single and burst transfers improving data throughput.  


### AHB system components:
AHB is basically a bus ( a bundle of signals ) which connects a single/multiple masters and slaves
1. AHB MANAGER (MASTER)  
    1. the manager initiates all transactions on a bus  
    2. it provides the address , control signals and data during read and write operations  
    3. only one manager can control the bus at a time to avoid conflict  
2. AHB SUBORDINATE (SLAVE)  
    1. the subordinate responds to the manager's requests based on the provided address  
    2. subordinates include memories, peripherals, and bridges to the other buses like APB  
    3. if the address sent by the manager does not match any subordinate, the response is consideres invalid  

![AMBA-AHB](https://github.com/ShankhalikaMallick/AMBA-AHB/blob/IMAGES/ahb%20block%20diagram.png?raw=true)

## MANAGER BLOCK DIAGRAM:
![Manager](https://github.com/ShankhalikaMallick/AMBA-AHB/blob/IMAGES/manager.png?raw=true)   
SIGNALS: 
all signals are indicated with 'H' which indicates AHB protocol. (for APB signals it is indicated with letter 'P')
1. GLOBAL SIGNALS: common to master and slave
   1. HRESETn: active low signal - 0=reset, 1=not reset  
   2. HCLKn: clock signal  
2. TRANSFER RESPONSE SIGNALS:
   1. HREADY:  
      input for master, output for slave     
      it will tell master if slave is ready for any transaction or not;      
      if HREADY=0 -> slave is not ready;  
      if HREADY=1 -> slave is ready for transaction  
   2. HRESP:  
      input to master, output to a slave  
      it tells master if transaction is successful or not  
      if HRESP=0 -> transaction successful  
      if HRESP=1 -> transaction not successful    
3. ADDRESS AND CONTROL SIGNALS:
    1. HWRITE:  
       indicates is the transaction happening is a read or a write transaction  
       if HWRITE=0 -> read transaction, data transfer from slave to master ie. read from slave  
       if HWRITE=1 -> write transaction, data transfer from master to slave ie. write to slave
   2. HADDR[31:0]:  
      32-bit signal output to master    
      this is used to select the slave
   3. HSIZE[2:0]:  
      3 bits output to the master. used to define the size of data item.
      if a packet is sent from master to slave then the size of the packet is decided by HSIZE signal
   4. HBURST[2:0]:  
      3 bit. counts how many packets are sent.   
      defines the burst mode of an AHB transaction specifying how many data transfers occur in a single burst  
      determines whether the transfer is single, incremental or wrapping burst.
   5. HPROT[3:0]: (optional)      
   6. HTRANS[1:0]:  
      2 bits  
      00: IDLE-     indicates master is idle  
      01: BUSY-     indicates master is busy   
      10: NON SEQUENTIAL  
      11: SEQUENTIAL
   7. HMASTLOCK: (optional)
4. DATA BUS:  
   1. HRDATA[31:0]:  
      input to master; output from slave  
      32 bit data bus to read data from slave to master
   2. HWDATA[31:0]:  
      input to slave; output from master
      32 bit data bus to write data from master to slave
     

