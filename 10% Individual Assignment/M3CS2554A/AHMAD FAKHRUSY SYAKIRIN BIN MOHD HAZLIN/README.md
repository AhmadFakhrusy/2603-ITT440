# ANALYTICS FOR YOUTUBE CREATORS
# STUDENT'S NAME : AHMAD FAKHRUSY SYAKIRIN BIN MOHD HAZLIN
# STUDENT'S ID : 2024642602# YOUTUBE VIDEO LINK : https://youtu.be/uExROcL1Z6c

# PROBLEM STATEMENTS
The exponential growth of YouTube metadata poses a significant challenge for creators who need to analyze audience behavior in real-time.
- High Latency in I/O Operations: Sequential scripts spend a majority of their execution time waiting for data to be read from large CSV files or retrieved via network requests, leading to CPU idling.
- Computational Inefficiency: Analyzing millions of engagement records (likes, views, and affection rates) using a single-threaded approach fails to utilize the multi-core capabilities of modern computing architectures.

# OBJECTIVES
- Performance Optimization: To design a Python-based analytical engine that utilizes Concurrency and Parallelism to reduce data processing time.
- Comparative Analysis: To evaluate the execution time differences between sequential, multi-threaded, and multi-processed approaches for YouTube dataset analysis.
- Data Aggregation: To accurately calculate audience metrics across large-scale datasets.

# PROJECT SCOPE- Dataset: High-volume YouTube creator data stored in structured formats.
- Implementation: Developed using Python, specifically utilizing the threading and multiprocessing libraries.
- Functionality: The system focuses on back-end data crunching and performance benchmarking rather than front-end visualization.

# IMPLEMENTATION
- Sequential
- Concurrent
- Parallel

# DIFFERENCE OF THE IMPLEMENTATION
- SEQUENTIAL- program executes one instruction at a time.
- CONCURRENT- program doing multiple things at once.
- PARALLEL- simultaneous execution of multiple tasks.

# CODE STRUCTURE
<img width="457" height="164" alt="image" src="https://github.com/user-attachments/assets/acb658a5-419f-4eda-9f2c-7ffc80a4565e" />

<img width="410" height="124" alt="image" src="https://github.com/user-attachments/assets/ea102271-3f04-4b6e-aa0f-1d425ec7fc35" />

# KEY FUNCTION
- generate_youtube_data(): Simulates a massive dataset (50 million records)
- build_analytics_db(data): Organizes the raw data into a structured format.
- get_global_rankings(db): Iterates through the entire database to calculate the Top 10 Videos and Top 10 Channels based on total watch time.
- analyze_channel(channel_id, db): It calculates specific metrics for a single channel, including total hours watched, total video count, and identifying the top video for that specific creator.
- run_sequential(db, targets): Processes the analysis of target channels one by one in a linear fashion.
- run_concurrent(db, targets): It manages concurrency by interleaving tasks, which is useful for managing overhead.
- run_parallel(db, targets): Distribute the analysis across all available CPU cores.

# DATA TABLE
<img width="678" height="165" alt="image" src="https://github.com/user-attachments/assets/190e3ac4-01b6-45e9-9d31-4300c8c8f87a" />

# EXPECTED OUTPUT
<img width="676" height="857" alt="image" src="https://github.com/user-attachments/assets/1d9c0717-e2bd-4465-afeb-627b11fba3ca" />
<img width="677" height="163" alt="image" src="https://github.com/user-attachments/assets/39c50045-7b9b-488d-b097-f25284ea3c25" />

# SUMMARY
- Sequential vs. Concurrent: The sequential method performed slightly better than the concurrent method (0.023s vs 0.029s). This often occurs with very small tasks or datasets because the overhead of creating and managing threads exceeds the time saved by interleaving the tasks.

- Parallel Performance Anomaly: The parallel execution was significantly slower, taking over 42 seconds compared to the fractional seconds of the other methods.

- The "Overhead" Factor: The Parallel Processing (multiprocessing) carries a heavy "startup cost". Because multiprocessing must create entirely new instances of the Python interpreter and copy data into new memory spaces, it is inefficient for tasks that are already very fast to complete sequentially.

- Resource Management: For this specific test case, the baseline sequential method is the most efficient choice. Parallelism would
likely only show a positive "Speedup" if the computational workload per task was much larger, justifying the 42-second management overhead.
