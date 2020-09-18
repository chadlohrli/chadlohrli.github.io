---
title: "Sequence Models"
summary: "Pure numpy RNN, LSTM, & GRU"
---

[Github](https://github.com/chadlohrli/sequence-models)

{% assign image_path = '/assets/images/sequential_models' %}

### Background
Sequential neural networks are useful for tasks that require modeling data that contains
some form of temporal dependence. Since language is highly dependent on context, and context is derived many times by analyzing an entire sentence, sequential networks are exceptionally useful for finding these temporal dependencies that build context. To demystify how sequential networks perform this context building, it seems beneficial to go through the entire process of implementing these networks from scratch, taking a first principles approach to learning the inner workings of these models.

### Models
#### RNN - Recurrent Neural Network
<img src="{{site.url}}{{image_path}}/rnn.png" width="700" height="500" /><br/>
reference: <https://www.coursera.org/learn/nlp-sequence-models><br/>
Sequential networks are in reality just one unit cell where the output state ​**a**​​ is fed back into itself after each computation. This allows the network to keep updating itself with each time step ​**t** ​​of a training sequence and thus the outputs ​**y**​​ are dependent on all the previous states ​**a**​​ and input ​**x**​​. One consequence of the RNN is that because the network shares the same hidden weights, it is prone to unstable gradients leading to a common phenomena called the vanishing gradient problem. With longer sequences this issue becomes more a problem and thus the network has a hard time learning long term dependencies.   

#### LSTM - Long Short Term Memory
<img src="{{site.url}}{{image_path}}/lstm.png" width="400" height="200" /><br/>
reference: <https://www.coursera.org/learn/nlp-sequence-models><br/>
The LSTM is a much more complicated model but it performs well with longer sequences and attempts to fix the vanishing gradient problem. As opposed to the RNN, the LSTM has three extra gates, namely the forget gate, the update gate, and the output gate. Each of these gates are controlled by a sigmoid function which determines how relevant the information coming into the cell is. The state of the LSTM at any point is labeled by ​**c**​​ and gets updated by the input state **a**​​ after it is fed through each gate. There is noticeably better performance with the vanilla LSTM model when trained against longer sequences.


#### GRU - Gated Recurrent Unit
<img src="{{site.url}}{{image_path}}/gru.png" width="400" height="200" /><br/>
reference: <http://colah.github.io/posts/2015-08-Understanding-LSTMs/><br/>
The GRU similarly to the LSTM is an improved version of a basic recurrent neural network. In order to address the vanishing gradient problem it keeps the state vector to remember long term dependencies. The main distinction from the LSTM is that the forget and update gates are combined into one, which according to the creators (Cho, et al. 2014) is motivated by faster computation and easier implementation. Implementing the GRU with backpropagation turned out to be harder due to complicated partial derivatives (would any mathematicians be so kind to check my work? :) ). To test the GRU against the LSTM, a simple character-level model was trained on the Linux Kernel Code. The LSTM and GRU achieved an identical training accuracy over 100k iterations, although the GRU trained about 20% faster.

### Code Structure
<img src="{{site.url}}{{image_path}}/graph.png" width="700" height="500" />    
The structure of `sequential.py` follows the graph above and is inspired by the code presented in
[Coursera's NLP course](https://www.coursera.org/learn/nlp-sequence-models).

### Character Level Language Modeling
Each model can be tested by seeding it with a character and letting it predict the next characters in a word. Depending
on the type of data it is trained on it may generate a single word or a sequence (string) of words. Some of the datasets in the code are described below:   
<img src="{{site.url}}{{image_path}}/dataset.png" width="500" height="200" />   

### Results
Training the model on a subset of Elon Musk's tweets we can observe the results below:    
<img src="{{site.url}}{{image_path}}/elontweets.png" width="600" height="750" />     
It's interesting to see that each model is able to capture twitter contextual specifics such as the **t.co** links and
**@** handles. Another observation is even though the RNN seems to diminsh with more iterations (vanishing gradient), it's not apparent that its prediction capabilities are any worse than its superior models.

Training the model on a subset of the linux source code shows the limitations of the RNN:
<img src="{{site.url}}{{image_path}}/linux.png" width="600" height="750" />     
The models were all trained over 100k iterations.
While the LSTM and GRU seem to produce samples that look more or less like c code, the RNN's
output is gibberish. Another observation is the GRU trains faster the LSTM which satisifies the purpose
of its architecture.

Check out the code on github if you want to give these models a try: <https://github.com/chadlohrli/sequence-models>

### References
1. <https://www.coursera.org/learn/nlp-sequence-models>
2. <http://colah.github.io/posts/2015-08-Understanding-LSTMs/>
3. <http://karpathy.github.io/2015/05/21/rnn-effectiveness/>
4. <https://arxiv.org/pdf/1406.1078.pdf>


