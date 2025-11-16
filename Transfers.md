## BASIC TRANSFERS
A transfer consistes of 2 phases:  
1. <b> Address </b>: Lasts for a single HCLK cycle unless it is extended by the previous bus transfer. All the address control signals should have a valid value and the valid address of the first packet is provided.  
2. <b> Data </b>: might require several HCLK cycles. Use the HREADY signal to control the number of clock cycles requires to complete the transfer. here data is either read from the slave or is written from the master. the data transfer is done in this phase.  

data transfer can be either a read or write operation. for a write operation we write data from master to slave. this takes minimum 2 clock cycles. the first cycle is called the address phase which will last for one cycle unless it is extended by the previous bus transfer.  

Every data which we are going to send or receive is associated with an <b> ADDRESS. <b/>  In case of read/write data by master, the master defines which slave is to be connected by the corresponding address of the slave. if there is a transfer of more than one or more packets from the master to the slave, transferred in bursts, we have address associated with each packet.  

## Example: READ TRANSFER WITH NO WAIT STATE (HREADY=1)
Eg: If we have a read transfer as shown (HWRITE=0) read from slave. Direction of data is from slave to master. 
![READ TRANSFER](https://github.com/ShankhalikaMallick/AMBA-AHB/blob/IMAGES/read%20transfer.png)
in the first phase (1st clock) master provides the control signals. Here HWRITE=0, HREADY=1, HADDR is the address associated with the packet i want the slave to send. the entire communication will occur when HREADY=1, is the slave is ready for transfer.  
  Address will do both:  
  1. select the slave with which the master wants to communicate and 
  2. also associate with the packet transferred

There is no HRDATA provided during the address phase. Data is provided from the slave in the data phase using HRDATA. ( During data phase address is made B... this is pipelining, we are preparing for next address phase. will see this later. )

## Example : WRITE TRANSFER WITH NO WAIT STATE (HREADY=1)
<img width="1583" height="684" alt="image" src="https://github.com/user-attachments/assets/69f5bd89-5c1b-476a-9d79-148d12134759" />
Here we have a write transfer where we write data from master to slave, so HWRITE=1. HREADY=1 always. Data is sent in the data phase.   

##  BASIC TRANSFERS:
In a simple transfer with no wait states:  (HREADY!=0)
  1. The manager drives the address and control signals onto the bus after the rising edge of HCLK.   
  2. The subordinate then samples the address and control information on the next rising edge of HCLK.  
  3. After the slave has sampled the address and control it cam start to drive the appropiate HREADYOUT response. this response is sampled by the manager on the third rising edge of HCLK.

## READ TRANSFER WITH TWO WAIT STATES: (HREADY=0)
<img width="814" height="318" alt="image" src="https://github.com/user-attachments/assets/8b6f9ae2-9b98-4487-8e68-6447bafead13" />


