## BASIC TRANSFERS
A transfer consistes of 2 phases:  
1. <b> Address </b>: Lasts for a single HCLK cycle unless it is extended by the previous bus transfer
2. <b> Data </b>: might require several HCLK cycles. Use the HREADY signal to comntrol the number of clock cycles requires to complete the transfer

3. data transfer can be either a read or write operation. for a write operation we write data from masterto slave. this takes minimum 2 clock cycles. the first cycle is called the address phase which will last for one cycle unless it is extended by the previous bus transfer.   
