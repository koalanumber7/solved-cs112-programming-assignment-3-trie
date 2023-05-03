Download Link: https://assignmentchef.com/product/solved-cs112-programming-assignment-3-trie
<br>
<strong>In this assignment you will implement a tree structure called Trie (pronounced “Tree” or “Try”!).</strong>

<h1>Summary</h1>

You will write an application to build a tree structure called Trie for a dictionary of English words, and use the Trie to generate completion lists for string searches.

<h1>Trie Structure</h1>

A Trie is a general tree, in that each node can have <em>any</em> number of children. It is used to store a dictionary (list) of words that can be searched on, in a manner that allows for ef cient generation of completion lists.

The word list is originally stored in an array, and the trie is built off of this array. Here are some examples of word lists and the tries built to store the words, followed by an explanation of the trie structure and its relationship to its source word list.

<strong>Trie 1                                           Trie 2                                                                                                          Trie 3</strong>

Child (0,0,0) of root stores common prefix “d” of its children “data” (left child) and “door” (right child), in triplet 0 (index of first word “data” in list), 0 (starting position of prefix “d”), and 0 (ending position of prefix “d”). Internal nodes represent prefixes, leaf nodes represent complete words. The left leaf node stores triplet 0 (first word in list), 1 (first index past the common prefix “d”, and 3 (last index in word). The right leaf node is stored similarly.




<strong>Trie 4                                                                               Trie 5                                                Trie 6</strong>




2 children, one for each word.

(Common suffixes are irrelevant)

A node stores the longest common prefix among its children. Since “do” is the longest common prefix of all the words in the list, it is stored in the child of the root node as the triplet (0,0,1). The left branch points to a subtree that stores “door” and “doom” since they share a common prefix “doo”, while the right branch terminates in the leaf node for “dorm” stored as the triplet 1 (index of word “dorm”), 2 (starting position of substring “rm” following prefix “do”), and 3 (ending position of substring “rm”)




<strong>Trie 7</strong>

There is no common prefix among all the words. But “door” and “doom” have a common prefix “doo”, while “pore” and “port” have a common prefix “por”.







<h1>Special Notes</h1>

Every leaf node represents a complete word, and every complete word is represented by some leaf node. (In other words, internal nodes do not represent complete words, only proper pre xes.)

No node, except for the root, can have a single child. In other words, every internal node has at least 2 children. Why? Because an internal node is a <em>common pre x</em> of several words. Consider these trees, in each of which an internal node has a single child (incorrect), and the equivalent correct tree:




<strong>One-word trie                                               Two-word trie</strong>

<strong>Incorrect/Correct                                          Incorrect/Correct</strong>

A trie does NOT accept two words where one entire word is a pre x of the other, such as “free” and “freedom”.

(You will not come across this situation in any of the test cases for your implementation.)

The process to build the tree (described in the <strong>Building a Trie</strong> section below), will create a single child of the root for the longest common pre x “free”, and this node will have a single child, a leaf node for the word “freedom”. But this is an incorrect tree because it will (a) violate the constraint that no node aside from the root can have a single child, and (b) violate the requirement that every complete word be a leaf node (the complete word “free” is not a leaf node).







On the other hand, a tree with two leaf node children off the root node, one for the word “free” and the other for the word “freedom” will be incorrect because the longest common pre x MUST be a separate node. (This is the basis of completion choices when the user starts typing a word.)

<h1>Data Structure</h1>

Since the nodes in a trie have varying numbers of children, the structure is built using linked lists in which each node has three elds:

<strong>substring</strong> (which is a triplet of indexes)

<strong>rst child</strong>, and <strong>sibling</strong>, which is a pointer to the next sibling.

Here’s a trie and the corresponding data structure:




<h1>Building a Trie</h1>

A trie is built for a given list of words that is stored in array. The word list is input to the trie building algorithm. The trie starts out empty, inserting one word at a time.

<h2>Example 1</h2>

The following sequence shows the building of the above trie, one word at a time, with the complete data structure shown after each word is inserted.

<strong>Input and Initial</strong>

<strong>After Inserting “door”                     After Inserting “dorm”                                     After Inserting “doom”</strong>

<strong>Empty Tree</strong>

<h2>Example 2</h2>

This shows the sequence of inserts in building Trie 7 shown earlier.




<strong>Empty            After inserting “cat”        After inserting “muscle”            After inserting “pottery”                               After inserting “possible”</strong>

<strong>After inserting “possum”                                            After inserting “musk”</strong>

<h1>Pre    x Search</h1>

Once the trie is set up for a list of words, you can compute word completions ef ciently.

For instance, in the trie of Example 2 above (cat, muscle, …), suppose you wanted to nd all words that started with “po” (pre x). The search would start at the root, and touch the nodes [0,0,2],(1,0,2),(2,0,1),(2,2,2),(3,2,3),[2,3,6],[6,3,5],[3,4,7],[4,4,5] . The nodes marked in red are the ones that hold words that begin with the given pre x.

Note that NOT ALL nodes in the tree are examined. In particular, after examining (1,0,2), the entire subtree rooted at that node is skipped. This makes the search ef cient.

(Searching all nodes in the tree would obviously be very inef cient, you might as well have searched the word array in that case, why bother building a trie!)

<h1>Implementation</h1>

Download the attached trie_project.zip le to your computer. DO NOT unzip it. Instead, follow the instructions on the Eclipse page under the section “Importing a Zipped Project into Eclipse” to get the entire project into your Eclipse workspace.

You will see a project called Trie with the following classes in the trie package: TrieNode, Trie, and TrieApp.

There are also a number of sample test les of words directly under the project folder (see the <strong>Testing</strong> section that follows.)

You will implement the following methods in the Trie class:

<strong>(50 pts)</strong> buildTrie: Starting with an empty trie, builds it up by inserting words from an input array, one word at a time. The words in the input array are all lower case, and comprise of letters ONLY.

<strong>(30 pts)</strong> completionList: For a given search pre x, scans the trie ef ciently, gathers and returns an ArrayList of references to all leaf TrieNodes that hold words that begin with the search pre x (you should NOT create new nodes.) For instance, in the trie of Example 2 above, for search pre x “po” your implementation should return a list of references to these trie leaf nodes: [2,3,6],[6,3,5],[3,4,7],[4,4,5].

NOTE:

The order in which the leaf nodes appear in the returned list does not matter.

You may NOT search the words array directly, since that would defeat the purpose of building the trie, which allows for more ef cient pre x search. See the <strong>Pre             x Search</strong> section above. If you search the array, you will NOT GET ANY credit, even if your result is correct.

If your pre x search examines unnecessary nodes (see <strong>Pre                x Search</strong> section above), you will NOT GET ANY credit, even if your result is correct.

Make sure to read the comments in the code that precede classes, elds, and methods for code-speci c details that do not appear here. Also, note that the methods are all static, and the Trie has a single private constructor, which means NO Trie instances are to be created – all manipulations are directly done via TrieNode instances.

You may NOT MAKE ANY CHANGES to the <strong>Trie.java</strong> le EXCEPT to (a) ll in the body of the required methods, or (b) add private helper methods. Otherwise, your submission will be penalized.

You may NOT MAKE ANY CHANGES to the TrieNode class (you will only be submitting Trie.java). When we test your submission, we will use the exact same version of TrieNode that we shipped to you.

<h1>Testing</h1>

You can test your program using the supplied TrieApp driver. It rst asks for the name of an input le of words, with which it builds a trie by calling the Trie.buuldTree method. After the trie is built, it asks for search pre xes for which it computes completion lists, calling the Trie.completionList method.

Several sample word les are given with the project, directly under the project folder. (words0.txt, words1.txt, words2.txt, words3.txt, words4.txt). The rst line of a word le is the number of the words, and the subsequent lines are the words, one per line.

There’s a convenient print method implemented in the Trie class that is used by TrieApp to output a tree for veri cation and debugging ONLY. Our testing script will NOT look at this output – see the <strong>Grading</strong> section below.

When we test your program:

Words will ONLY have letters in the alphabet.

All words–for building the trie as well as for pre x searches–will be input in lower case. We will NOT input duplicate words.

We will NOT input two words such that one is a pre x of the other, as in “free” and “freedom”, i.e. a complete word will not be a pre x of another word.

Here are a couple of examples of running TrieApp:

The rst run is for <a href="https://www.cs.rutgers.edu/courses/112/classes/spring_2020_venugopal/progs/prog3/words3.txt">words3.txt</a>:




Enter words file name =&gt; words3.txt




TRIE




—root      |           doo      —(0,0,2)

|               door          —(0,3,3)          |               doom          —(3,3,3)

|           por      —(1,0,2)

|               pore          —(1,3,3)          |               port          —(2,3,3)




completion list for (enter prefix, or ‘quit’): do door,doom

completion list for: quit




The second run is for <a href="https://www.cs.rutgers.edu/courses/112/classes/spring_2020_venugopal/progs/prog3/words4.txt">words4.txt</a><a href="https://www.cs.rutgers.edu/courses/112/classes/spring_2020_venugopal/progs/prog3/words4.txt">:</a>




Enter words file name =&gt; words4.txt




TRIE




—root      |           cat      —(0,0,2)      |           mus      —(1,0,2)

|               muscle

—(1,3,5)          |               musk          —(5,3,3)

|           po      —(2,0,1)

|               pot          —(2,2,2)              |                   pottery

—(2,3,6)              |                   potato

—(6,3,5)

|               poss          —(3,2,3)              |                   possible

—(3,4,7)              |                   possum

—(4,4,5)




completion list for (enter prefix, or ‘quit’): pos possible,possum




completion list for: mu muscle,musk




completion list for: pot

pottery,potato




completion list for: quit




Try these tests with your implementation – your Trie printout MUST look IDENTICAL to the above. If your tree looks different, either your program logic is incorrect, or there is something different in the sequence of word inserts. In either case, you will not get any credit, so make sure you x your code.

Also try the other sample word les. AND, try with word les of your own, formatted exactly like the sample word les – rst line is number of words, then one word per subsequent line.

Go over the TrieApp code to understand how the Trie methods are called, and how the returned array list from completionList is processed to actually print the completion words.

<h1>Submission</h1>

Submit your <strong>Trie.java</strong> le.

<h1>Grading</h1>

The buildTrie method will be graded by <em>comparing the tree structure resulting from your implementation, with the correct tree structure produced by our implementation.</em> We will NOT be looking at the printout of the tree, the print method in the Trie class (used by TrieApp as in the above test examples) is for your convenience only.

The completionList method will be graded by inputting pre x strings to some of the trees created in buildTrie. However, these <em>trees will be created by our correct implementation of</em> buildTrie. In other words, to test your completionList implementation, we will NOT use your buildTrie implementation at all. This is fully for your bene t, because if your buildTrie implementation is incorrect, it will not adversely affect the credit you get for your completionList implementation. We will also make sure that the nodes you return belong to the Trie, and not some independent nodes you created outside of the Trie.