# PhoneNet to LocalTalk Cable

Because PhoneNet and LocalTalk use differential signalling and only transitions on the bus are significant, rather than levels, I don't expect that reversing this pinout would cause a problem.  Nonetheless, this is the actual pinout of a PhoneNet to LocalTalk cable I have.


```
         6P4C                    Mini-DIN-3
              123456   
    __       ________  
 __|  |__   | |||||| |       .--_--.       .--_--.  
|        |  |.------.|      /   3   \     /   3   \ 
|  oooo  |  ||      ||     ( 2     1 )   ( 1     2 )
|_||||||_|  |'------'|      \_  '  _/     \_  '  _/ 
  123456    |__|  |__|        '---'         '---'   
               |  |         
 (front)      (top)         (female)       (male)    
```

 
| 6P4C | Mini-DIN-3 |
| ---- | ---------- |
| n/c  | 3          |
| 2    | 2          |
| 3    | n/c        |
| 4    | n/c        |
| 5    | 1          |
