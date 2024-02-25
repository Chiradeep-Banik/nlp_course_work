
# Natural Language Processing

The main goals for NLP is :
- to make sense of the natural human language...
- Find out similarites between words.

# Basic Terminalogy :
- **Corpus (C)**
    - All the words together forms the corpus.
    - Contain duplicate words as well
- **Vocabulary (V)**
    - All the unique words in out Corpus.
- **Document (D)**
    - One Entry in our dataset is one document
    - For example one row of data.
- **Word (W)**
    - A single word


# STEMMING v/s LEMMATIZATION
- Stemming is an algorithmic approach to convert the word into its root forms
    - But the root word may or may not be a real word
- Lemmatization is a look-up based approach to convert the words in to its root forms called **lemma**.
    - But here the root form will be real words.
- Stemming is Faster than Lemmatization
- Lemmatization is more readable


## Basic approaches to convert normal language to numbers : 
- Two types of approaches
    - Frequency Based:
        - One-hot-encoding
        - Bag-of-words
        - n-gram
        - TF-IDF
    - Prediction Based (Mostly usign deeplearning based algos):
        - Word2Vec

( Lets say we have a document containing lot of words )
### First Approach **(ONE-HOT-Encoding)**:
- Cons:
    - Sparse Data for a large vocabulary(V)
    - No Fixed size
    - No way to deal with out of vacabulary (OOV) problem
    - No way to capture sementic meaning

- Make a dictionary of unique words and convert them into one-hot-encoding
- Example :
```cpp
string document = "How are you";

/* 

we have 3 words so we will one-hot-encode them as

"How"=[1, 0, 0] 
"are"=[0, 1, 0] 
"you"=[0, 0, 1] 

*/

vector<vector<int>> encoding = [[1, 0, 0],[0, 1, 0],[0, 0, 1]];

```

### SECOND Approach (BAG-OF-WORDS)(uni-gram):
- We are taking the vocabulary and creating a v-sized vector for each document.
- Where each word's frequency in the document(D) is tracked
- Example
```cpp
    string document1 = "How are you";
    string document1 = "What about you";
    
    set<string> vocabulary = {"How","are" ,"you","What","about"};

    vector<int> bog_d1 = {1,1,1,0,0};
    vector<int> bog_d2 = {0,0,1,1,1};
    
    /*
        Here we are just keeping track of the frequencies of the words in each document
            D1   =   {"How","are","you","What","about"};
                        1     1      1      0      0
    */

```
- This can handle the OUT-of-Vocabulary problem that one-hot-encoding faced..
- Cons:
    - Sparse data
    - Does not capture semetic relationship
    - Does not consider ordering
    - DOes not consider any out-of-vocabulary data


### N-GRAMS(n-words bag-of-words):
- In spite of using the words of the vocabulary...this approach considers **group of n-words** for grouping..
- Example
```cpp
    string document1 = "How are you";
    string document1 = "What about you";

    // Lets make a bi-gram (2-gram)
    //our grouping would be like
    set<string> groups = {"How are","are you","what about","about you"};
    vector<int> n_gram_d1 = {1,1,0,0};
    vector<int> n_gram_d2 = {0,0,1,1};

```
- This approach can capture sementic relationship of a document(to some extent)
- Cons:
    - Still cannot deal with Out-of-vocabulary(OOV) problem
    - n-gram increases the feature numbers...thus making it more computationally expensive.

### Term_Frequency-Inverse_Document_Frequency(TF-IDF):
- Term_Frequency (TF)(w|D) =(Frequncy of the word(n(w)) in D) / (Total Numebr of words in D)
- Inverse_Document_Frequency (IDF)(w) = log((number of Documents)/(number of D in which word occures))
- Example
```cpp
    string document1 = "How are you";
    string document1 = "What about you";
    


    double tf(string word,string doc){
        return (freq_in_doc(word,doc))/(num_of_words_in_doc(doc));
    }

    double idf(int num_of_docs,string word){
        return log(num_of_docs/num_of_docs_with_word(word));
    }
    vector<double> tf_idf_d1 = {tf("How",document1)*idf(2,"How"),
                            tf("are",document1)*idf(2,"are"),
                            tf("you",document1)*idf(2,"you")};
    
    vector<double> tf_idf_d2 = {tf("What",document2)*idf(2,"What"),
                            tf("about",document2)*idf(2,"about"),
                            tf("you",document2)*idf(2,"you")};
                            
```
- Used in google search engine
- Cons:
    - Sparse output
    - can not deal out-of-vocabulary problem
    - If the vocabulary is large then huge dimentions
    - Cannot capture sementic relationship

### The drawbacks for both the approaches are 
- The encodings for each words cannot capture the similarity between two words
- I.E. The encodings are on different hyper-planes...so the dot product always is **ZERO**


### Embedding Appraoch :
- We will create a n-dimentional vector as embeddings where each value in the vector will give us a similarity coffiecient with that vector-plane
- In this way we can calculate how similar two words are to one another by doing a dot-product and getting a non-zero value.
- Example :
```cpp
string document = "How are you";
vector<string> words = {"How","are","you"};

/* 
We will craete lets-say a 3-dimentional encoding vector for each word...

vector<vector<int>> encode(vector<string> words,int n_dimentions){
    .....We will convert the words into embeddings
    return embeddings;
}

*/
vector<vector<int>> encodings = encode(worsd,3);

// encodings = {{1.392,4.23,1.23},{3.392,7.23,1.23},{0.392,1.22,9.23}}

```
- PROS : 
    - Captures the sementic meaning of the document
    - Low dimentional vectors ( thus less computationally intensive )
    - Dense vector ( i.e. none or very few are 0 )

