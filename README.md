This is a challenge I was randomly invited by Google while googling things for work, I found this problem quite interesting, so I decided to post my solution and explanation here.

Gearing Up for Destruction
==========================

As Commander Lambda's personal assistant, you've been assigned the task of configuring the LAMBCHOP doomsday device's axial orientation gears. It should be pretty simple -- just add gears to create the appropriate rotation ratio. But the problem is, due to the layout of the LAMBCHOP and the complicated system of beams and pipes supporting it, the pegs that will support the gears are fixed in place.

The LAMBCHOP's engineers have given you lists identifying the placement of groups of pegs along various support beams. You need to place a gear on each peg (otherwise the gears will collide with unoccupied pegs). The engineers have plenty of gears in all different sizes stocked up, so you can choose gears of any size, from a radius of 1 on up. Your goal is to build a system where the last gear rotates at twice the rate (in revolutions per minute, or rpm) of the first gear, no matter the direction. Each gear (except the last) touches and turns the gear on the next peg to the right.

Given a list of distinct positive integers named pegs representing the location of each peg along the support beam, write a function solution(pegs) which, if there is a solution, returns a list of two positive integers a and b representing the numerator and denominator of the first gear's radius in its simplest form in order to achieve the goal above, such that radius = a/b. The ratio a/b should be greater than or equal to 1. Not all support configurations will necessarily be capable of creating the proper rotation ratio, so if the task is impossible, the function solution(pegs) should return the list [-1, -1].

For example, if the pegs are placed at [4, 30, 50], then the first gear could have a radius of 12, the second gear could have a radius of 14, and the last one a radius of 6. Thus, the last gear would rotate twice as fast as the first one. In this case, pegs would be [4, 30, 50] and solution(pegs) should return [12, 1].

The list pegs will be given sorted in ascending order and will contain at least 2 and no more than 20 distinct positive integers, all between 1 and 10000 inclusive.

Languages
=========

To provide a Java solution, edit Solution.java
To provide a Python solution, edit solution.py

Test cases
==========
Your code should pass the following test cases.
Note that it may also be run against hidden test cases not shown here.

-- Java cases --  
Input:  
Solution.solution({4, 17, 50})  
Output:  
&ensp;&ensp;-1,-1  
  
Input:  
Solution.solution({4, 30, 50})  
Output:  
&ensp;&ensp;12,1  
  
-- Python cases --  
Input:  
solution.solution([4, 30, 50])  
Output:  
&ensp;&ensp;12,1  
  
Input:  
solution.solution([4, 17, 50])  
Output:  
&ensp;&ensp;-1,-1  

# Solution
## Physics and geometry

Before we start solving the problem, let's discuss some physics.

*For simplicity, we can imagine having perfectly circular wheels instead of gears; this physically and geometrically does not create any problems because the transmission ratio does not change.*

First of all, let's define angular velocity as $\large \omega=\frac{2\pi}{T}\left[\frac{rad}{s}\right]$, where $\large T$ is the time it takes for a point on the circumference to make one complete revolution, while the velocity is defined as $\large v=\frac{2\pi ~r}{T}\left[\frac{m}{s}\right]$, where $\large T$ is the time it takes for a point on the circumference to travel the space $\large 2\pi ~r$ and $\large r$ is the radius of the circumference, basically the velocity tell us, how fast is the point moving ($\large m/s$) on the circumference.

The relation between velocity and angular velocity is $\large v=\omega ~r$.

Our goal is to make the last gear rotates at twice the rate (in revolutions per minute, or rpm) of the first gear, which translates into the fact that the angular velocity of the last gear ($\large\omega_L$) must be double of the first gear ($\large\omega_1$), that is, $\large\omega_L=2\omega_1$.

We know that the speed ($\large v$) of adjacent gears is equal (because they touch each other); in other words, the amount of space (meters) that a tooth of the first gear travels must equal that of a tooth of the second gear, the same goes for the second and third gear, for the third and fourth gear and so on.

So we can say that all the teeth of all the gears move at the same speed:

$$
\large
v_1=v_2=v_3=v_n=v_L
$$

Consequently, the speed of the first gear is equal to that of the last gear:

$$
\large
v_1=v_L
$$

Let's solve the equation above:

$$
\large
\begin{array}{ll}
\omega_1 ~r_1=\omega_L ~r_L\\
r_1=\frac{\omega_L}{\omega_1} ~r_L\\
r_1=\frac{2\omega_1}{\omega_1} ~r_L\\
r_1=2r_L\\
\end{array}
$$

From the solution above, it can be seen that for the last gear to rotate at double the speed of the first gear, the radius of the first gear must be double that of the last gear, and we don't have to worry about the radius and angular velocity of the intermediate gears.

## Solution with an example

We will use an example to then find the equations and consequently the algorithm.

In the following, we will use $\large P_1$ to indicate the position of the first peg, $\large P_2$ for the second peg and so on, while with $\large R_n$ we will indicate the radius of the relative gear.

Below is a graphic example of a system with 7 gears

![7 Gear Example](https://github.com/parmi93/gearing-up-for-destruction/blob/main/img/example_7_gear_dark_mode.svg#gh-dark-mode-only)![7 Gear Example](https://github.com/parmi93/gearing-up-for-destruction/blob/main/img/example_7_gear_light_mode.svg#gh-light-mode-only)

Let's set up a system of equations to solve the example above:

$$
\large
\begin{cases}
R_1=2R_7\\
P_2-P_1=R_1+R_2\\
P_3-P_2=R_2+R_3\\
P_4-P_3=R_3+R_4\\
P_5-P_4=R_4+R_5\\
P_6-P_5=R_5+R_6\\
P_7-P_6=R_6+R_7\\
\end{cases}
$$

Move the radii (in ascending order) to the left of the equations:

$$
\large
\begin{cases}
R_1=2R_7\\
R_2=P_2-P_1-R_1\\
R_3=P_3-P_2-R_2\\
R_4=P_4-P_3-R_3\\
R_5=P_5-P_4-R_4\\
R_6=P_6-P_5-R_5\\
R_7=P_7-P_6-R_6
\end{cases}
$$

Substitute the radii to the right of the equations (starting from the third equation):

$$
\large
\begin{cases}
R_1=2R_7\\
R_2=P_2-P_1-R_1\\
R_3=P_3-P_2-P_2+P_1+R_1\\
R_4=P_4-P_3-P_3+P_2+P_2-P_1-R_1\\
R_5=P_5-P_4-P_4+P_3+P_3-P_2-P_2+P_1+R_1\\
R_6=P_6-P_5-P_5+P_4+P_4-P_3-P_3+P_2+P_2-P_1-R_1\\
R_7=P_7-P_6-P_6+P_5+P_5-P_4-P_4+P_3+P_3-P_2-P_2+P_1+R_1
\end{cases}
$$

With the following step, we rearrange the terms $\large P_n$ in ascending order for each equation and we change the sign of each $\large P_n$ belonging to the odd number gears.

$$
\large
\begin{cases}
R_1=2R_7\\
R_2=+(\underset{S_2}{\underbrace{-P_1+P_2}})-R_1\\
R_3=-(\underset{S_3}{\underbrace{-P_1+P_2+P_2-P_3}})+R_1\\
R_4=+(\underset{S_4}{\underbrace{-P_1+P_2+P_2-P_3-P_3+P_4}})-R_1\\
R_5=-(\underset{S_5}{\underbrace{-P_1+P_2+P_2-P_3-P_3+P_4+P_4-P_5}})+R_1\\
R_6=+(\underset{S_6}{\underbrace{-P_1+P_2+P_2-P_3-P_3+P_4+P_4-P_5-P_5+P_6}})-R_1\\
R_7=-(\underset{S_7}{\underbrace{-P_1+P_2+P_2-P_3-P_3+P_4+P_4-P_5-P_5+P_6+P_6-P_7}})+R_1
\end{cases}
$$

Above we will notice that there is a sequence ($\large S_N$) that gets bigger as the gear number increases; basically we can say that $\large S_N$ is equal to the previous sequence ($\large S_{N-1}$) by adding two new elements ($\large P_N$ and $\large P_{N-1}$), which means that if we want to compute $\large S_N$ we have to compute all the previous sequences first, so to calculate $\large S_7$ we first need to calculate $\large S_2$, $\large S_3$, $\large S_4$, $\large S_5$ and $\large S_6$.

We can generalize the sequence with the following formula:

$$
\large
\begin{array}{ll}
S_1=0\\
S_N=S_{N-1}+(-1)^{N}(P_N-P_{N-1})
\end{array}
$$

We can also write this sequence ($\large S_N$) without using the recursion, using a summation:

$$
\large
S_N=\sum_{i=2}^{N}(-1)^i(P_i-P_{i-1})
$$

In any case, let's continue by solving our example; with the following steps we calculate the radius of the first gear:

$$
\large
\begin{array}{ll}
R_1=2R_7\\
R_1=2(-S_7+R_1)\\
R_1=-2S_7+2R_1\\
R_1-2R_1=-2S_7\\
R_1(1-2)=-2S_7\\
-R_1=-2S_7\\
R_1=2S_7\\
\end{array}
$$

We can now note that to calculate $\large R_1$ we just need to calculate $\large S_7$ and multiply it by 2.

Let's expand the equation to notice all the  $\large S_N$ sequences in it:

$$
\large
R_1=2(\underset{S_7}{\underbrace{\underset{S_6}{\underbrace{\underset{S_5}{\underbrace{\underset{S_4}{\underbrace{\underset{S_3}{\underbrace{\underset{S_2}{\underbrace{-P_1+P_2}}+P_2-P_3}}-P_3+P_4}}+P_4-P_5}}-P_5+P_6}}+P_6-P_7}})
$$

Now we can write the formula to calculate the radius of the first gear in a 7 gear system:

$$
\large
R_1=2S_7=2\sum_{i=2}^{7}(-1)^i(P_i-P_{i-1})
$$

If we repeat the example with 6 gears we will find the following equation:

$$
\large
R_1=\frac{2}{3}(\underset{S_6}{\underbrace{\underset{S_5}{\underbrace{\underset{S_4}{\underbrace{\underset{S_3}{\underbrace{\underset{S_2}{\underbrace{-P_1+P_2}}+P_2-P_3}}-P_3+P_4}}+P_4-P_5}}-P_5+P_6}})
$$

So we can derive the following formula to solve a system with 6 gears:

$$
\large
R_1=\frac{2}{3}S_6=\frac{2}{3}\sum_{i=2}^{6}(-1)^i(P_i-P_{i-1})
$$

We can see that the above formula is very similar to the one with 7 gears; the only difference is that  $\large S_6$ is multiplied by $\large \frac{2}{3}$ instead of $\large 2$.

If we keep repeating this exercise several times with different numbers of gears, we will notice at some point that $\large S_N$ is multiplied by $\large 2$ when the number of gears is odd and by $\large \frac{2}{3}$ when the number of gears is even, so taking into account this fact we can now write the final formula, which works in all cases whether there are an odd or even number of gears:

$$
\large
R_1=\frac{2}{3}((-1)^{N+1}+2)S_N
$$

Same formula but using summation instead of $\large S_N$:

$$
\large
R_1=\frac{2}{3}((-1)^{N+1}+2)\sum_{i=2}^{N}(-1)^i(P_i-P_{i-1})
$$

To implement the above formula into an algorithm it's quite simple, we just need to iterate through all the pegs calculating their sum and changing their signs depending on whether the index is even or odd.
```java
/*r1 will contain the numerator and denominator of the radius of the first gear, also 
 * there are always at least two pegs, so we can already compute S_1 and save it in r1[0]. */
int[] r1 = {pegs[1] - pegs[0], 1};

//This cycle is the implementation of the summation according to the formula
for(int i = 2; i < pegs.length; i++) {
    //Note that here 'i' is the index of the array "pegs", while in the formula it's the number of the peg (starting from 1)
    if(i % 2 == 0) {
        r1[0] += -pegs[i] + pegs[i-1]; //Note that r1[0] will contain odd S_N whenever 'i' is even
    } else {
        r1[0] += pegs[i] - pegs[i-1]; //Note that r1[0] will contain even S_N whenever 'i' is odd
    }
}
//We multiply the result of the summation by 2 or 2/3, depending on whether the number of pegs is odd or even
r1[0] *= 2;
if(pegs.length % 2 == 0) {
    //If possible we simplify the fraction (as required by the challenge)
    if(r1[0] % 3 == 0) {
        r1[0] /= 3;
    } else {
        r1[1] = 3;
    }
}
```

The problem with the above formula/algorithm is that it doesn't take into account that the radius of the gears must be $\large \geq1$; in fact, the above formula can result in radii smaller than 1, in some cases even negative radii.

So let's set up the following system of inequalities to impose all $\large R_N\geq1$:

$$
\large
\begin{cases}
(R_2=+S_2-R_1)\geq 1\\
(R_3=-S_3+R_1)\geq 1\\
(R_4=+S_4-R_1)\geq 1\\
(R_5=-S_5+R_1)\geq 1\\
(R_6=+S_6-R_1)\geq 1\\
(R_7=-S_7+R_1)\geq 1\\
\end{cases}
$$

Please note that above we don't need to set $\large R_1\geq1$ because we already set $\large R_7\geq1$, which is half of $\large R_1$, this means that if $\large R_7\geq1$, then automatically $\large R_1\geq1$.

In the following step, we rearrange the terms by moving $\large R_1$ to the left of the inequalities and remove the negative sign of all odd $\large S_N$:

$$
\large
\begin{cases}
R_1\leq S_2-1\\
R_1\geq S_3+1\\
R_1\leq S_4-1\\
R_1\geq S_5+1\\
R_1\leq S_6-1\\
R_1\geq S_7+1\\
\end{cases}
$$

In the system above we can see something interesting; basically, to check that every radius is $\large \geq1$, we have to check that:

$$
\large 
\begin{cases}
R_1\leq S_{N}-1~~~~~\text{for every even N}\\
R_1\geq S_{N}+1~~~~~\text{for every odd N}
\end{cases}
$$

### Check the minimum size of all radii

This leads us to another conclusion, that is if we know the smallest $\large S_{Neven}$ and the biggest $\large S_{Nodd}$ we would need to satisfy only two conditions:

$$
\large
\begin{cases}
R_1\leq min(S_{Neven})-1\\
R_1\geq max(S_{Nodd})+1
\end{cases}
$$

So now the problem reduces to finding $\large max(S_{Nodd})$ and $\large min(S_{Neven})$, which is quite simple; in fact, we can see that in the above algorithm at each iteration, we are already calculating the value of the related $\large S_N$, which is saved in ```r1[0]```, which means we don't need to do a second iteration to find the $\large max(S_{Nodd})$ and $\large min(S_{Neven})$, we just need to add a few lines of code to the iteration we already have.
```java
/*r1 will contain the numerator and denominator of the radius of the first gear, also 
 * there are always at least two pegs, so we can already compute S_2 and save it in r1[0]. */
int[] r1 = {pegs[1] - pegs[0], 1};
int min_s_even = r1[0]; //Initialize with S_2, which is the smallest Sn_even known so far
int max_s_odd = 0;      //Initialize with S_1 (defined as 0), which is the biggest Sn_odd known so far

//This cycle is the implementation of the summation according to the formula
for(int i = 2; i < pegs.length; i++) {
    //Note that here 'i' is the index of the array "pegs", while in the formula it's the number of the peg (starting from 1)
    if(i % 2 == 0) {
        r1[0] += -pegs[i] + pegs[i-1]; //Note that r1[0] will contain odd S_N whenever 'i' is even
        if(r1[0] > max_s_odd) { //Find max(Sn_odd)
            max_s_odd = r1[0];
        }
    } else {
        r1[0] += pegs[i] - pegs[i-1]; //Note that r1[0] will contain even S_N whenever 'i' is odd
        if(r1[0] < min_s_even) { //Find min(Sn_even)
            min_s_even = r1[0];
        }
    }
}
//We multiply the result of the summation by 2 or 2/3, depending on whether the number of pegs is odd or even
r1[0] *= 2;
if(pegs.length % 2 == 0) {
    //If possible we simplify the fraction (as required by the challenge)
    if(r1[0] % 3 == 0) {
        r1[0] /= 3;
    } else {
        r1[1] = 3;
    }
}
//Verify that no radius is less than 1
if(r1[0] < (max_s_odd + 1) * r1[1] || r1[0] > (min_s_even - 1) * r1[1]) {
    r1[0] = r1[1] = -1;
}
```
<br/>

**If you have suggestions, improvements or want some clarification, feel free to open a discussion in the [Issues](https://github.com/parmi93/gearing-up-for-destruction/issues) section!**
