# utl-fourteen-solutions-to-add-a-column-and-copy-table-to-a-another-folder
Fourteen solutions to add a column and copy table to a another folder.

    Fourteen solutions to add a column and copy table to a another folder                                          
                                                                                                                   
      Changed output destination to another folder                                                                 
                                                                                                                   
      Two Solutions below (plus 14 other possibilities - some require a view)                                      
                                                                                                                   
          1. DOSUBL                                                                                                
          2. HASH with view - assumes 12 datasets are conformable/congruent                                        
                                                                                                                   
    github                                                                                                         
    https://tinyurl.com/ycy5gxvx                                                                                   
    https://github.com/rogerjdeangelis/utl-add-a-variable-to-twelve-datasets-proc-sql-alter-table                  
                                                                                                                   
    For 13 possible other techniques                                                                               
    https://tinyurl.com/y6uzqbm2                                                                                   
    https://github.com/rogerjdeangelis/utl_thirteen_algorithms_to_split_a_table_based_on_groups_of_data            
                                                                                                                   
    SAS-L                                                                                                          
    https://listserv.uga.edu/cgi-bin/wa?A2=SAS-L;3aeca166.1901d                                                    
                                                                                                                   
    INPUT  (12 datasets)                                                                                           
    ====================                                                                                           
                                                                                                                   
    Filename d:\sd1                                                                                                
                                                                                                                   
     #     SAS Table                                                                                               
                                                                                                                   
     1  GASHAW_VW3_201801                                                                                          
     2  GASHAW_VW3_201802                                                                                          
     3  GASHAW_VW3_201803                                                                                          
     4  GASHAW_VW3_201804                                                                                          
     5  GASHAW_VW3_201805                                                                                          
     6  GASHAW_VW3_201806                                                                                          
     7  GASHAW_VW3_201807                                                                                          
     8  GASHAW_VW3_201808                                                                                          
     9  GASHAW_VW3_201809                                                                                          
    10  GASHAW_VW3_201810                                                                                          
    11  GASHAW_VW3_201811                                                                                          
    12  GASHAW_VW3_201812                                                                                          
                                                                                                                   
    run;quit;                                                                                                      
                                                                                                                   
    * create 12 datasets;                                                                                          
    data _null_;                                                                                                   
     do i=1 to 12;                                                                                                 
        call symputx('i',put(i,z2.));                                                                              
        rc=dosubl('                                                                                                
          data "d:/sd1/gashaw_vw3_2018&i..sas7bdat";                                                               
             x=1;                                                                                                  
          run;quit;                                                                                                
        ');                                                                                                        
     end;                                                                                                          
    run;quit;                                                                                                      
                                                                                                                   
    EXAMPLE OUTPUT                                                                                                 
    --------------                                                                                                 
                                                                                                                   
    WORK.LOG total obs=12                                                                                          
                                                                                                                   
      I    RC                 STATUS                                                                               
                                                                                                                   
      1     0    gashaw_vw3_201801 Variable Added                                                                  
      2     0    gashaw_vw3_201802 Variable Added                                                                  
      3     0    gashaw_vw3_201803 Variable Added                                                                  
      4     0    gashaw_vw3_201804 Variable Added                                                                  
      5     0    gashaw_vw3_201805 Variable Added                                                                  
      6     0    gashaw_vw3_201806 Variable Added                                                                  
      7     0    gashaw_vw3_201807 Variable Added                                                                  
      8     0    gashaw_vw3_201808 Variable Added                                                                  
      9     0    gashaw_vw3_201809 Variable Added                                                                  
     10     0    gashaw_vw3_201810 Variable Added                                                                  
     11     0    gashaw_vw3_201811 Variable Added                                                                  
     12     0    gashaw_vw3_201812 Variable Added                                                                  
                                                                                                                   
                                                                                                                   
    PROCESS                                                                                                        
    =======                                                                                                        
                                                                                                                   
    1. DOSUBL                                                                                                      
    ---------                                                                                                      
                                                                                                                   
    %symdel i / nowarn;                                                                                            
    data log;                                                                                                      
                                                                                                                   
     do i=1 to 12;                                                                                                 
        call symputx('i',put(i,z2.));                                                                              
                                                                                                                   
        rc=dosubl('                                                                                                
          proc sql;                                                                                                
            alter table "d:/sd1/gashaw_vw3_2018&i..sas7bdat"                                                       
            add abc num                                                                                            
          ;quit;                                                                                                   
          %let cc=&syserr;                                                                                         
        ');                                                                                                        
                                                                                                                   
        * uses hiddon dragon space;                                                                                
        if symgetn('cc')=0 then status=cats("gashaw_vw3_2018",put(i,z2.)," Variable Added");                       
        else status=cats("gashaw_vw3_2018",put(i,z2.)," Failed Update");                                           
        output;                                                                                                    
     end;                                                                                                          
     stop;                                                                                                         
                                                                                                                   
    run;quit;                                                                                                      
                                                                                                                   
                                                                                                                   
    2. HASH - assumes 12 datasets are conformable/congruent                                                        
    -------------------------------------------------------                                                        
                                                                                                                   
    libname sd1 "d:/sd1";                                                                                          
    libname del "d:/sd1/delete";                                                                                   
                                                                                                                   
    data _null_;                                                                                                   
                                                                                                                   
    if _n_=0 then do; %let rc=%sysfunc(dosubl(                                                                     
       data havAll/view=havAll;                                                                                    
          set sd1.gashaw_vw3_2018: indsname=dsns;                                                                  
          dsn=dsns;                                                                                                
       run;quit;                                                                                                   
       ));                                                                                                         
    end;                                                                                                           
                                                                                                                   
    if _n_=1 then do;                                                                                              
       dcl hash Hoh () ;                                                                                           
          hoh.definekey  ("DSN") ;                                                                                 
          hoh.definedata ("DSN", "h") ;                                                                            
          hoh.definedone () ;                                                                                      
        dcl hash H ;                                                                                               
    end;                                                                                                           
                                                                                                                   
    set havAll end=dne;                                                                                            
                                                                                                                   
    * load the detal data;                                                                                         
      if hoh.find() ne 0 then do;                                                                                  
         h= _new_ hash(multidata:'y');                                                                             
         h.definekey  ("_iorc_") ;                                                                                 
         h.definedata ('DSN','X') ;                                                                                
         h.definedone () ;                                                                                         
         hoh.add();                                                                                                
      end;                                                                                                         
                                                                                                                   
    h.add();                                                                                                       
                                                                                                                   
    dcl hiter hi('hoh');                                                                                           
                                                                                                                   
    if dne then do;                                                                                                
       do while(hi.next()=0); * iterate datasets output detail data;                                               
          h.output(dataset:cats('del.',scan(dsn,2,'.'),'(drop=dsn)'));                                             
       end;                                                                                                        
    end;                                                                                                           
    run;                                                                                                           
                                                                                                                   
                                                                                                                   
                                                                                                                   
                                                                                                                   
