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

