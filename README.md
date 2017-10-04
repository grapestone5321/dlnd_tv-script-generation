# TV-Script Generation

In this project, you'll generate your own Simpsons TV scripts using RNNs. You'll be using part of the Simpsons dataset of scripts from 27 seasons. The Neural Network you'll build will generate a new TV script for a scene at Moe's Tavern.

## Get the Data

The data is already provided for you. You'll be using a subset of the original dataset. It consists of only the scenes in Moe's Tavern. This doesn't include other versions of the tavern, like "Moe's Cavern", "Flaming Moe's", "Uncle Moe's Family Feed-Bag", etc..

## Explore the Data

Play around with view_sentence_range to view different parts of the data.

## Implement Preprocessing Functions

The first thing to do to any dataset is preprocessing. Implement the following preprocessing functions below:

- Lookup Table
- Tokenize Punctuation

### Lookup Table

To create a word embedding, you first need to transform the words to ids. In this function, create two dictionaries:

- Dictionary to go from the words to an id, we'll call vocab_to_int 
- Dictionary to go from the id to word, we'll call int_to_vocab 

Return these dictionaries in the following tuple (vocab_to_int, int_to_vocab)

### Tokenize Punctuation

We'll be splitting the script into a word array using spaces as delimiters. However, punctuations like periods and exclamation marks make it hard for the neural network to distinguish between the word "bye" and "bye!".

Implement the function token_lookup to return a dict that will be used to tokenize symbols like "!" into "||Exclamation_Mark||". Create a dictionary for the following symbols where the symbol is the key and value is the token:

- Period ( . )
- Comma ( , )
- Quotation Mark ( " )
- Semicolon ( ; )
- Exclamation mark ( ! )
- Question mark ( ? )
- Left Parentheses ( ( )
- Right Parentheses ( ) )
- Dash ( -- )
- Return ( \n )

This dictionary will be used to token the symbols and add the delimiter (space) around it. This separates the symbols as it's own word, making it easier for the neural network to predict on the next word. Make sure you don't use a token that could be confused as a word. Instead of using the token "dash", try using something like "||dash||".

## Preprocess all the data and save it

Running the code cell below will preprocess all the data and save it to file.

# Check Point

This is your first checkpoint. If you ever decide to come back to this notebook or have to restart the notebook, you can start from here. The preprocessed data has been saved to disk.

## Build the Neural Network

You'll build the components necessary to build a RNN by implementing the following functions below:

- get_inputs
- get_init_cell
- get_embed
- build_rnn
- build_nn
- get_batches

Check the Version of TensorFlow and Access to GPU

### Input

Implement the get_inputs() function to create TF Placeholders for the Neural Network. It should create the following placeholders:

- Input text placeholder named "input" using the TF Placeholder name parameter.
- Targets placeholder
- Learning Rate placeholder

Return the placeholders in the following tuple (Input, Targets, LearningRate)

## Build RNN Cell and Initialize

Stack one or more BasicLSTMCells in a MultiRNNCell.

- The Rnn size should be set using rnn_size 
- Initalize Cell State using the MultiRNNCell's zero_state() function
- Apply the name "initial_state" to the initial state using tf.identity() 

Return the cell and initial state in the following tuple (Cell, InitialState)

### Word Embedding

Apply embedding to input_data using TensorFlow. Return the embedded sequence.

### Build RNN

You created a RNN Cell in the get_init_cell() function. Time to use the cell to create a RNN.

- Build the RNN using the tf.nn.dynamic_rnn()
- Apply the name "final_state" to the final state using tf.identity() 

Return the outputs and final_state state in the following tuple (Outputs, FinalState)

### Build the Neural Network

Apply the functions you implemented above to:

- Apply embedding to input_data using your get_embed(input_data, vocab_size, embed_dim) function.
- Build RNN using cell and your build_rnn(cell, inputs) function.
- Apply a fully connected layer with a linear activation and vocab_size as the number of outputs.

Return the logits and final state in the following tuple (Logits, FinalState)

Save Parameters
Save seq_length and save_dir for generating a new TV script.### Batches

Implement get_batches to create batches of input and targets using int_text. The batches should be a Numpy array with the shape (number of batches, 2, batch size, sequence length). Each batch contains two elements:

The first element is a single batch of input with the shape [batch size, sequence length] 

The second element is a single batch of targets with the shape [batch size, sequence length] 

If you can't fill the last batch with enough data, drop the last batch.

For exmple, get_batches([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20], 3, 2) would return a Numpy array of the following:

     [
       # First Batch
       [
         # Batch of Input
         [[ 1  2], [ 7  8], [13 14]]
         # Batch of targets
         [[ 2  3], [ 8  9], [14 15]]
       ]

       # Second Batch
       [
         # Batch of Input
         [[ 3  4], [ 9 10], [15 16]]
         # Batch of targets
         [[ 4  5], [10 11], [16 17]]
       ]

       # Third Batch
       [
          # Batch of Input
         [[ 5  6], [11 12], [17 18]]
         # Batch of targets
         [[ 6  7], [12 13], [18  1]]
       ]
     ]

Notice that the last target value in the last batch is the first input value of the first batch. In this case, 1. This is a common technique used when creating sequence batches, although it is rather unintuitive.

## Neural Network Training

### Hyperparameters

Tune the following parameters:

- Set num_epochs to the number of epochs.
- Set batch_size to the batch size.
- Set rnn_size to the size of the RNNs.
- Set embed_dim to the size of the embedding.
- Set seq_length to the length of sequence.
- Set learning_rate to the learning rate.
- Set show_every_n_batches to the number of batches the neural network should print progress.

### Build the Graph

Build the graph using the neural network you implemented.

## Train

Train the neural network on the preprocessed data. If you have a hard time getting a good loss, check the forms to see if anyone is having the same problem.

## Save Parameters

Save seq_length and save_dir for generating a new TV script.

# Checkpoint

## Implement Generate Functions

### Get Tensors

Get tensors from loaded_graph using the function get_tensor_by_name(). Get the tensors using the following names:

- "input:0"
- "initial_state:0"
- "final_state:0"
- "probs:0"

Return the tensors in the following tuple (InputTensor, InitialStateTensor, FinalStateTensor, ProbsTensor)

### Choose Word

Implement the pick_word() function to select the next word using probabilities.

## Generate TV Script

This will generate the TV script for you. Set gen_length to the length of TV script you want to generate.

# The TV Script is Nonsensical

It's ok if the TV script doesn't make any sense. We trained on less than a megabyte of text. In order to get good results, you'll have to use a smaller vocabulary or get more data. Luckly there's more data! As we mentioned in the begging of this project, this is a subset of another dataset. We didn't have you train on all the data, because that would take too long. However, you are free to train your neural network on all the data. After you complete the project, of course.

# Submitting This Project

When submitting this project, make sure to run all the cells before saving the notebook. Save the notebook file as "dlnd_tv_script_generation.ipynb" and save it as a HTML file under "File" -> "Download as". Include the "helper.py" and "problem_unittests.py" files in your submission.
