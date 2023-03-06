# Starve-free-readers-writers-problem
The reader-writer problem is a classic synchronization problem in operating systems , the main challenge is to ensure that the shared resource is accessed correctly and efficiently while avoiding starvation. Specifically, we need to prevent concurrent access by multiple writers, which could result in data inconsistencies, while still allowing concurrent access by multiple readers to improve system performance.we will be solving this challenge by making use of semaphores.
## shared data
shared data can be files, data base e.t.c
## Semaphores Used
                 1)mutex    //binary semaphore which ensures changing of read_count atomically
                 2)wrt      //binary semaphore which ensures the mutual exclusion of writer processes
                 3)decider_mutex  //binary semaphore which ensures equal priority to reader and writer processes that result in avoidance of starvation
                 
## initialization of used variables      
                                1) mutex=1
                                2) wrt=1    
                                3)decider_mutex=1  
                                4)read_count=0  //indicates the no.of readers reading the file at that instant
 ## WAIT and SIGNAL function declaration
 ``` cpp
 WAIT(Semaphore *S){
                      S->value--;
                       if(S->value<0)
                                      { add this process P to Combined_Queue;
                                              block();}
                  }
                                     
                    

  SIGNAL(Semaphore *S){
                      S->value--;
                       if(S->value<0)
                                      { Remove a process P to Combined_Queue;
                                              wake up(P);}
}
```                                     
                      
                      
# starve free Solution:
##### we are using decider_mutex semaphore to make sure mutual exclusion between each of (readers plus writers) i.e whether reader have to enter into the critical section or writer ,and i am making use of wrt semaphore to make sure mutual exclusion between readers and each of writers, so i am calling wait fuction when reader enters into critical section and releasing semaphore when read count becomes zero and i am using mutex semaphore to ensure mutual exclusion between only writers seperately.


## Pseudo code for Starve-free writers process
``` cpp
WAIT(decider_mutex);
                  WAIT(wrt);

                 --------------------------
                 
                   writing is performed   // **control section** //
                 
                 --------------------------
                  SIGNAL(wrt);
                  
                  SIGNAL(decider_mutex);
```                  
## Pseudo code for Starve-free readers process
``` cpp
                  WAIT(decider_mutex);
                  
                  WAIT(mutex);
                  
                  if(read_count==1)
                    WAIT(wrt);
                    
                    SIGNAL(mutex);
                    
                    SIGNAL(decider_mutex);
                    -------------------------------

                          reading is performed  // **control section** //

                    -------------------------------
                    WAIT(mutex);
                    
                    if(read_count==0)
                         SIGNAL(wrt);
                         
                         SIGNAL(mutex);
 ```                        
                         

