# Pen and Paper Threshold Secret Sharing for BIP-39

## Introduction

When using hardware wallets, it is quite common that we have a single 
*recovery seed* consisting of 24 words, as specified by BIP-39 standard.
The general advice is never to enter these 24 words into any computer, 
as none of them can be trusted to be without backdoors and keyloggers and 
fishing for recovery seeds is potentially too profitable to assume that it 
is not already going on on a massive scale.

Writing down these 24 words is a good idea, but trusting them to third 
parties is risky, as they cannot be expected to go to the same lengths 
protecting their secrecy as we do, as they have far less skin in this 
game. However, we can split the secret into three parts in such a way 
that none of the trusted parties can recover our seed from what they 
have, but we can recover from any two parts. As long as our trusted 
third parties do not collude against us, our recovery seed is safe. 
Thus, we are protected from losing access to our seed as long as at 
least two of the shares are available for us as well as from it getting 
into the hands of thiefs as long as at least two are out of their reach.

In this description, you find a cryptographic algorithm that can be 
performed by pen and paper but cannot be broken even by supercomputers.

## Algorithm Description and Justification

First, the 24 words are shuffled in a random order, using 24 numbered 
pieces of paper.

Then, we put (different) 16 words in each of the three envelopes, such 
that words 1-16 (in the random ordering) go into the first envelope, words
9-24 go into the second envelope and words 1-8 and 17-24 go into the third 
envelope.

In each envelope, we also put two random permutations: *yellow 1* and
*red 1* into the first envelope, *green 1* and *yellow 2* into the second 
envelope, and *red 2* and *green 2* into the third envelope. They are 
constructed in such a way that the sequential application of permutations 1 
and 2 of the same color to the random ordering of words would result 
in the correct ordering of our recovery seed.

Attackers that get their hands on these 24 words without the ordering 
need to search through 24! ≈ 2⁷⁹ of possible permutations. The permutations 
enclosed in one envelope reveal zero information about the correct ordering, 
as for any ordering there exists a corresponding "other" permutation matching 
the ones in the envelope.

Similarly, the missing 8 words in each envelope force the attacker to search 
through 2048⁸ = 2⁸⁸ possibilities to recover all the words. However, this 
already includes their ordering (8! ≈ 2¹⁶ possibilities), so the corresponding 
entropy needs to be subtracted. Thus, in order to recover all the 24 words 
in the correct order having access to only one of the envelopes, the attacker 
needs to search through 79 + 88 - 16 bits of entropy.

However, the 24 words encode 264 bits, of which only 256 bits encode the 
random seed, making 8 bits redundant and the total entropy facing the 
attacker 79 + 88 - 16 - 8 = 143 bits. Searching through such a huge key space 
is not feasible with currently available computing power and can be 
expected to be remain so for the foreseeable future.

## Example

Suppose that your BIP-39 seed is the following sequence of 24 words:

     1	speak
     2	horror
     3	funny
     4	gasp
     5	thing
     6	please
     7	disorder
     8	hover
     9	sauce
    10	gloom
    11	cabbage
    12	know
    13	short
    14	razor
    15	glide
    16	general
    17	knife
    18	again
    19	switch
    20	worry
    21	lobster
    22	pond
    23	tomato
    24	decorate

Now, put your 24 numbered pieces of paper into a hat and pull them out 
in a random order. Write down the sequence and its ordinal numbers:

```
16,21,12, 7,13,10,23,15,18,11,22, 1,19, 4, 6, 9,14, 2,20, 8, 5,17, 3,24
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```

On three clean sheets of paper, write down the words corresponding to 
the first sixteen, the last sixteen and the first and the last eight 
numbers, respectively, together with their numbers in the new ordering 
as follows:

### Sheet one

     1  general
     2  lobster
     3  know
     4  disorder
     5  short
     6  gloom
     7  tomato
     8  glide
     9  again
    10  cabbage
    11  pond
    12  speak
    13  switch
    14  gasp
    15  please
    16  sauce

### Sheet two

     9  again
    10  cabbage
    11  pond
    12  speak
    13  switch
    14  gasp
    15  please
    16  sauce
    17  razor
    18  horror
    19  worry
    20  hover
    21  thing
    22  knife
    23  funny
    24  decorate

### Sheet three

     1  general
     2  lobster
     3  know
     4  disorder
     5  short
     6  gloom
     7  tomato
     8  glide
    17  razor
    18  horror
    19  worry
    20  hover
    21  thing
    22  knife
    23  funny
    24  decorate

Now, put the numbered pieces of paper back into the hat and draw them out 
in a random order. Write down the sequence (with the ordinals) to
**sheet one**. For example:
```
15,12, 9,11, 3,23,10,20,21,14,19, 5, 7,16,22,13, 8, 1, 2,17, 6,24,18, 4
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```
Then do another draw, and write down the sequence on **sheet one** again.
For example:
```
24,19,10,18,11, 5,15,16, 3, 1,12, 6, 7,14,17,23, 4, 8,21,20, 2, 9,22,13
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```
Do a third draw, and write down the sequence, this time on **sheet two**:
```
 4, 5,21,14, 7, 6,18,16,15,19,24,22,12,13, 9, 1,20,17, 2, 3, 8,10,11,23
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```

Now, write the *complementary* sequence of the *first* sequence on 
**sheet one** to **sheet two** as follows:
Word 1 of the shuffled sequence (`1 general`) goes to position 15 
according to first sequence of **sheet one** (as 1 is below 15), but 
in the end, it needs to go to position 16 (as 16 is above 1 in the 
original shuffle). Therefore, write 16 in position 15 of the 
*complimentary sequence*. Similarly, word 2 (`lobster`) goes to 12, 
but it should go to 21. Thus, write 21 in position 12. And so on. 
The second sequence on **sheet two** thus becomes:
```
 2,20,13,24, 1, 5,19,14,12,23, 7,21, 9,11,16, 4, 8, 3,22,15,18, 6,10,17
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```

Using the exact same algorithm, write the *complementary* sequence of
the *second* sequence on **sheet one** to **sheet three**:

```
11, 5,18,14,10, 1,19, 2,17,12,13,22,24, 4,23,15, 6, 7,21, 8,20, 3, 9,16
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```

Finally, using this algorithm again, write the *complementary* sequence 
of the *first* sequence of **sheet two** to **sheet three**:

```
 9,20, 8,16,21,10,13, 5, 6,17, 3,19, 4, 7,18,15, 2,23,11,14,12, 1,24,22
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```

To make restoring easier for yourself, you can mark the corresponding 
sequences with highlighters of the same color and number them. Thus, 
**sheet one** would have a yellow and a red sequence, both numbered 1, 
**sheet two** would have a green sequence numbered 1 and a yellow sequence 
numbered 2, **sheet tree** would have a red and a green sequence, both 
numbered 2.

At this point, each sheet has 16 words and two random sequences. Put these 
sheets in three different envelopes and hide them in three different places. 
If you entrust them to trustworthy people, one envelope each, do not tell 
them where the other two envelopes are. You can write it in your testament, 
though.

### Decryption

Suppose, you need to restore your seed and you can access envelopes **one** 
and **three**. The corresponding sequences are the *second* sequence of 
**sheet one** and the *first* sequence of **sheet three**. In our example:
```
24,19,10,18,11, 5,15,16, 3, 1,12, 6, 7,14,17,23, 4, 8,21,20, 2, 9,22,13
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```
and
```
11, 5,18,14,10, 1,19, 2,17,12,13,22,24, 4,23,15, 6, 7,21, 8,20, 3, 9,16
 1, 2, 3, 4, 5, 6, 7, 8, 9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24
```

We have all the 24 words with ordinal numbers in the shuffled sequence, and 
we need to place them into their final position. We simply need to look up 
the final position of each word in the two sequences one after the other:
Word 1 `general` is 24 in the first sequence and 24 is 16 in the second 
sequence. Therefore in the final order we have `16 general`. Word 2 `lobster` 
goes 2→19→21, resulting in `21 lobster`. Word 3 `know` goes 3→10→12, thus 
`12 know`. And so on, until we recover all the words.
