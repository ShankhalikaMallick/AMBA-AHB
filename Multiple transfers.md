### multiple transfers  
let there be 3 data packets 
P1: with address A on which write operation is performed ( written from master to slave)  
P2: with address B on which read operation is performed ( read from slave to master)  
P1: with address C on which write operation is performed ( written from master to slave)  
<img width="1306" height="801" alt="image" src="https://github.com/user-attachments/assets/a213df83-1df3-401b-8555-0ffd5846ad86" />
<img width="1280" height="504" alt="image" src="https://github.com/user-attachments/assets/26ce0ca4-8e68-4bab-bc1d-fe4a6bed2328" />
steps:  
1. check for HREADY signal. if HREADY=0, master will not start any phase
2. as HREADY=1, master starts address phase. it provides address A and HWRITE=1, there is no data in the HRDATA as it is a write operation, there is no data in HWDATA as it is the addresss phase
3. so: T0- T1 phase is ADDRESS phase for packet P1
4. T1- T2 is the data phase for packet P1, so HWDATA= data(A), as HREADY=1 data phase lasts for only one clock cycle
5. AHB is a pipelined protocol, so the address phase of packet P2 will start during T1- T2
6. P2 has to be read from slave to master, HWRITE=0, HRDATA=0, HREADY=1 during T1-T2
7. next we have data phase of P2 packet during T2-T3.
8. but HREADY=0, means master is not ready to ready the data from slave, so there will not be any read data HRDATA
9. then data phase is extended to next cycle. T3-T4 is the data phase of P2 packet when HREADY=1.
10. we have another packet P3. the address phase of P3 will start during T2-T3.
11. As HREADY=0, address will be held stable for T3-T4. So extended address phase of P3 is T2-T4
12. HWDATA=data(C) when HREADY=1 and data(B) has already been read, that is during T4-T5
13. T4-T5 is the data phase of P3 : HWDATA=data(C)
