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
<table>
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
    