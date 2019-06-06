# utl-DOW-remove-groups-with-at-least-one-missing-but-keep-remaining-groups-with-one-or-two-children
Remove groups with at least one missing but keep remaining groups with one or two children
    DOW Remove groups with at least one missing but keep remaining groups with one or two children                                           
                                                                                                                                             
    github                                                                                                                                   
    https://tinyurl.com/y3nxqg8t                                                                                                             
    https://github.com/rogerjdeangelis/utl-DOW-remove-groups-with-at-least-one-missing-but-keep-remaining-groups-with-one-or-two-children    
                                                                                                                                             
    SAS Forum (for sql solutions - DOW is more flexible - and fast due to caching?)                                                          
    https://tinyurl.com/y69q4lcr                                                                                                             
    https://communities.sas.com/t5/SAS-Programming/Remove-group-by-conditions/m-p/564056                                                     
                                                                                                                                             
    *_                   _                                                                                                                   
    (_)_ __  _ __  _   _| |_                                                                                                                 
    | | '_ \| '_ \| | | | __|                                                                                                                
    | | | | | |_) | |_| | |_                                                                                                                 
    |_|_| |_| .__/ \__,_|\__|                                                                                                                
            |_|                                                                                                                              
    ;                                                                                                                                        
                                                                                                                                             
    data have;                                                                                                                               
      input family children;                                                                                                                 
    cards4;                                                                                                                                  
    1 1                                                                                                                                      
    1 .                                                                                                                                      
    1 2                                                                                                                                      
    1 3                                                                                                                                      
    2 2                                                                                                                                      
    2 3                                                                                                                                      
    3 3                                                                                                                                      
    3 3                                                                                                                                      
    4 1                                                                                                                                      
    ;;;;                                                                                                                                     
    run;quit;                                                                                                                                
                                                                                                                                             
                                                                                                                                             
    Up to 40 obs WORK.HAVE total obs=9                                                                                                       
                                                                                                                                             
                             | Rules                                                                                                         
      FAMILY     CHILDREN    |                                                                                                               
                                                                                                                                             
         1           1       | Remove group because of missing                                                                               
         1           .       |                                                                                                               
         1           2       |                                                                                                               
         1           3       |                                                                                                               
                                                                                                                                             
         2           2       | Keep group because no missing                                                                                 
         2           3       | and we have a 2                                                                                               
                                                                                                                                             
         3           3       | Remove because no 1 or 2                                                                                      
         3           3       |                                                                                                               
                                                                                                                                             
         4           1       | Keep no missing and a 1                                                                                       
                                                                                                                                             
    *            _               _                                                                                                           
      ___  _   _| |_ _ __  _   _| |_                                                                                                         
     / _ \| | | | __| '_ \| | | | __|                                                                                                        
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                         
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                        
                    |_|                                                                                                                      
    ;                                                                                                                                        
                                                                                                                                             
    WORK.WANT total obs=3                                                                                                                    
                                                                                                                                             
    Obs    FAMILY    CHILDREN                                                                                                                
                                                                                                                                             
     1        2          2                                                                                                                   
     2        2          3                                                                                                                   
     3        4          1                                                                                                                   
                                                                                                                                             
    *                                                                                                                                        
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                       
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                      
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                      
    | .__/|_|  \___/ \___\___||___/___/                                                                                                      
    |_|                                                                                                                                      
    ;                                                                                                                                        
                                                                                                                                             
                                                                                                                                             
    data want;                                                                                                                               
                                                                                                                                             
     do _n_=1 by 1 until (last.family);                                                                                                      
        set have;                                                                                                                            
        by family;                                                                                                                           
        if missing(children)  then flgMis=1;                                                                                                 
        if children in (1,2)  then flgKid=1;                                                                                                 
     end;                                                                                                                                    
                                                                                                                                             
     do _n_=1 to _n_;                                                                                                                        
       set have;                                                                                                                             
       if flgMis then continue;    /* cannot use 'then delete' goes to top */                                                                
       else if flgKid then output; /* interesting logic? */                                                                                  
     end;                                                                                                                                    
                                                                                                                                             
     flgMis = 0;                                                                                                                             
     flgKid = 0;                                                                                                                             
                                                                                                                                             
     drop flg:;                                                                                                                              
                                                                                                                                             
    run;quit;                                                                                                                                
                                                                                                                                             
