---
title: GPGPU With Cellular Automata & A*
date: 2016-01-10 19:25:29
---

# Introduction
For my dissertation I had decided that I wanted to take a closer look at how algorithms can be sped up by the use of GPGPUs, and wanted to truly explore how they work and what their limits were when used within games. To do this I looked at a number of different GPGPU libraries and tried to pick a library that would both be simple to understand but also powerful enough to give back meaningful data.

# Libraries (SDKs)
The libraries that I tested within my dissertation were ATI Stream 1.4, OpenCL (using the only OpenCL library available at the time, ATI Stream 2.0), CUDA & Brahma. In order to fairly compare these libraries I devised a simply test that would both test the libraries speed of execution as well as their ease of use. I came up with the idea of just simply completing one matrix addition that would add the values of two matrices together to produce a constant outcome, which I would be able to check against the correct answer. All of the libraries had their strengths and weaknesses and after a number of headaches brought on by all sorts of problems from linker errors in visual studio to destroying 2 GPUs, I was finally able to produce a number of graphs and figures to help pick the most suitable library and below are some are the graphs that were produced in the process.

![A graph comparing the execution time of a number of different GPGPU libraries.](images/diss_execution_time.png)

As you can see there was a major difference between the speed of execution on the  libraries, ranging from 19ms to less than a millisecond showing just how important it is to test a number of solutions when it comes to the most efficient GPGPU library. I’d just like to point out in favour of ATI Stream 1.4 that I think that this slow execution rate is due to that the first call to the GPU initialises the device ready for stream processing (in retrospect, I would have run a number of iterations and returned an average speed of execution).

Another thing to point out is the fact that both ATI Stream libraries allowed me to perform the calculations on a CPU with the same code and as you can see both times the CPU is actually faster at calculating the answer to this, which is down to the fact that the memory has to be copied across to the GPU before executing.

![A comparison of the source code lines used within each library](images/diss_source_code.png)

Now we have the comparison of the amount of source code lines, which I felt gave a fair view of how complicated the development of the application would be (the more lines of source code there is, the more likely that a bug will arise). As you saw in the last image ATI Stream 2.0 clearly won when it came down to speed of execution, however it was also the most complicated application with over 6 times more lines of code than its closest rival, CUDA, I’m putting this down to the library being relatively new as it was still in beta when I was testing these and therefore I’m sure there will be a lot of things that could eventually be removed.

The clear winner in this category was Brahma which is the only non C/ C++ library I tested, and in fact it was a breeze to code this simple application, however I’m not so sure it has the capabilities of the other libraries as it wasn’t developed by a big company but rather a solo programmer, named Ananth.

# Library Choice
After seeing these results I decided that it would probably be best to go for the middle man in this, a library which wasn’t the most complicated but also not the quickest. This fell down to using either ATI Stream 1.4 or CUDA both of which were both relatively quick (as long as you take into account that ATI Stream 1.4 was both executing and initialise the GPU in the speed test) as well as a small amount of lines of code. While I was writing these applications I very much liked both of these libraries but CUDA had an extra edge over ATI Stream 1.4 because it seems to have a much bigger following and that meant a lot more documentation and help would be available if I got into trouble when developing the final application.

# Algorithms

I looked at a number of algorithms that could possibly be improved with the use of GPGPUs, these were A*, HPA* and cellular automata. I looked specifically at whether the algorithms could be split into multiple arrays of data and be performed on in a parallel fashion as this I felt would give the biggest performance boost when calculating the algorithm on the GPGPU.

During my research of these algorithms I quickly realised that the only algorithm that could be used in this manner would be Cellular Automata as it allows you to calculate each cell in any order which is exactly what I required for this implementation. However using cellular automata alone would not find a path within a ‘Fat Maze’ (a maze which has more than one route), so I also decided that I would complete the path using A* and see whether performing cellular automata before the A* algorithm would improve the performance of complicated mazes.

# Cellular Automata

The Cellular Automata algorithm I used throughout my dissertation was [based](http://realtimecollisiondetection.net/blog/?p=57) on an article by Christer Ericson, it uses simple instructions sent to every cell in a maze in a number of iterations, to close off any dead ends that may exist, for more information about how Cellular Automata works please check my dissertation below. My thoughts as to why this could improve A* was that the A* algorithm often finds itself searching down dead ends or areas that eventually do not lead to the particular area you wish to move to and with the removal of most of these areas it would mean that the A* algorithm would find the path without looking at these dead ends. The images below show a maze in where only A* was used to calculate a path as well as a maze where Cellular Automata is used to remove the dead ends, you can see that far less dead ends are being checked.

![Showing the differences between A* alone and A* with Cellular Automata](images/diss_cellular_automata_a.png)

# Conclusion

During my dissertation I had been looking for results that would show in real terms that the method of using Cellular Automata to pre-process a maze before A* would have an overall benefit to the performance on maze, however I found the opposite within my results. It seemed that due to the slower speed of Cellular Automata processing coupled with the bottleneck of transferring data from system memory to the host memory on the GPGPU, meant that the overall process was slower than the A* algorithm alone, and that the speedup given to the A* algorithm after pre-processing the maze had only a minimal effect on the speed of the A* algorithm. Below you can see one of the results given by the dissertation, which clearly shows that my first thoughts on this project were incorrect.

![Average processing time for path finding techniques tested](images/diss_avg_calc_speeds.png)