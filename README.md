# Αρχιτεκτονική Υπολογιστών 
## Εργαστηριακή Άσκηση 2
#### Παπαδόπουλος Γρηγόρης - 9441
#### Πρόκου Κατερίνα - 9476
#### ~~~ Αναλυτική Αναφορά ~~~   
***Βήμα 1ο***  
_1._ Βασικές παράμετροι του επεκεργαστή που προσομοιώνει ο gem5, όσον αφορά το υποσύστημα μνήμης. Συγκεκριμένα, παρουσιάζονται τα μεγέθη των:  
* L1 instructions cache= 32kB
* L1 data cache = 64kB
* L2 cache = 2MB
* L1i associativity = 2
* L1d associativity = 2
* L2 associativity = 8
* Cache line = 64B

_2._  Σχετικά με το _401.bzip2_ :  
* Χρόνος εκτέλεσης = 0.083654  
* CPI = 1.673085   
* Συνολικά miss rate της L1 Data Cache = 0.014312  
Συνολικά miss rate της L1 Instruction Cache = 0.000075  
Συνολικά miss rate της L2 Cache = 0.295247  

Σχετικά με το _429.mcf_ :  
* Χρόνος εκτέλεσης = 0.062553  
* CPI = 1.251067    
* Συνολικά miss rate της L1 Data Cache = 0.002062  
Συνολικά miss rate της L1 Instruction Cache = 0.019032   
Συνολικά miss rate της L2 Cache = 0.067668  

Σχετικά με το _456.hmmer_ :  
* Χρόνος εκτέλεσης =  0.000060    
* CPI = 7.652116   
* Συνολικά miss rate της L1 Data Cache = 0.050342      
Συνολικά miss rate της L1 Instruction Cache = 0.094337      
Συνολικά miss rate της L2 Cache = 0.943038    

Σχετικά με το _458.sjeng_ :  
* Χρόνος εκτέλεσης = 0.513823     
* CPI = 10.276466    
* Συνολικά miss rate της L1 Data Cache = 0.121830  
Συνολικά miss rate της L1 Instruction Cache = 0.000020    
Συνολικά miss rate της L2 Cache = 0.999978  

Σχετικά με το _470.lbm_ :   
* Χρόνος εκτέλεσης = 0.174763  
* CPI = 3.495270   
* Συνολικά miss rate της L1 Data Cache = 0.060972      
Συνολικά miss rate της L1 Instruction Cache = 0.000095
Συνολικά miss rate της L2 Cache = 0.99994  

![vima1](https://user-images.githubusercontent.com/58628111/101291402-2584ed00-3811-11eb-9a0d-cf72b45031dd.png)

_3._ ΣΗΜΕΙΩΣΗ : Στο δικό μας μηχάνημα τα benchmarks χρησιμοποιούσαν default συχνότητα των 2GHz, οπότε για να παρατηρήσουμε το οποιοδήποτε scaling χρησιμοποιήσαμε συχνότητα 4GHz. Για το _401.bzip2_ :  
* [system.cpu_clk_domain] clock=500  
* [system.clk_domain] clock=1000  
Παρατηρούμε 2 διαφορετικούς χρονισμούς ρολογιών. Ο 1ος εξ αυτών αναφέρεται στην συχνότητα της CPU αυτής καθ αυτής, δηλαδή την εκτέλεση των εντολών, ενώ ο 2ος λειτουργεί σαν συγχρονιστής.Εάν προστεθεί νέος επεξεργαστής στο σύστημα θα λειτουγεί και αυτός στην συχνότητα 2GHz όπως και ο αρχικός επεξεργαστής.Αυτό φαίνεται και από το entry στο αρχείο config.ini :  
[system.cpu] clk_domain=system.cpu_clk_domain	 	
Default : [system.cpu_clk_domain] clock=500  --->  sim_seconds:0.083654 / CPI:1.673085  
4GHz : [system.cpu_clk_domain] clock=250  ------>  sim_seconds:0.045532 / CPI:1.821276  

Στα ίδια συμπεράσματα καταλήξαμε τρέχοντας και το _456.hmmer_ με:  
Default : [system.cpu_clk_domain] clock=500  
4GHz : [system.cpu_clk_domain] clock=250  
και [system.clk_domain] clock=1000 και στα δύο.  


