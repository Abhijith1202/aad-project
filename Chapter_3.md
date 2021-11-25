# Chapter 3: A few Compression Algorithms
In this chapter, we will discuss about a few compression algorithms such as the Huffman coding, LZ algorithm, image compression etc, and potential drawbacks and improvements in these algorithms.

## 3.1 Huffman Coding
Huffman coding is a **lossless data compression algorithm**. It is one of the more popular data compression algorithms taught. The goal is to most economically write a given long string (over some alphabet) in binary, ie, _encoding_ it. Consider the English alohabet for instance. We know that some letters like 'e' and 'a' are used more frequently, whereas letters like 'z' and 'x' are comparatively rarer. The main idea of Huffman coding is to **assign lesser bit length strings to charactoers/symbols that appear more frequently in the language. The codes assigned to these are *prefix codes*, to avoid ambiguity (as explained in section 2.3.3). Notice that this encoding may not be unique in the case that two of the characters have the same frequency. This algorithm follows the *greedy* paradigm.

The problem we have can be modelled as: Given frequencies _f<sub>1</sub>,f<sub>2</sub>,...f<sub>n</sub>_ of n characters. we need to make a *tree* such that the *leaves denote the characters*, and minimize the overall length of the encoding. Let us introduce a new term: the **cost** of a tree as the sum of (frequency of a character) x (depth of the character) ie, 

![huffmancost](images/huffmancost.png)

Now, as the algorithm belongs to the greedy paradigm, it has to satisfy *greedy choice property* and the *optimum substructure property*. These are attained as:
- For the greedy choice, note that the symbols with the lowest frequencies must be at the deepest leaves of the tree,ie , children of the lowest node (sublings) , as otherwise, swapping these two with the lowest occuring characters would improve the tree cost, and hence the encoding.

- For optimum substructure property:
    - define the frequency  of an internal node to be the sum of frequencies of its descendant leaves.
    - now, the cost of tree is the sum of frequencies of all leaves and internal nodes. Why?
        - Suppose an internal node has descendant leaves of size f1, f2.
        - Now if the node had n as its bitlength, the leaves are obtained by adding another bit (0 or 1)
        - thus, to account for the entire tree, adding f1,f2 will give the number of *this* extra bit, and defining the frequency of the internal node to be f1+f2 ,recursively climbing the tree will ensure that we do indeed get the entire cost of the tree.

    - A tree with f1,f2 as the lowest sibling leave frequencies has cost (f1+f2) + the cost of tree having leaves of frequency {(f1+f2),f3,f4,...fn} (Effectively, we reduce the tree size and remodel it into a smaller preblem.)
        
        ![huffman tree](images/huffmantree.png)

- Thus, both greedy choice property and optimum substructure property is satisfied

The algorithm can be summed up as:
    ```procedure Huffman(f)
    Input: an array of frequencies f[1,2...n]
    output: An encoding tree with n leaves

    get a priority queue (like a heap) H of integers, ordered by f (their frequencies).
    for i = 1 to n
        insert (H,i)
    for k = n+1 to 2n-1
        i = deletemin(H)
        j = deletemin(H) (i and j gives the 2 least frequent nodes)
        create a node numbered k with children as i and j
        f[k]=f[i]+f[j]
        insert(H,k)
    ```

For example, consider a situation as shown below:
<table >
    <tr>
        <th>Character</th>
        <th>Frequency</th>
    </tr>
    <tr>
        <td>a</td>
        <td>5</td>
    </tr>
    <tr>
        <td>b</td>
        <td>9</td>
    </tr>
    <tr>
        <td>c</td>
        <td>12</td>
    </tr>
    <tr>
        <td>d</td>
        <td>13</td>
    </tr>
    <tr>
        <td>e</td>
        <td>16</td>
    </tr>
    <tr>
        <td>f</td>
        <td>45</td>
    </tr>
</table>

1. Build a priority queue (heap) with 6 nodes for the 6 characters.
2. Ge the 2 minimum freq nodes, ie, a and b and add a new internal node with frequency f = 5+9 = 14
    ![s2](images/huffmanex/s2.jpeg)

3. Now, the heap contains 6-2+1=5 nodes, as:
<table >
    <tr>
        <th>Character</th>
        <th>Frequency</th>
    </tr>
    <tr>
        <td>c</td>
        <td>12</td>
    </tr>
    <tr>
        <td>d</td>
        <td>13</td>
    </tr>
    <tr>
        <td>IN</td>
        <td>14</td>
    </tr>
    <tr>
        <td>e</td>
        <td>16</td>
    </tr>
    <tr>
        <td>f</td>
        <td>45</td>
    </tr>
</table>
Where IN denotes the internal node<br>
Repeat the steps till each of the characters are popped

![s3](images/huffmanex/s3.jpg)

<table >
    <tr>
        <th>Character</th>
        <th>Frequency</th>
    </tr>
    <tr>
        <td>IN</td>
        <td>14</td>
    </tr>
    <tr>
        <td>e</td>
        <td>16</td>
    </tr>
    <tr>
        <td>IN</td>
        <td>25</td>
    </tr>
    <tr>
        <td>f</td>
        <td>45</td>
    </tr>
</table>

![s4](images/huffmanex/s4.jpg)

<table >
    <tr>
        <th>Character</th>
        <th>Frequency</th>
    </tr>
    <tr>
        <td>IN</td>
        <td>25</td>
    </tr>
    <tr>
        <td>IN</td>
        <td>30</td>
    </tr>
    <tr>
        <td>f</td>
        <td>45</td>
    </tr>
</table>

![s5](images/huffmanex/s5.jpg)

<table >
    <tr>
        <th>Character</th>
        <th>Frequency</th>
    </tr>
        <td>f</td>
        <td>45</td>
    </tr>
    <tr>
        <td>IN</td>
        <td>55</td>
    </tr>
</table>

![s6](images/huffmanex/s6.jpg)

Now, the heap contains only 1 element, and we are done!
While traversing down the tree formed:
- while moving left, write a 0
- while moving right, write a 1 

![final](images/huffmanex/final.jpg)

The codes are:
<table >
    <tr>
        <th>Character</th>
        <th>Code-word</th>
    </tr>
    <tr>
        <td>f</td>
        <td>0</td>
    </tr>
    <tr>
        <td>c</td>
        <td>100</td>
    </tr>
    <tr>
        <td>d</td>
        <td>101</td>
    </tr>
    <tr>
        <td>a</td>
        <td>1100</td>
    </tr>
    <tr>
        <td>b</td>
        <td>1101</td>
    </tr>
    <tr>
        <td>e</td>
        <td>111</td>
    </tr>
</table>

- Time complexity: deletemin() operation is used 2(n-1) times, and it takes O(logn) complexity (heap). Thus, the overall time complexity of this algorithm is O(nlogn)

The Huffman coding can be used for transmitting text and compression of text documents and fax messages. They are also used by conventional compression formats like PKZIP, GZIP, etc. It is useful in cases where there is a series of frequently occurring characters. The Morse code could be a far-fetched idea from the Huffman coding, as the symbold which occur more frequently are given shorter codes in the language.