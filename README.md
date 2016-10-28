# SortingCompetitionMaterials2016

Materials for UMM CSci 3501 "Algorithms and computability" 2016 sorting competition.

## Goal of the competition

The Sorting Competition is a multi-lab exercise on developing the fastest sorting algorithm for a given type of data. By "fast" we mean the actual running time and not the Big-Theta approximation. The solutions are developed in Java and will be ran on a single processor.

## The data

The data file starts with coordinates of two points on a square grid of integers that we will refer to as *reference points*. The size of the grid is referred to as `gridSize` and will be between `50000` and `200000`. Each point is given on its own line; its X coordinate followed by the Y coordinate, separated by spaces. 

The data to be sorted consists of triples of integers. The first two elements are X and Y coordinates of each point to be sorted, followed by a timestamp, which also can be viewed just as the number of the point in the sequence, starting with 0 and up `N - 1` (where `N` is the total number of points to be sorted). 

## How do you need to sort the data 

The points are sorted based on their distance to **the closer** of the two reference points. If the distance is smaller than the threshold `0.0000000001`, it is considered zero. If the two points being compared have the difference of their distances to the closest reference point within the treshold, they are considered to be at the equal distance. In this case the point with the smaller timestamp is considered smaller. 

## How is the data generated

The points are generated as a sequence of *walks*. The first walk starts at the middle of the grid, subsequent walks start at the end point of a previous walk. The walks are generated until the desired number of points (denoted `N`) is generated. Each walk is generated as follows:
* The length of the walk (the number of points in it) is randomly generated between `walkMin` and `walkMax`. 
* A random direction is generated. Its angle is a double between 0 and 360, where 0 indicates the positive direction of X coordinate.
* A random *speed* is generated as an integer between 0 and `maxSpeed`.
All distributions are uniform, including the lower bound and excluding the upper one. 

Walk points are generated by "moving" in the chosen direction by the chosen step. If a generated coordinate is negative or over the `gridSide`, the motion is "wrapped around": `gridSide` is added to negative coordinates and subtracted from a coordinate greater than `gridSide`. The walk continues from that point. 

The points of the walk are recorded as a triple of integers: the `X` coordinate, the `Y` coordinate, and the timestamp which is simply teh number of the point in the sequence (the start point of the first sequence has timestamp `0`, and is incremented by `1` for every point). The triples are written out one per line, as integers separated by spaces, in the order in which sequences were generated.  

The last walk may be shorter than its randomly generated length so that the total number of points is exactly `N`.  

The changeable parameters to the data generation are as follows: 
* `gridSize` can be between `50000` and `200000` (inclusive)
* Number of points can be between `100000` and `1000000`
* Minimum walk length can be between `1` and `100`
* Maximum walk length can be between `100` and `1000`
* Maximum speed can be between `50` and `300`. The minimum speed is always zero. 
**TBD**: add the ability to set parameters for data generator from the command line. 

Once the data is generated, it is written out to the output file (the name supplied on the command line for the data generator). Here is a beginning of data file:
```
23597 83108
67067 36505
50000 50000 0
49950 50026 1
49900 50052 2
49850 50078 3
49800 50104 4
49750 50130 5
49700 50156 6
49650 50182 7
49600 50208 8
```
Here the two reference points are `(23597, 83108)` and `(67067, 36505)`, and the sequence of points starts at `(50000, 50000)` with a  timestamp `0`, and continues from there. 

See the details of data generator in [DataGenerator2016.java](src/DataGenerator2016.java) file. 

For complete sample data file see [data1.txt](data1.txt). Its sorted output is in [out1.txt](out1.txt) file. 

## Setup for sorting
The file [Group0.java](https://github.com/elenam/SortingCompetitionMaterials2015/blob/master/src/Group0.java) provides a template for the setup for your solution. Your class will be called `GroupN`, where `N` is the group number that is assigned to your group. The template class runs the sorting method once before the timing for the [JVM warmup](http://alexandru-ersenie.com/2010/09/12/important-aspects-in-load-performance-testing-1-server-warm-up/). It also pauses for 10ms before the actual test to let any leftover I/O to finish. Since the warmup and the actual sorting are done on the same array (for no reason other than simplicity), the array is cloned from the same input data. 

The data reading, the array cloning, the warmup sorting, and writing out the output are all outside of the timed portion of the method, and thus do not affect the total time. 

Note that you may not use any global variables (other than the coordinates of the reference points that are already initialized for you; you may not change them). 

The only method in the [Group0.java](src/Group0.java) files that you may modify is the `sort` method. It must take the array of points, each of which is in turn an array of 3 integers. The return type of the method can be what it is now, which is the same as the parameter type `int [][]`, or it can be a different array type. If you are sorting in-place, i.e. the sorted result is in the same array, then you can just return a reference to that array, as my prototype method does. If you are returning a different type of an array, the following rules have to be followed:
* Your `sort` method return type needs to be changed to whatever  array you are returning, and consequently the type of `sorted` array in `main` needs to be changed. 
* Your return type has to be an array (not an array list) and it has to have the same number of elements as the original array, one point per element. 
* You need to supply a function to write out your result into a file. The file has to be exactly the same as in the prototype implementation; they will be compared using `diff` system command. 

If you are not changing the return type, you don't need to modify anything other than `sort` method. 

Even though you are not modifying anything other than the `sort` method, you still need to submit your entire class: copy the template, rename the Java class to your group number, and change the`sort` method. You may use supplementary classes, just don't forget to submit them. Make sure to add your names in comments when you submit. 

**Important:** if the sorting times may be too small to distinguish groups based on just one run of the sorting, so I may loop over the sorting section multiple times. If this is the case, I will let you know no later than a day after the preliminary competition and will modify `Group0` file accordingly.  

## Dates:

*Friday, Oct 7* in class (1pm) is the *preliminary* competition. Please send me all your materials no later than 10:30am on Friday. This is required for everyone in the class. Groups remain anonymous after this phase, but all the solutions (in bytecode) become available. 

We may have a second preliminary competition some time the week of Oct 10. 

*Thursday, Oct 20* in the lab (2pm) is the *final* competition. All source code is posted immediately after that. Those in class will have their names revealed, others may choose to remain anonymous (but the code will still be posted). 

Note that there are several more parts of the Algorithms assignment, including presentations and a write-up. Obviously, these are only for students in the class. 

### Scoring

The programs are tested on a few (between 1 and 3) data sets. For each data set each group's program is run three times, the median value counts. The groups are ordered by their median score for each data file and assigned places, from 1 to N. 

The final score is given by the sum of places for all data sets. If the sum of places is equal for two groups, the sum of median times for all the runs resolves the tie. So if one group was first for one data set and third for the other one (2 sets total being run), it scored better than a group that was third for the first data set and second for the other. However, if one group was first for the first set and third for the other one, and the second group was second in both, the sum of times determines which one of them won since the sum of places is the same (1 + 3 = 2 + 2). 

If a program has a compilation or a runtime error, doesn't sort correctly, or prints anything other than the total time in milliseconds, it gets a penalty of 1000000ms for that run. 

## System specs

The language used is Java 8 (as installed in the CSci lab). It's ran on a single CPU core.  

I will post a script for running this program (with a correctness check and all), but for now a couple of things to know: run your program out of `/tmp` directory to avoid overhead of communications with the file server, and pin your program to a single core, i.e. run it like this:
``taskset -c 0 java GroupN``

## Results of the competition

Congratulations to the winners and thanks to all for participating! 

* Group 2, Skye and Jocelyn:  The sum of places is 2, the sum of medians is 55.0. * Disqualified since they forgot to add deep cloning*
* Group 12 Aaron Lemmon (not in the class): The sum of places is 4, the sum of medians is 133.0 *1st Place overall*
* Group 10 Dan Frazier, Mitch Finzel: The sum of places is 6, the sum of medians is 181.0 *1st Place in class*
* Group 9 Ben Sixel, Jack Ziegler: The sum of places is 10, the sum of medians is 330.0 *2nd Place in class*
* Group 5 Laverne Schrock, Tony Song: The sum of places is 10, the sum of medians is 334.0 *3rd Place in class*
* Group 6 Humza Haider, Shawn Seymour: The sum of places is 10, the sum of medians is 345.0
* Group 1 TM, Sam Miller: The sum of places is 16, the sum of medians is 719.0
* Group 4 Kyle Hakala, Zach Litzinger: The sum of places is 17, the sum of medians is 688.0
* Group 8 Elsa Brwoning, Ai Sano: The sum of places is 18, the sum of medians is 731.0
* Group 7 Emily Schaefer, Sophia Mitchellette, Sydney Richards: The sum of places is 22, the sum of medians is 1408.0
* Group 3 Daniel W.: The sum of places is 24, the sum of medians is 1745.0
* Group 11: Richard Stangl, Jacob Mitchell: The sum of places is 26, the sum of medians is 2000000.0

## Correctness analysis

Each group is assigned another group to check whether that group followed all the rules as spefcified above and in clarifications sent to the class list. If any issues are reported, the group in question can reply if they think they have followed the rules. 

The correctness analysis reports are due Monday Oct 24, and the responses (if any) are due Wedn Oct 26. After this process completes, the competition results are final. 

## Presentations

Presentations will be in the lab Th Nov 3 at 2pm. 

You need to prepare a 3-5 minute presentation about your algorithm. Prepare 3-5 slides about your algorithm in PDF format. Once your presentation is done, either issue a pull request to add it to this directory of the repo, or send it to me by email. The final time to submit it is Wedn Nov 2 at 11:59pm. 

Your presenation has to address:

1. Group number and your place; whether your algorithm sorted correctly at the competition.
2. How the data was represented in your program (Strings, arrays, your custom types, etc).
3. What algorithms were used for sorting? Be clear about what they were applied to. If they were modified from the classical (CLRS) implementation in any way (or if they are not listed in CLRS), describe the changes or the algorithm.
4. State the running time is in terms of Big-theta of the worst case. If you are making a claim that your time on the given data is in a different Big-theta class than the worst case, please explain why. Also, estimate constants by the number of times the array gets traversed.
5. List all of the non-constant memory used in addition to the given array and what it is used for.
6. Address any correctness concerns that you get from the group that checked correctness of your algorithm. If you agree with their analysis then simply say so.
7. If you would like, mention what you would have changed to make the program run faster.

Keep in mind that lists and tables work better for slides than long sentences.

Please take notes when listening to the presentation; you will need those for the final write-up.

## Final write-up requirements.

The final write-up is individual work. 

The final write-up has two parts: the description of your algorithm (if you presented on another group's algorithm then of their algorithm), and a summary of what algorithms overall turned out to be the most effective for this problem and why.

Each of the two parts should be about a page long.

The algorithm description should follow the guidelines for the presentation, only be more detailed. Examples (of data storage, etc.) help.

The algorithm comparison should list all approaches that were used for the problem by all teams (e.g. "three groups used variations of radix sort") and their comparative effectiveness. Note that data represntation plays a significant part in efficiency. 

Please summarize what have you learned through the competition.

The final write-up needs to be submitted to me by the end of the day Friday Nov 11 as a google or a pdf file, by email. 

### Credits

Thanks to Kevin Arhelger who suggested the starting idea and to Stephen Adams for an independent, somewhat similar, suggestion. Also thanks to Peter Dolan for a helpful conversation. 





