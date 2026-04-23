# AHMAD FAKHRUSY SYAKIRIN BIN MOHD HAZLIN

## Problem Statement

As a YouTube analytics platform grows, it must process millions of data to provide creators with actionable insights. The technical challenges include :

Computational Latency :                                                                            
Analyzing watch time and top performing videos across 50,000,000 rows can take several minutes if processed sequentially.

The Python GIL Bottleneck :                                                        
Python’s Global Interpreter Lock prevents standard threads from performing mathematical calculations in parallel, rendering basic multi-threading ineffective for heavy data crunching.

Memory Exhaustion :                                                      
Storing raw data for 50 million events can exceed 8GB of RAM, leading to system instability during the aggregation phase.

## Sequential Code Segment 

def run_sequential(db, targets):                                                
    print(f"[MODE] Sequential...")                                              
    start = time.time()                                       
    results = [analyze_channel(t, db) for t in targets]                                             
    return results, time.time() - start                                         

## Concurrent Code Segment

def run_concurrent(db, targets):                             
    print(f"[MODE] Threading...")                                     
    start = time.time()                                       
    with ThreadPoolExecutor(max_workers=4) as executor:                                       
        results = list(executor.map(lambda t: analyze_channel(t, db), targets))                                      
    return results, time.time() - start                                

## Parallel Code Segment

def analyze_wrapper_parallel(channel_id):                                        
    return analyze_channel(channel_id, _channel_db_ref)                                             

def run_parallel(db, targets):                                   
    print(f"[MODE] Multiprocessing...")                                        
    start = time.time()                                      
    # High workers for high core count CPUs                                    
    with ProcessPoolExecutor(max_workers=multiprocessing.cpu_count(),                                  
                             initializer=init_worker,                                         
                             initargs=(db,)) as executor:                                  
        results = list(executor.map(analyze_wrapper_parallel, targets))                                   
    return results, time.time() - start

## Analysis 

To provide instant lookup times for creators, the system transforms raw logs into a Nested Hash Map.                                                    

Primary Key : Channel ID                                                     
Secondary Key : Video ID                                       
Value : Accumulated Watch Time                                         

The system was tested using three distinct execution paths to find the most efficient processing bridge.                                                                                     

Sequential :                                                               
Processed one channel at a time. While stable, it leaves 90% of modern CPU power idle.                                                                         

Concurrent :                                                                                                    
Ideal for tasks like downloading data, but for YouTube Analytics, it failed to provide speedup because the CPU cannot perform the math for two threads at the exact same millisecond due to the GIL.           

Parallel :                                                                                                
The most effective solution. By spawning a separate "worker" for each CPU core, the system bypasses the GIL.

## Conclusion

The project successfully demonstrates that for CPU-bound tasks like YouTube watch-time analytics.                

Parallelism is mandatory at scale :                                                                     
At 50 million records, multiprocessing is the only viable path to meaningful speedup, provided the system has sufficient RAM.            

Threading is inefficient for math : 
It is because the analytics involve heavy summation and sorting, threading offers negligible benefits over Sequential processing due to the GIL.                                                            

Memory management is the silent killer :                                                                                     
The most critical parts of the code are not the analytics themselves, but the data cleanup and the efficient sharing of the database reference across cores.
