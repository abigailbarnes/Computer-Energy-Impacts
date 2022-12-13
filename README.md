# Computer-Energy-Impacts
This week, there are two tasks, both of the synthesis variety. As usual, you'll be turning in both code and a PDF write-up.

Note that the part of Task 1 in which you measure power usage requires running the Python energyusage Links to an external site.module, which requires an Intel CPU and in our experience only runs on Linux. The CS Department Tech Staff has kindly set up two appropriate machines for the class, and we'll have you ssh into those systems.

They have configured these systems to be accessible only within the UChicago CS network. We are configuring them to be accessible only by ssh key. Thus, we need you to follow the steps in Task 0 ASAP because Blase needs to configure accounts for all students in the class manually.

Task 0: Necessary Prerequisite. Do ASAP! (9 points, but necessary for Task 1!)
Step 1: Connect to linux.cs by running ssh username@linux.cs.uchicago.edu (replacing username with your CNet id).

Step 2: Run cd .ssh

Step 3: Run ssh-keygen

Step 4: When it asks for a file name, type cs259

Step 5: When it asks for a passphrase, just leave it blank (hit enter twice)

Step 6: Type cat cs259.pub

Step 7: Copy and paste the whole output (starting with ssh-rsa and ending with the username, the whole thing) into Qualtrics survey Links to an external site.and submit it.

Note that to sign in to our machines once Blase has manually created your account, you will:

Step 1: Connect to linux.cs by running ssh username@linux.cs.uchicago.edu (replacing username with your CNet id).

Step 2: On linux.cs, run either ssh username@cs25910a or ssh username@cs25910b (replacing username with your CNet id).

Now you're in! Run your code in your home directory. Make sure to chmod 700 all files so that they aren't inadvertently visible to others in the class.

Task 1 (Synthesis): Energy Impacts of Computing (60 points)
In this task, you will first be implementing some of the blockchain proof of work concepts we discussed briefly in class. Implementing these mechanisms will give you a better sense of how the proof of work (and its adjustable threshold) function. Most importantly, you will get a chance to measure the energy consumption of this proof of work with various parameterizations and estimate the energy usage of more realistic parameterizations for real-world blockchain usage.

For this first task, you will simulate a (simplified) experience of mining a block, measure its energy consumption, and calculate a rough estimate of how much energy the entire Bitcoin system is consuming.

For background:

Read Section 2.4 (pages 61-67) of this textbook Links to an external site.. Note that the book simplifies the hash puzzle, but the general principle is the same as in the actual Bitcoin blockchain.

(UPDATED 4/21) Also read Sections 5.1-5.4 (pages 131-157) of the same textbook Links to an external site.to better understand the mining process.

Also read this other description Links to an external site.of the structure of a block.

Finally, take a look at some of the recently mined blocks Links to an external site.on the actual blockchain.

Note that only Parts 1-4 and 1-5 need to be done on the Linux machines we set up for that purpose. All other parts of the assignment should not be done on those machines to avoid excessive resource contention.

Part 1-1 (15 points total)
10 points Use Python 3 (or whatever language you prefer) to implement a blockchain-style proof-of-work mechanism. Specifically, use the inequality from page 64 of the textbook (the first reading) in which you look for a nonce that solves the hash puzzle. Use SHA-256 for the hash function, for which you can use the Python hashlib library Links to an external site.or whatever other library you prefer. In this first implementation, guess the nonce incrementally starting from 0 (i.e., 0, 1, 2, etc...) based on the following parameters:

Previous hash = '00000003ef809ab31c2339e3938349437161a40eb9d19162b0185bc5be78d2f8'

Transactions = '14d65eb004b8fca9fa7873263ddfb0b0f3101be84ebb5f847ac6f9aaf2a17ebf'

Target: For convenience, for this assignment you just need the hash to have 7 leading 0s in hex. (This is equivalent to having a target of 2256-28 = 2228.)

Use base 16 for your nonce.

Note that this computation will take a while. To debug your code, set fewer leading 0s as your target, which will be much faster to solve and debug.

As a reminder, you are not allowed to submit other people's code as your own, including from the internet. Specifically, you may not reference Python implementations of the proof of work mechanism. However, referencing Python documentation, StackOverflow, or other sites to see how to do low-level things in Python (e.g., string manipulation, using different libraries) is okay. Just do not consult sources that implement the proof of work for you.

Submit your code and also answer the following question:

Write-up Question 1 (5 points): What was the final, successful nonce you got?

Part 1-2 (10 points total)
5 points Create a second Python script that guesses the nonce again, but instead of guessing sequentially, it randomly Links to an external site.samples the possible values between 0 (inclusive) and 2256 (exclusive) with uniform probability. Run this 5 times.

Submit your code and also answer the following question:

Write-up Question 2 (5 points): Report the average number of hashes it took for this approach to find a nonce that solves the hash puzzle. How does this compare to the incremental approach?

Part 1-3 (5 points total)
Write-up Question 3 (5 points): Would it be more advantageous to a) guess the nonce values randomly, b) guess incrementally with initial_nonce being 1, or c) guess incrementally with initial_nonce being the nonce of the previous block? Make sure to explain why you came to this conclusion. To answer this question, you should consider what you found in the previous questions, the sources we provided, and the fact that each miner (or mining pool) on the blockchain network has their own wallet address (where the bitcoins would go if they receive any).

Part 1-4 (10 points total)
5 points Now, use the energyusage Links to an external site.Python library to measure the energy it takes for each hash when mining this same block.

Note that your answer should be for a single hash. However, you will want to measure more than a single hash computation. A lot of one-time costs go into computing a single hash. For instance, starting up the Python interpreter would be included in your measurement. Thus, you'll want to measure the energy usage for a large number of hash computations and then divide your result by the number of hash computations that were made. Note also that you don't need to solve the proof of work puzzle to answer this question. You just have to run the same process, counting the number of hashes computed and the amount of energy usage to calculate that many hashes, then divide. You should probably repeat this procedure for different numbers of hashes computed to better estimate the energy consumption of a single hash.

If you have a Linux machine with an Intel CPU and/or an NVIDIA GPU, check that /sys/class/powercap/intel-rapl:*/energy_uj exists. If so, make it user-readable (with chmod) before running your proof-of-work program. Alternatively, you can run your Python script as root (sudo). Note that this library only works on Linux, not Mac or Windows.

Submit your code and also answer the following question:

Write-up Question 4 (5 points): Report your results in Joules, with scientific notation rounded to three decimal places (e.g. 1.234 x 10-2). How fast was your machine (i.e., how many hashes per second)? If you used your own machine, also briefly describe its specs (e.g., "IBM Thinkpad laptop running Ubuntu 20.04 with an Intel CORE i7 processor"). If you used the machines we set up, say so.

Part 1-5 (20 points total)
5 points About every two weeks, Bitcoin changes the target such that the rate of successfully mined blocks is about once every 10 minutes. Vary the target (i.e. the number of leading 0s) and graph how much the expected energy consumption in Joules would be. Note that it will take a long time to mine blocks with small targets; at some point, it will take longer than the time you have for this assignment! Therefore, you should estimate these values manually and plot them.

Submit your code and also answer the following question:

Write-up Question 5 (5 points): In your write-up, include your graph and indicate which points were computed in your experiment and which are estimates.

Assume every node has your same computational power (from Part 1-4) and the overall Bitcoin network is performing over 220 million trillion (2.2 x 1020) hashes per second according to this graph Links to an external site..

Write-up Question 6 (5 points): How many nodes would the Bitcoin network need to have if making the assumption that everyone is running a machine with equivalent computational power to your computer? How much energy total would be consumed (in watts)?

In reality, Bitcoin miners use machines that are more computationally powerful than the ones we provided (and probably the ones you have). Take a look at this page Links to an external site..

Write-up Question 7 (5 points): Assuming every node is an Antminer S19 (95 Terahashes per second, 3250 Watts), and the same overall rate of hashes per second from the previous question (220 million Terahashes) applies here, how many nodes would we expect the Bitcoin network to have? How much energy total would be consumed (in Watts)?

Task 2: Synthesis: Character Encodings (40 points)
As we discussed, the state of character encodings (how we represent in computer-intelligible form different alphabets and scripts) was a gigantic mess for decades. Thankfully, the web has mostly standardized on UTF-8 encoding in recent years. In this exploration task, we will explore the character encodings, languages, and specific alphabets and scripts used on popular websites, particularly through a web scrape you will conduct. This tutorial Links to an external site.gives a nice overview of using Python to scrape and parse websites via the urllib.request and beautifulsoup libraries, as does this much shorter tutorial Links to an external site..

To represent different characters in bits requires having a mapping of bits to characters. In the early days of computing, the ASCII standard Links to an external site.was developed based on the English alphabet. Shockingly, ASCII is still widely used, even in places it shouldn't be. Instead, in modern times we all should be using UTF-8 Links to an external site.. This tutorial Links to an external site.gives a pretty accessible overview of UTF-8.

Note that this task involves non-trivial web scraping, and you'll want to be smart about your approach so that you're not sitting there for hours waiting on your code to run only to find a small error invalidates all of that computation and network usage. Rather than re-running your scraping code each time, you might want to run your scrape once and cache the results. Similarly, you might want to test your approach on a small number of sites before running it on the whole thing. You also might want to have a way to pick up where you left off if your internet drops out. You should also be prepared to handle webpages that throw errors, appear offline, or otherwise can't be scraped; you will likely encounter such pages.

Write-up Question 8 (8 points): The file tranco_L6X4-1m.csv.zip contains a zipped version of the Tranco list Links to an external site.of the top million websites. Scraping a million websites would take too long; take what you believe to be an appropriate sample of this list. As your first task, prepare this scrape. You might decide to download all of these pages locally. You might instead decide to complete the rest of the sub-tasks and then run your scrape in real time. Both are valid approaches. In your write-up, briefly describe your approach to sampling the top million webpages.

Write-up Question 9 (8 points): The W3C internationalization guide Links to an external site.notes two acceptable ways to indicate that UTF-8 is being used. What fraction of webpages you scraped (i) used the first, shorter method; (ii) used the second, longer method; (iii) indicated some different character encoding; (iv) didn't indicate the character encoding?

Write-up Question 10 (8 points): The Mozilla Develop Network guide Links to an external site.gives a preferred and a discouraged way for denoting the language of a page. What fraction of webpages you scraped (i) used the preferred method; (ii) used the discouraged method; (iii) didn't indicate the language of the page?

Write-up Question 11 (8 points): Create a table indicating the fraction of pages in your scrape in different languages, from the most to the least prevalent. Use the actual name Links to an external site.of the languages, in addition to the two-character code and its frequency, in your table. Ignore pages that did not specify the language, but be sure to handle any cases where multiple languages were specified for a page. Include this table in your write-up.

Write-up Question 12 (8 points): What were the most common UTF-8 characters themselves used on the pages you visited? Create a table indicating the most common UTF-8 characters in your scrape, from the most to the least prevalent. Pick a sensible cut-off fraction for "most common" since the full list would get really long. Use the unicodedata function Links to an external site.to include the official "name" of each character, in addition to the character itself and its frequency, in your table. Include this table in your write-up.
