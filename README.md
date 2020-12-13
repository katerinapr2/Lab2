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
Παρατηρούμε 2 διαφορετικούς χρονισμούς ρολογιών. Ο 1ος εξ αυτών αναφέρεται στην συχνότητα της CPU αυτής καθ αυτής, δηλαδή την εκτέλεση των εντολών, ενώ ο 2ος λειτουργεί σαν χρονιστής, δηλαδή συγχρονίζει όλες τις εργασίες που εκτελούνται στο πέρς ενός κύκλου ρολογιού. Εάν προστεθεί νέος επεξεργαστής στο σύστημα θα λειτουγεί και αυτός στην συχνότητα 2GHz όπως και ο αρχικός επεξεργαστής.Αυτό φαίνεται και από το entry στο αρχείο config.ini :  
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
Με τα παρακάτω διαγράμματα μπορούμε να δούμε πιο συγκεκριμένα την επίδραση κάθε παραμέτρου στο cpi και τα misses του _470.lbm_.   
![specbzip-l1size](https://user-images.githubusercontent.com/58628111/102022179-c5042b80-3d8d-11eb-86e1-54fdc6576cd4.png) ![specbzip-l1size-dmiss](https://user-images.githubusercontent.com/58628111/102022193-cdf4fd00-3d8d-11eb-9074-4d2fd47a2648.png)    
![specbzip-l1siz-imiss](https://user-images.githubusercontent.com/58628111/102022194-d0575700-3d8d-11eb-87f2-321b7eeb5366.png) ![specbzip-l1size-l2miss](https://user-images.githubusercontent.com/58628111/102022196-d2211a80-3d8d-11eb-96c7-da33e00d5c0e.png)   

![specbzip-l2size](https://user-images.githubusercontent.com/58628111/102022204-e9600800-3d8d-11eb-8587-b6f50c56ed55.png) ![specbzip-l2-dmiss](https://user-images.githubusercontent.com/58628111/102022205-ebc26200-3d8d-11eb-9b03-81c4f847fb6d.png)   
![specbzip-l2-imiss](https://user-images.githubusercontent.com/58628111/102022207-ecf38f00-3d8d-11eb-8682-98bbdbf78115.png) ![specbzip-l2-l2miss](https://user-images.githubusercontent.com/58628111/102022209-ef55e900-3d8d-11eb-93ad-f5fd6905615a.png)   

![specbzip-assoc-cpi](https://user-images.githubusercontent.com/58628111/102022219-0563a980-3d8e-11eb-8205-5ef9f20bc9db.png) ![specbzip-assoc-dmiss](https://user-images.githubusercontent.com/58628111/102022221-072d6d00-3d8e-11eb-8139-f352c57d2d15.png)   
![specbzip-assoc-imiss](https://user-images.githubusercontent.com/58628111/102022223-08f73080-3d8e-11eb-9a5a-f0a1c44272b2.png) ![specbzip-assoc-l2miss](https://user-images.githubusercontent.com/58628111/102022224-0ac0f400-3d8e-11eb-8b86-65650fe427f7.png)  

![specbzip-cachline-cpi](https://user-images.githubusercontent.com/58628111/102022229-20361e00-3d8e-11eb-8b49-998623ab9bec.png) ![specbzip-cacheline-dmiss](https://user-images.githubusercontent.com/58628111/102022231-22987800-3d8e-11eb-8fe4-0d858f79de22.png)    
![specbzip-cacheline-imiss](https://user-images.githubusercontent.com/58628111/102022234-24623b80-3d8e-11eb-9440-ed2e4a6e782e.png) ![specbzip-cacheline-l2miss](https://user-images.githubusercontent.com/58628111/102022237-25936880-3d8e-11eb-80b1-b3bace1927ec.png)    

***Σχετικά με το _429.mcf_*** : Benchmark που εστιάζει σε προβλήματα προγραμματισμού (scheduling).Θεωρήσαμε ότι επωφελείται από αύξηση μεγέθους της L2, ωστόσο χωρίς ιδιαίτερο αποτέλεσμα.  
![Lab2-specmcf_cpi](https://user-images.githubusercontent.com/58628111/101356573-f0c07680-38a0-11eb-8787-6ae4dd8b7d95.png)  ![Lab2-specmcf_dcache](https://user-images.githubusercontent.com/58628111/101356581-f4ec9400-38a0-11eb-9742-906c4afa5da7.png)  
![Lab2-specmcf_icache](https://user-images.githubusercontent.com/58628111/101356586-f74eee00-38a0-11eb-9623-7bd695619aa5.png)  ![Lab2-specmcf_l2](https://user-images.githubusercontent.com/58628111/101356599-fc13a200-38a0-11eb-8d22-6d72c171bc2c.png)  


***Σχετικά με το _456.hmmer_*** : Κυριότερη χρησιμότητα του στο χώρο της βιολογίας και συγκεκριμένα στην ανάλυση του DNA.Αυτό σε συνδυασμό με τα υψηλά miss rate των L1 μνημών μας οδήγησε στην παράλληλη αύξηση των μεγεθών τους με μικρή βελτίωση.Αντίστοιχα σκεφτήκαμε και για την L2 με παρόμοια αποτελέσματα.Τέλος καταλήξαμε στην αύξηση του μεγέθους της cacheline που σε συνδυασμό με τις προηγούμενες αλλαγές επέφερε το μεγαλύτερο όφελος.Ωστόσο παρατηρήσαμε ότι το CPI βελτώνεται μέχρι μια συγκεκριμένη αύξηση μεγέθους cacheline.    
![Lab2-spechmmer_cpi](https://user-images.githubusercontent.com/58628111/101347311-8ef91000-3892-11eb-815f-ad04d0145a8e.png)  ![Lab2-spechmmer_dcache](https://user-images.githubusercontent.com/58628111/101347318-90c2d380-3892-11eb-8626-c310a83c827b.png)
![Lab2-spechmmer_icache](https://user-images.githubusercontent.com/58628111/101347325-94eef100-3892-11eb-9b22-683971168ed7.png)  ![Lab2-spechmmer_l2](https://user-images.githubusercontent.com/58628111/101347332-96b8b480-3892-11eb-9dfd-20ceb8f30d5e.png)   
Με τα παρακάτω διαγράμματα μπορούμε να δούμε πιο συγκεκριμένα την επίδραση κάθε παραμέτρου στο cpi και τα misses του _456.hmmer_.    
![image](https://user-images.githubusercontent.com/58628111/101842062-ef68a580-3b4f-11eb-9088-53f763fba7f0.png) ![image](https://user-images.githubusercontent.com/58628111/101842074-f42d5980-3b4f-11eb-8665-ad4f93940270.png)   
![image](https://user-images.githubusercontent.com/58628111/101842289-643bdf80-3b50-11eb-96da-b7f27fa46be3.png) ![image](https://user-images.githubusercontent.com/58628111/101842302-6aca5700-3b50-11eb-982d-b0d8a70869cb.png)   

![image](https://user-images.githubusercontent.com/58628111/101842515-e0362780-3b50-11eb-8e29-53a7667d173d.png) ![image](https://user-images.githubusercontent.com/58628111/101842522-e4fadb80-3b50-11eb-8635-70c23da5f517.png)   
![image](https://user-images.githubusercontent.com/58628111/101842529-e926f900-3b50-11eb-9420-44eeb23bbc26.png) ![image](https://user-images.githubusercontent.com/58628111/101842537-ee844380-3b50-11eb-8955-f2fcc8b0376f.png)    

![image](https://user-images.githubusercontent.com/58628111/101842932-ccd78c00-3b51-11eb-8fa0-027e881b0e0a.png) ![image](https://user-images.githubusercontent.com/58628111/101842941-d3660380-3b51-11eb-834c-c2a00d64010f.png)   
![image](https://user-images.githubusercontent.com/58628111/101842948-d6f98a80-3b51-11eb-8cfa-532797710a22.png) ![image](https://user-images.githubusercontent.com/58628111/101842954-d9f47b00-3b51-11eb-8693-5274a9c667db.png)   

![image](https://user-images.githubusercontent.com/58628111/101842849-969a0c80-3b51-11eb-9313-37e30a339680.png) ![image](https://user-images.githubusercontent.com/58628111/101842856-9994fd00-3b51-11eb-8611-cf4820eb200e.png)   
![image](https://user-images.githubusercontent.com/58628111/101842862-9c8fed80-3b51-11eb-9cc4-44525a1fd89c.png) ![image](https://user-images.githubusercontent.com/58628111/101842865-9ef24780-3b51-11eb-8700-b6e6c41d1a70.png)    

***Σχετικά με το _458.sjeng_*** : Αποτελεί banchmark που ειδικεύεται πάνω σε παρτίδες σκάκι.Φαίνεται λοιπόν ότι το κυρίαρχο χαρακτηριστικό του είναι η επεξεργασία δεδομένων ώστε να παρθεί η σωστή απόφαση.Γι αυτό το λόγο λειτουργήσαμε παρόμοια με το BZIP benchmark.   
![Lab2-specsjeng_cpi](https://user-images.githubusercontent.com/58628111/101296393-f29c2280-382b-11eb-9906-ffbb5c9fd0e5.png)  ![Lab2-specsjeng_dcache](https://user-images.githubusercontent.com/58628111/101296395-f465e600-382b-11eb-9271-13fe021e720b.png)  
![Lab2-specsjeng_icache](https://user-images.githubusercontent.com/58628111/101296397-f62fa980-382b-11eb-8cdd-fb1f50f887d1.png)  ![Lab2-specsjeng_l2](https://user-images.githubusercontent.com/58628111/101296400-f92a9a00-382b-11eb-8e99-6c095ced9c7c.png)  
Με τα παρακάτω διαγράμματα μπορούμε να δούμε πιο συγκεκριμένα την επίδραση κάθε παραμέτρου στο cpi και τα misses του _470.lbm_.    
![specsjeng-l1-cpi](https://user-images.githubusercontent.com/58628111/102022495-f0881580-3d8f-11eb-9b67-addf7fd1590a.png) ![specsjeng-l1-dmiss](https://user-images.githubusercontent.com/58628111/102022497-f2ea6f80-3d8f-11eb-9d92-107516afd701.png)   
![specsjeng-l1-imiss](https://user-images.githubusercontent.com/58628111/102022499-f4b43300-3d8f-11eb-9711-7ffb3838ea85.png) ![specsjeng-l1-l2miss](https://user-images.githubusercontent.com/58628111/102022501-f67df680-3d8f-11eb-98d4-fbca3b14500d.png)   

![specsjeng-l2-cpi](https://user-images.githubusercontent.com/58628111/102022506-04cc1280-3d90-11eb-8732-d5d059aab967.png) ![specsjeng-l2-dmiss](https://user-images.githubusercontent.com/58628111/102022509-072e6c80-3d90-11eb-9d7f-96ac8859b728.png)   
![specsjeng-l2-imiss](https://user-images.githubusercontent.com/58628111/102022512-0990c680-3d90-11eb-839d-1d13461d188e.png) ![specsjeng-l2-l2miss](https://user-images.githubusercontent.com/58628111/102022513-0a295d00-3d90-11eb-8756-b8400101fa8f.png)   

![specsjeng-assoc-cpi](https://user-images.githubusercontent.com/58628111/102022521-1ad9d300-3d90-11eb-9801-059293936c76.png) ![specsjeng-assoc-dmiss](https://user-images.githubusercontent.com/58628111/102022523-1ca39680-3d90-11eb-8f48-0a26c2197fb0.png)   
![specsjeng-assoc-imiss](https://user-images.githubusercontent.com/58628111/102022525-1dd4c380-3d90-11eb-9445-7b70cdc7f085.png) ![specsjeng-assoc-l2miss](https://user-images.githubusercontent.com/58628111/102022526-1f05f080-3d90-11eb-8ee5-ded202592014.png)   

![specsjeng-cacheline-cpi](https://user-images.githubusercontent.com/58628111/102022538-2fb66680-3d90-11eb-972c-f27f6212501c.png) ![specsjeng-cacheline-dmiss](https://user-images.githubusercontent.com/58628111/102022540-30e79380-3d90-11eb-9623-b598f6c2fd47.png)   
![specsjeng-cacheline-imiss](https://user-images.githubusercontent.com/58628111/102022541-32b15700-3d90-11eb-98c6-97973ec22aa3.png) ![specsjeng-cacheline-l2miss](https://user-images.githubusercontent.com/58628111/102022542-347b1a80-3d90-11eb-876c-6c2f18014522.png)    


***Σχετικά με τις δοκιμές στο _470.lbm_*** : Το συγκεκριμένο benchmark υπολογίζει τη ροή ενός ρευστού σε ένα χώρο.Αρχικά αυξήσαμε τα μεγέθη των L1 instruction cache και L1 data cache καθώς είδαμε ότι την πρώτη φορά που έτρεξε το benchmark η L1d είχε σχετικά υψηλό miss rate.Έπειτα αυξήσαμε την L2 και τα associativity όλων με ελάχιστη διαφορά και η πιο σημαντική βελτίωση ήρθε μετά την αύξηση του μεγέθους της cacheline.   

![Lab2-speclibm_cpi](https://user-images.githubusercontent.com/58628111/101345248-73403a80-388f-11eb-8fd6-e9bbd6e93ab1.png)  ![Lab2-speclibm_dcache](https://user-images.githubusercontent.com/58628111/101345257-7804ee80-388f-11eb-9be2-228e533e080e.png)  
![Lab2-speclibm_icache](https://user-images.githubusercontent.com/58628111/101345268-7a674880-388f-11eb-870e-795aa8b1fe55.png)  ![Lab2-speclibm_l2](https://user-images.githubusercontent.com/58628111/101345293-805d2980-388f-11eb-8a50-6346adaa3a14.png)   
Με τα παρακάτω διαγράμματα μπορούμε να δούμε πιο συγκεκριμένα την επίδραση κάθε παραμέτρου στο cpi και τα misses του _470.lbm_.  
![image](https://user-images.githubusercontent.com/58628111/101932770-f5a86180-3be3-11eb-887d-8c79bcf87f3f.png) ![image](https://user-images.githubusercontent.com/58628111/101932783-f9d47f00-3be3-11eb-8a38-fab280db62ad.png)   
![image](https://user-images.githubusercontent.com/58628111/101932796-fd680600-3be3-11eb-9c83-d8692ac7f76f.png) ![image](https://user-images.githubusercontent.com/58628111/101932808-0062f680-3be4-11eb-9ce6-721a78689d36.png)   

![image](https://user-images.githubusercontent.com/58628111/101932901-1ffa1f00-3be4-11eb-83f8-7b5e111b0266.png) ![image](https://user-images.githubusercontent.com/58628111/101932919-238da600-3be4-11eb-95fc-0bc894a5f306.png)   
![image](https://user-images.githubusercontent.com/58628111/101932931-27212d00-3be4-11eb-8a1d-99195db39b41.png) ![image](https://user-images.githubusercontent.com/58628111/101932945-2be5e100-3be4-11eb-9ca2-816f7d751c59.png)   

![image](https://user-images.githubusercontent.com/58628111/101936672-846bad00-3be9-11eb-8c20-ac32b58b6cb3.png) ![image](https://user-images.githubusercontent.com/58628111/101936688-87ff3400-3be9-11eb-8968-92d999cac048.png)   
![image](https://user-images.githubusercontent.com/58628111/101936698-8d5c7e80-3be9-11eb-97db-93db301199d4.png) ![image](https://user-images.githubusercontent.com/58628111/101936707-92213280-3be9-11eb-9da7-a3c3697bb378.png)   

![image](https://user-images.githubusercontent.com/58628111/101944017-b6ced780-3bf4-11eb-8c30-a3a3989ba791.png) ![image](https://user-images.githubusercontent.com/58628111/101944028-bafaf500-3bf4-11eb-9da4-438726379190.png)   
![image](https://user-images.githubusercontent.com/58628111/101944047-be8e7c00-3bf4-11eb-9d93-e99d7a1ac98a.png) ![image](https://user-images.githubusercontent.com/58628111/101944056-c3533000-3bf4-11eb-86c8-3f48ea2a567f.png)   




***Βήμα 3ο***  
Είναι γεγονός πως η αύξηση του μεγέθους των caches, επιφέρει σημαντικό κόστος στον χρόνο σάρωσης των caches. Παρόλα αυτά παρατηρούμε πως με την αύξηση αυτή έχουμε καλύτερη απόδοση. Σκοπός λοιπόν είναι να βρούμε το σημείο όπου από τη μία, ο χρόνος προσπέλασης δεν αυξάνεται σημαντικά και από την άλλη, οι αστοχίες μειώνονται. Συγκεκριμένα, καθώς ο χρόνος προσπέλασης της L1 είναι πιο κρίσιμος από αυτόν της L2, το κόστος της αύξησης του μεγέθους της είναι μεγαλύτερο.   
Επίσης, σημαντικό είναι να αναφέρουμε πως η cacheline επηρεάζει το CPI, καθώς η λειτουργία της είναι η μεταφορά δεδομένων και εντολών από την κύρια μνήμη σε cache. Όμως, δεν πετυχαίνουμε την βελτίωση της απόδοσης με την συνεχή αύξηση της cacheline, αφού μετά από ένα σημείο μπορεί να επιφέρει αντίθετα και μη επιθυμητά αποτελέσματα (_συμπέρασμα που προέκυψε από το 456.hmmer_).  
Ακόμη, όσο μεγαλύτερο το associativity της κάθε cache, τόσο μεγαλύτερο και το πλήθος των διαφορετικών ετικετών που πρέπει να συγκρίνουμε ώστε να βρούμε αυτό που μας ενδιαφέρει, πράγμα που αυξάνει την πολυπλοκότητα.    

***Κριτική***   
Η εργασία αυτή ήταν πιο ενδιαφέρουσα από την προηγούμενη, 


***Πηγές***  
- www.spec.org   
- Οργάνωση και σχεδίαση υπολογιστών, David A. Patterson & John L. Hennessy  





 



