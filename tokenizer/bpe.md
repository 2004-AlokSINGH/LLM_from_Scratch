### How the BPE Algorithm Works
The algorithm follows an iterative process to build a vocabulary from a corpus of text:
1. Initial Splitting: Every word in the dataset is first split into individual characters.
2. End-of-Word Marking: An additional symbol (such as /w) is added to the end of each word to signal where the word concludes. This prevents the model from confusing subwords that appear in the middle of a word with those at the end.
3. Frequency Counting: The algorithm creates a frequency table of all words and individual characters present in the data.
4. Iterative Merging: The tokenizer identifies the most frequent pair of consecutive tokens and merges them into a single new "subword" token.
5. Repetition: This merging step is repeated until a pre-defined vocabulary size or number of iterations is reached.

Example:

The sources provide an example using a small dataset containing the words: "old", "older", "finest", and "lowest".

**Initial State**: The words are broken down into characters: o l d /w, o l d e r /w, f i n e s t /w, and l o w e s t /w.

 **Iteration 1** (Merging 'e' and 's'): The algorithm finds that the pair e and s appears 
most frequently (9 times in "finest" and 4 times in "lowest," totaling 13).
It merges them to create a new subword token: es.
Step 1:

<img width="241" height="168" alt="image" src="https://github.com/user-attachments/assets/00897617-01f5-4fe6-83a4-10c3615a01e5" />

**Iteration 2** (Merging 'es' and 't'): The new token es is frequently followed by t. 
These are merged to create the subword EST.


<img width="300" height="215" alt="image" src="https://github.com/user-attachments/assets/208900c0-6fb8-46a6-a48a-246f9a2ed2ca" />

**Iteration 3** (Merging 'EST' and '/w'): Since EST is always the end of the word in this specific dataset,
the algorithm merges it with the end-of-word symbol to create estw.

<img width="286" height="224" alt="image" src="https://github.com/user-attachments/assets/2c16ff67-a490-42e0-b3aa-7e06cacbb5ec" />


**Iteration 4** (Merging 'o', 'l', and 'd'): The algorithm identifies that o and l often appear 
together (in "old" and "older") and merges them into ol. Finally, it merges ol and d to create the root token old.

<img width="243" height="209" alt="image" src="https://github.com/user-attachments/assets/f74391cb-3a5d-4f29-9afd-ec28188c3e7b" />


<img width="323" height="200" alt="image" src="https://github.com/user-attachments/assets/6a5a43b0-1e2d-4af7-a479-4734852bb5ec" />


***Stopping Criteria** :- Iteration or token count


##### The Resulting Vocabulary
Through this process, the tokenizer creates a set of subword tokens like "old" and "estw".
