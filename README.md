# DL_tv_script_generation
DL_tv_script_generation is a project that deals with RNN and text embedding in order to generate a TV script.
This Project is mandatory in the Udacity Nanodegree Program that i am pursuing. I will be commenting on the key learning from this project here.

## Installation
I was using the workspace of Udacity. So this is out of the scope of this repository unfortunately.

### Prerequisites
#### Skills / Knowledge
understand and/or have experience with:
- the functioning of RNN: Model building (choice LSTM or GRU, hyperparameters: number of hidden layers and their dimensions, etc.), feedforward thru time design, backpropagation for model updating, optimizer definition, loss function definition 
- the logic and the functioning of text embedding: setting the appropriate dimension and padding size.
- the functioning of transfer learning and how to choose the strategy giving the quantity of the new data and the similarity of the data and the use case.
- the integration of the whole model and the setting of adeauqate parrameters: value of learning rate, of minibatch size, number of epochs, etc.

#### Infrastructure 
I was using the workspace of Udacity. So this is out of the scope of this repository unfortunately. 

## Key learning (and best practices)
**1. Pre-processing**
### Lookup Table
Build a look up table for all vocabulary: in order to index them in a dictionnaries to find vocab from index and index from vocab respectively int_to_vocab and vocab_to_int.

### Tokenize Punctuation
Replace punctuation by word.
**2. Data Batching **
### Mini batch design
- design the sequence generation engine: taking the neighboring vocab to build the sequence of needed length. 
- the label each time is the sequence_length +1th word.
So we have the input output of a supervised prediction. The model will have to predict the correct word based on the sequence passed as input by learning on these examples.

### Test dataloader
In order to test the data formatting.

**2. Data mining **
### Model architecture building 
Build the logic of words embeddings, reducing the dimensionality of the vocab into the fixed number of embedding dimensions. Predict the sequence_length +1th word using RNN.
Check dimensions consistency.
#### Define forward and backpropagation
put all of that in action: Stack in a sequential order the layers with the needed activation functions. backpropagate the error between the outputs of the last RNN cell and label created in the minibatching step.
- don't forget to initialise the hidden weights (hidden = rnn.init_hidden(batch_size)) at the beginning of each epoch.
- don't forget to initialise the optimizer gradient to zero (rnn.zero_grad()) at the beginning of each iteration (each batch of each epoch). 
- perform the error computation: the difference between the output of the forward pass thru the network and the desired target. 
- perform an optimizer step. 
And iterate this on 
#### Optimizer and loss function definition
Both were predefined by default. Since it is a classification, the loss function is the Cross-Entropy Loss function. the optimizer is Adam.
#### Model hyper-parameters optimization 
- When training and validating, we keep the focus on both training loss and validation loss, both should be decreasing. Optimize the number of epochs and the learning rate if the Losses are fluctuating. Stop the training when the validation Loss is starting to increase. 
- The test accuracy has a minimum threshold, if it is not met, this could mean that the model architecture or the train data transformer were insufficient to enable the model to generalize or to have enough prior knowledge and can't extract enough features, or that the parameters are not optimized well and are leading to overfitting (the case of high embedding size). I choise LSTM over GRU as it converged faster, an embedding size of 400 3 hidden layers of dimension 500. These hyperparameters were chosen after some iterations and tests with the data given in this very exercice.

