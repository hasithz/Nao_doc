# Custom GRU implementation 

GRU stands for gated recurrent unit.GRU is a type of recurrent neural network (RNN) that is commonly used for natural language processing (NLP) and speech recognition tasks. It is a variation of the RNN architecture that is designed to handle the problem of vanishing gradients.

In traditional RNNs, the hidden state is updated based on the current input and the previous hidden state, leading to information from early inputs in the sequence potentially being lost as the hidden state is updated many times. GRUs address this problem by using gating mechanisms, which allow the network to control the flow of information through the hidden state.

The GRU has two gates: the reset gate and the update gate. The reset gate determines how much of the previous hidden state should be discarded, and the update gate determines how much of the previous hidden state should be passed on to the new hidden state.

GRUs have become popular in NLP and speech recognition tasks due to their ability to handle sequential data effectively, while being computationally efficient. They can handle long-term dependencies in sequences and have the ability to preserve information from earlier inputs while also allowing the network to forget irrelevant information.

## Custom GRU network using pytorch 

```python 
class GRUCell(nn.Module):
    def __init__(self, input_size, hidden_size):
        super(GRUCell, self).__init__()
        self.input_size = input_size
        self.hidden_size = hidden_size
        self.reset_gate = nn.Linear(input_size + hidden_size, hidden_size)
        self.update_gate = nn.Linear(input_size + hidden_size, hidden_size)
        self.hidden_transform = nn.Linear(input_size + hidden_size, hidden_size)

    def forward(self, input, hidden):
        print("Input Shape:", input.shape)
        print("Hidden Shape:", hidden.shape)
        combined = torch.cat((input, hidden), dim=1)
        reset_gate = torch.sigmoid(self.reset_gate(combined))
        update_gate = torch.sigmoid(self.update_gate(combined))
        combined = torch.cat((input, reset_gate * hidden), dim=1)
        hidden_transform = torch.tanh(self.hidden_transform(combined))
        next_hidden = (1 - update_gate) * hidden + update_gate * hidden_transform
        output = torch.tanh(next_hidden)
        return next_hidden, output
```

here we will be using this as a classification so in order to implement as a classifier we use this as belows 

```python
class GRUClassifier(nn.Module):
    def __init__(self, input_size, hidden_size, num_classes):
        super(GRUClassifier, self).__init__()
        self.gru = GRUCell(input_size, hidden_size)
        self.fc = nn.Linear(hidden_size, num_classes)
        
    def forward(self, x, h0):
        out, hn = self.gru(x, h0)
        out = self.fc(out[-1,:])
        return out
```

!!! note

    input and hidden are the inputs to the GRU cell. The input should have dimensions (batch_size, input_size) and the hidden should have dimensions (batch_size, hidden_size).


    this has a better explanation on how GRU and LSTMs are made
    https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21


if needed to have more control 

```python 
class GRU_cell_AI_SUMMER(torch.nn.Module):
    """
    A simple GRU cell network for educational purposes
    """

    def __init__(self, input_length=10, hidden_length=20):
        super(GRU_cell_AI_SUMMER, self).__init__()
        self.input_length = input_length
        self.hidden_length = hidden_length

        # reset gate components
        self.linear_reset_w1 = nn.Linear(self.input_length, self.hidden_length, bias=True)
        self.linear_reset_r1 = nn.Linear(self.hidden_length, self.hidden_length, bias=True)


        self.linear_reset_w2 = nn.Linear(self.input_length, self.hidden_length, bias=True)
        self.linear_reset_r2 = nn.Linear(self.hidden_length, self.hidden_length, bias=True)
        self.activation_1 = nn.Sigmoid()

        # update gate components
        self.linear_gate_w3 = nn.Linear(self.input_length, self.hidden_length, bias=True)
        self.linear_gate_r3 = nn.Linear(self.hidden_length, self.hidden_length, bias=True)
        self.activation_2 = nn.Sigmoid()

        self.activation_3 = nn.Tanh()

    def reset_gate(self, x, h):
        x_1 = self.linear_reset_w1(x)
        h_1 = self.linear_reset_r1(h)
        # gate update
        reset = self.activation_1(x_1 + h_1)
        return reset

    def update_gate(self, x, h):
        x_2 = self.linear_reset_w2(x)
        h_2 = self.linear_reset_r2(h)
        z = self.activation_2( h_2 + x_2)
        return z

    
    def update_component(self, x,h,r):
        x_3 = self.linear_gate_w3(x)
        h_3 = r * self.linear_gate_r3(h) 
        gate_update = self.activation_3(x_3+h_3)
        return gate_update


    def forward(self, x, h):
        # Equation 1. reset gate vector
        r = self.reset_gate(x, h)

        # Equation 2: the update gate - the shared update gate vector z
        z = self.update_gate(x, h)

        # Equation 3: The almost output component
        n = self.update_component(x,h,r)

        # Equation 4: the new hidden state
        h_new = (1-z) * n  + z * h

        return h_new
```

!!! note 
    [github](https://github.com/The-AI-Summer/RNN_tutorial/blob/master/notebooks/custom_LSTM_RNN_AI_summer_experiment.ipynb)
    <!-- https://github.com/The-AI-Summer/RNN_tutorial/blob/master/notebooks/custom_LSTM_RNN_AI_summer_experiment.ipynb -->