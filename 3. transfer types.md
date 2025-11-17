## HTRANS SIGNAL
this is a control signal as output from master and input to slave of 2 bits.   
possible values are 00, 01, 10, 11.  

## 1. IDLE: HTRANS[1:0] = 00
indcates that no data transfer is required. A manager uses an idle transfer when it does not want to perform data transfer. Subordinates must always provide a zero wait state OKAY response to IDLE transfers and the transfer must be ignored by the slave.

## 2. BUSY: HTRANS[1:0] = 01
the BUSY transfer type enables masters to insert IDLE cycles in the middle of a burst . this transfer type indicates that the manager is continuing with a burst but the next transfer cannot take place immediately. Subordinates must always provide a zero wait state OKAY response to BUSY transfers and the transfer must be ignored by the slave.

## 3. NONSEQ: HTRANS[1:0] = 10
Indicates a single transfer or the first transfer of a burst. The address and control signals are unrelated to the previous transfer. Single transfers on the bus are treated as bursts of length one and therefore the transfer tye is NONSEQUENTIAL

## 4. SEQ: HTRANS[1:0] = 11
The remaining transfers in a burst are SEQUENTIAL and the address is related to the previous transfer.

##
1.  suppose we want to perform write operation from a master to slave, there are 4 packets to be written, each associated with a definite address, 
2.  let there be 4 packets P1, P2, P3, P4 associated with addresses A,B,C,D respectively.
3.  here address A is a complete random address specified by NONSEQ value of HTRANS. this is the starting address not associated with any other addresses: independent address
4.  the independent address will also lie within a range of address boundary depending on the memory mapping to slave
5.  depending on the starting address of packet P1, the addresses of the remaining 3 packets P2, P3, P4 will be decided. they are not independent: SEQ value of HTRANS
6.  if we have another burst of 4 packets of data. say P11, P12, P13, P14. the address of P11 is independent and NONSEQ. the addressess of P12, P13, P14 will depend on P11's address. they are SEQUENTIAL and non independent

<img width="1632" height="752" alt="image" src="https://github.com/user-attachments/assets/ef56b53d-d291-43e8-8539-5a08465dc2b2" />

steps: 
1.  we are performing a read operation as HWRITE=0
2.  during T0-T1: HREADY=1, HBURST=INCR (incremental means the number of packets re undefined), HTRANS= NONSEQ the first packet address in now transferred, HADDR=0X20, HRDATA= not yet: address phase of P1
3.  during T1-T2: HREADY=1, HTRANS=BUSY (the master is busy, data is not read), data phase of P1
4.  during T2-T3: HREADY=1, HRDATA=data(0X20) ie the 1st packet is read after some time: data phase of P1
5.  during T2-T3: HTRANS=SEQ, HADDR=0X24, packet 2 address is transferred due to pipeling nature of AHB: address phase of P2
6.  during T3-T4: HRDATA=data(0X24), HADDR=0X28, data phase of P2, address phase of P3
7.  during T4-T5: HREADY=0, master is not ready for any operation
8.  during T5-T6: HTRANS=SEQ, HADDR=0X2C, HRDATA=data(0X28): address phase of P4, data phase of P3

## TRANSFER SIZE
<img width="1037" height="691" alt="image" src="https://github.com/user-attachments/assets/3dff245d-782d-4469-a166-8953b6f8fd1c" />

HSIZE is also a control signal output from master: it indicates the size of each packet  
1. if HSIZE=byte (3'b000) and P1 has address 0X20 (random)    
  Address of P2= 0X20 + 1 = 0X21   
  Address of P3= 0X21 + 1 = 0X22  
  Address of P4= 0X22 + 1 = 0X23  
  ADDRESS OF PAKET= PREVIOUS PACKET ADDRESS + SIZE OF EACH PACKET  
2. if HSIZE=half word (3'b001) and P1 has address 0X20 (random)    
  Address of P2= 0X20 + 2 = 0X22   
  Address of P3= 0X22 + 2 = 0X24  
  Address of P4= 0X24 + 2 = 0X26  
  ADDRESS OF PAKET= PREVIOUS PACKET ADDRESS + SIZE OF EACH PACKET  
