# Chapter 2 : Compression

## 2.1 What is compression
Let us continue with our example: the road trip. There are two bystanders to which you can ask the route- one is a prodigy and tells each of the multiple routes to your destination, and another knows the best/shortest route to the destination. Obviously, any normal person will get confused on hearing the entire lists of route he can take, and will probably end up getting lost again. The other guy, on the other hand, suggests the best route, and hence, is more often the preferred choice, as he too has decided the optimum route after considering all the routes.

Irrespective of who gives us the route, we generally choose the best route of the mentioned ones, and thus, the two bystanders effectively convey the same information. Thus, bystander 2 has conveyed the same information as bystander 1, while talking less (or consuming less of our memory space, lets say). In short, route by bystander 2 is the *compressed* form of whatever bystander 1 said.

Consider another example: the reader probably knows, and prefers the use of π as 3.14, and not as 3.14159265358... , and speed of light as 3 x 10<sup>8</sup> m/s, and not 299792458 m/s. Formally, **compression refers to conversion (of data, a data file, or a communications signal) in order to reduce the space occupied or bandwidth required**. Compression is normally used to reduce the size of data/file without removing information.

With the advent of technology, more and more exchange and storage of data came to be, and as storage of this data is somewhat costly, the search for better and better ways of compression began to be carried out. This search branched out into different fields, such as data compression, signal compresson etc, and hence, compression is now a very vast, disjoint branch.

Recall from chapter 1 that there are different algorithm paradigms (commonly used approach in designing algorithms). The goal of compression is one: to *compress* the data/file, ie, to reduce the space taken by it, and since it is not a way of designing an algorithm, compression is not a paradigm. Rather, it is a different field in computer science. The different algorithms used for different kinds of compression use different algorithm paradigms like the ones mentioned in chapter 1.

## 2.2 Why compression
As mentioned above, the need of compression is quite obvious
    - To save *space* while storing data
    - To save *time* while transmitting it
    - Most files have a lot of redundant content, and hence, if these can be "comdensed" into a simple representation (lets call it a black box) then it is much easier to store as well as understand.

For example, we know that a movie (1080p) takes around 2gb (not considering bluray). But an uncomressed 1080p 8-bit video requires about 10gb *per minute*. This means that an average movie will take up 1200gb, ie, more than 1TB space. Similarly, the pictures we save in our galleries (jpg or png) are compressed, and not actually the RAW files, which takes up a lot more space. In the modern era, where facetime, skype and other video conferencing is now more preferred than a phonecall, compression is extremely important.

## 2.3 Some terminologies used in compression
### 2.3.1 Self information and entropy
This was a term coined by Shannon. Suppose we have an event A whose probability of occurence is P(A). Then, _Self information_ of A is given by <br>
> *i(A) = - log<sub>b</sub>P(A)*

where b can be 2 (unit is bits), e (unit is nats) or 10 (unit is hartleys). In general, we take b as 2.
As we know that -log(x) is larger for an x that is closer to 0 in the interval (0,1). Thus, **if the probability of an event is low, the amount of self information associated with that event is high** , and vice versa.

Now, consider we have a set of independent events *A<sub>i</sub>*, and sample space S is the union of all these events. The average self information associated with some random experiment is then given by:
> H = Σ P(A<sub>i</sub>) x i(A<sub>i</sub>) = -Σ P(A<sub>i</sub>) x log<sub>b</sub>P(A<sub>i</sub>)

This quantity is called the **entropy** of the experiment. In other words, entropy is a **measure of the average number of binary symbols needed to code the output** of the experiment.

> For example, consider the binary sequences 000000 and 010101.
> 1. in 000000 , P(0)=1, and thus, entropy H = -1logl = 0. Entropy is 0, as we dont need any information to represent which symbol is next in the series, as the series contains only 0s.
> 2. in 010101 , p(0)=P(1)=0.5, and thus, H= -(0.5log0.5 + 0.5log0.5) [taking base as 2,] = -(-1) = 1. Since base is 2, we get that we need an average of 1 bit to represent each character in the string (which is true)

### 2.3.2 Models
Designing the data into a good model can be helpful in constructing efficient compression techniques for the data. Some commonly used models are:
- **Physical model**: Used when we know the _physics_ of the data generation process. An example is speech compression, as knowledge about the physics of speech production can be used to construct a mathematical model for the sampled speech process, and sampled speech can be encoded using this model. (In general, however, the physics of data generation is too complicated to understand, and hence we cannot make such models).

- **Probability model**: Keep the independence assumption and assign probabilities to each letter of the alphabet. Using the probability model, we can construct some very efficient codes to represent the letters in the alphabet . However, these codes are only efficient if our mathematical assumptions are in line with reality.

- **Markov model**: For models used in lossless compression, we use a specific type of Markov process called a *discrete time Markov chain*:
    - Let {x<sub>i</sub>} be the sequence of observations, then the sequence follows k-th order Markov model if<br> P(x<sub>n</sub>|x<sub>n-1</sub>,....,x<sub>n-k</sub>) = P(x<sub>n</sub>|x<sub>n-1</sub>,....,x<sub>n-k</sub>,...)
    - This can be stated as: knowledge of the past k symbols is equivalent to the knowledge of the entire past history of the process
    - Most commonly used Markov model is first order Markov model, ie,  P(x<sub>n</sub>|x<sub>n-1</sub>) = P(x<sub>n</sub>|x<sub>n-1</sub>,x<sub>n-2</sub>,x<sub>n-3</sub>,...)

Markov models are particularly useful in text compression where the probability of the succeeding letter is greatly influenced by the preceding letters.

- Oftentimes, it is not easy to use a single model to describe the source. In such cases, we can define a *composite source*, which can be seen as a combination of several sources, with only one source being active at any given time

## 2.3.3 Coding
Coding is the process of assignment of binary sequences to letters of an alphabet. The set of binary sequences is called a code, and each individual members of the set are called codewords. A trivial example of coding can be taken from the ASCII code of the English alphabet: A is 1000001, and that of a is 1000011. ASCII also contains codes for certain characters like "," (comma, ascii is 0011010). Each ASCII codeword has a fixed, same number of bits, and thus, ASCII is an example of **fixed length code**. 

More often than not, we use lesser number of bits to represent those letters that occur more frequently than others (as reducing the number of bits required, and hence the memory required is the cruz of compression in the first place). The average number of bits per symbol is called the **rate** of the code. A typical example that uses this idea is the Morse code (you probably already know what it is from spy and action movies :) ) in which the codewords that occur more frequently are shorter (E is `·`, while Z is `—··`)

To be useful, a code should not only reduce the length of encoding, but also have the ability to transfer information in an unambiguous manner. Consider the following 3 encodings of the same letters:

![encodingex](https://github.com/Abhijith1202/aad-project/blob/main/images/encodingex.png?raw=true)