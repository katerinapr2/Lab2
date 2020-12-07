# Αρχιτεκτονική Υπολογιστών 
## Εργαστηριακή Άσκηση 2
#### Παπαδόπουλος Γρηγόρης - 9441
#### Πρόκου Κατερίνα - 9476
#### ~~~ Αναλυτική Αναφορά ~~~   
***Βήμα 1ο***  
***_1._*** Βασικές παράμετροι του επεκεργαστή που προσομοιώνει ο gem5, όσον αφορά το υποσύστημα μνήμης. Συγκεκριμένα, παρουσιάζονται τα μεγέθη των:  
* L1 instructions cache= 32kB
* L1 data cache = 64kB
* L2 cache = 2MB
* L1i associativity = 2
* L1d associativity = 2
* L2 associativity = 8
* Cache line = 64B

***_2._***  Σχετικά με το _401.bzip2_ :  
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

![Lab2](https://user-images.githubusercontent.com/58628111/101346360-1776b100-3891-11eb-8eb2-e904205d2ed0.png)  


***_3._*** ΣΗΜΕΙΩΣΗ : Στο δικό μας μηχάνημα τα benchmarks χρησιμοποιούσαν default συχνότητα των 2GHz, οπότε για να παρατηρήσουμε το οποιοδήποτε scaling χρησιμοποιήσαμε συχνότητα 4GHz. Για το _401.bzip2_ :  
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

***Βήμα 2ο***  
***_1._***  ***Σχετικά με τις δοκιμές στο _401.bzip2_*** : Αποτελεί benchmark που στοχεύει στο compression αρχείων.Γι αυτό το λόγο κρίναμε σκόπιμο να αυξήσουμε στο μέγιστο το μέγεθος της L2 cache και L1 data cache ώστε να μπορούν να είναι άμεσα τα δεδομένα στον επεξεργαστή για compression.Επίσης κρίναμε ότι καθώς αυξάνεται το associativity και των 2 μνημών μειώνεται το CPI οπότε το αυξήσαμε στο μέγιστο.Τέλος το cacheline καθώς μεταφέρει τα δεδομένα από την κύρια μνήμη στις cache αποφασίσαμε ότι πρέπει να αυξηθεί.  
![bzip(cpi)](https://user-images.githubusercontent.com/58628111/101296299-4ce8b380-382b-11eb-9255-9e5f589ee774.png)  
![bzip(dcache)](https://user-images.githubusercontent.com/58628111/101296303-4fe3a400-382b-11eb-9621-d5ecf16438fb.png)  
![bzip(icache)](https://user-images.githubusercontent.com/58628111/101296304-5245fe00-382b-11eb-8566-3af4b4133942.png)  
![bzip-l2](https://user-images.githubusercontent.com/58628111/101296305-540fc180-382b-11eb-9eaf-ea6849039fc8.png)  

***Σχετικά με το _429.mcf_*** : Benchmark που εστιάζει σε προβλήματα προγραμματισμού (scheduling).Θεωρήσαμε ότι επωφελείται από αύξηση μεγέθους της L2, ωστόσο χωρίς ιδιαίτερο αποτέλεσμα.  
![Lab2-specmcf_cpi](https://user-images.githubusercontent.com/58628111/101356573-f0c07680-38a0-11eb-8787-6ae4dd8b7d95.png)  
![Lab2-specmcf_dcache](https://user-images.githubusercontent.com/58628111/101356581-f4ec9400-38a0-11eb-9742-906c4afa5da7.png)  
![Lab2-specmcf_icache](https://user-images.githubusercontent.com/58628111/101356586-f74eee00-38a0-11eb-9623-7bd695619aa5.png)  
![Lab2-specmcf_l2](https://user-images.githubusercontent.com/58628111/101356599-fc13a200-38a0-11eb-8d22-6d72c171bc2c.png)  


***Σχετικά με το _456.hmmer_*** : Κυριότερη χρησιμότητα του στο χώρο της βιολογίας και συγκεκριμένα στην ανάλυση του DNA.Αυτό σε συνδυασμό με τα υψηλά miss rate των L1 μνημών μας οδήγησε στην παράλληλη αύξηση των μεγεθών τους με μικρή βελτίωση.Αντίστοιχα σκεφτήκαμε και για την L2 με παρόμοια αποτελέσματα.Τέλος καταλήξαμε στην αύξηση του μεγέθους της cacheline που σε συνδυασμό με τις προηγούμενες αλλαγές επέφερε το μεγαλύτερο όφελος.Ωστόσο παρατηρήσαμε ότι το CPI βελτώνεται μέχρι μια συγκεκριμένη αύξηση μεγέθους cacheline.   
![Lab2-spechmmer_cpi](https://user-images.githubusercontent.com/58628111/101347311-8ef91000-3892-11eb-815f-ad04d0145a8e.png)
![Lab2-spechmmer_dcache](https://user-images.githubusercontent.com/58628111/101347318-90c2d380-3892-11eb-8626-c310a83c827b.png)
![Lab2-spechmmer_icache](https://user-images.githubusercontent.com/58628111/101347325-94eef100-3892-11eb-9b22-683971168ed7.png)
![Lab2-spechmmer_l2](https://user-images.githubusercontent.com/58628111/101347332-96b8b480-3892-11eb-9dfd-20ceb8f30d5e.png)


***Σχετικά με το _458.sjeng_*** : Αποτελεί banchmark που ειδικεύεται πάνω σε παρτίδες σκάκι.Φαίνεται λοιπόν ότι το κυρίαρχο χαρακτηριστικό του είναι η επεξεργασία δεδομένων ώστε να παρθεί η σωστή απόφαση.Γι αυτό το λόγο λειτουργήσαμε παρόμοια με το BZIP benchmark.
![Lab2-specsjeng_cpi](https://user-images.githubusercontent.com/58628111/101296393-f29c2280-382b-11eb-9906-ffbb5c9fd0e5.png)  
![Lab2-specsjeng_dcache](https://user-images.githubusercontent.com/58628111/101296395-f465e600-382b-11eb-9271-13fe021e720b.png)  
![Lab2-specsjeng_icache](https://user-images.githubusercontent.com/58628111/101296397-f62fa980-382b-11eb-8cdd-fb1f50f887d1.png)  
![Lab2-specsjeng_l2](https://user-images.githubusercontent.com/58628111/101296400-f92a9a00-382b-11eb-8e99-6c095ced9c7c.png)  

***Σχετικά με τις δοκιμές στο _470.lbm_*** : Το συγκεκριμένο benchmark υπολογίζει τη ροή ενός ρευστού σε ένα χώρο.Αρχικά αυξήσαμε τα μεγέθη των L1 instruction cache και L1 data cache καθώς είδαμε ότι την πρώτη φορά που έτρεξε το benchmark η L1d είχε σχετικά υψηλό miss rate.Έπειτα αυξήσαμε την L2 και τα associativity όλων με ελάχιστη διαφορά και η πιο σημαντική βελτίωση ήρθε μετά την αύξηση του μεγέθους της cacheline.

![Lab2-speclibm_cpi](https://user-images.githubusercontent.com/58628111/101345248-73403a80-388f-11eb-8fd6-e9bbd6e93ab1.png)  
![Lab2-speclibm_dcache](https://user-images.githubusercontent.com/58628111/101345257-7804ee80-388f-11eb-9be2-228e533e080e.png)  
![Lab2-speclibm_icache](https://user-images.githubusercontent.com/58628111/101345268-7a674880-388f-11eb-870e-795aa8b1fe55.png)  
![Lab2-speclibm_l2](https://user-images.githubusercontent.com/58628111/101345293-805d2980-388f-11eb-8a50-6346adaa3a14.png)  
 



