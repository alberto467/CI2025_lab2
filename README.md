# CI2025_lab2

# Genetic Algorithm for the Traveling Salesman Problem

## Operators overview

### Mutation operators

Multiple mutation operators have been explored:

- **Swap**: This operator selects two random positions in the tour and swaps them. It is the simplest.

- **Reverse Subsequence**: This operator selects a subsequence of the tour and reverses it, effectively inverting the edges of that subsequence.

- **Shift Subsequence**: This operator selects a subsequence and shifts it to a different position in the tour, effectively replacing some edges.

- **Random Mutation**: This operator randomly chooses between the above mutation strategies for each mutation operation, introducing more diversity. 

### Crossover operators

- **Join Solutions**: This operator creates a child solution by taking the first half of one parent and filling in the remaining cities from the second parent in the order they appear, ensuring no duplicates.

- **Edge Assembly Crossover (EAX)**: This operator is targeted to the traveling salesman problem, it analyzes the edges of both parents to create a child solution that combine edges from both parents, leading to a recombination of their characteristics.

## Comments

I first experimented with various mutation  operators and focused more on the genetic algorithm itself, considering various selection mechanisms, how elitist the algorithm should be, and in general different ways to manage the population cycle.
I found mutation operators in themselves to be able to make good progress on their own.

I then tried to implement some crossover operators, at first I tried to write some simple operators from scratch, and after some iterations I reached the generically named “join_solutions” crossover operator.
I managed to reach further gains by including my rather simple crossover operator, which lead me to believe that implementing a more advanced and targeted crossover operator would be the key to unlock even more gains.

After some research I discovered the EAX edge crossover operator, which is tailored for the traveling salesman problem, and has been discussed in literature for a while.
I unsuccessfully searched for a somewhat working or easily adaptable python implementation, then started implementing my version of EAX from a couple of papers.
After much debugging I reached a working state, but unfortunately the performances of this crossover operator were problematic, as I measured it to be hundreds of times slower then my simple crossover operator, despite considerable optimizations efforts with numba.
It has to be noted that the EAX operator does see to be more powerful when comparing by iterations, but in testing, simply running the fast crossover for many more iterations was improving faster. I also cannot exclude the presence of bugs in my implantation of EAX, it should also be said that there exists many versions and variants of EAX and there may very well be an implementation of it that would help overall performance.

In the end i kept with using my simple crossover operator, combined with the random mutation operator, which together seem to provide a good balance of exploration and exploitation. To ensure that this balance is kept, I implemented different probabilistic selection mechanisms, such as exponential and sigmoid based selections, which help to maintain diversity in the population while still favoring better solutions. I also used a temperature parameter with a schedule to control the "elitism" of the selection mechanisms, although the benefits seem to be negligible.
