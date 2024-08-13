```json
{
    'layer1.weight': tensor([...]),
    'layer1.bias': tensor([...]),
    'layer2.weight': tensor([...]),
    'layer2.bias': tensor([...]),
    ...
}

```
this is the format of state dictionary


Use example

```python
import torch
import torch.nn as nn

# Define a simple neural network
model = nn.Sequential(
    nn.Linear(2, 2),
    nn.ReLU(),
    nn.Linear(2, 1)
)

# Save the model state_dict
torch.save(model.state_dict(), 'model_state.pth')

# Load the model state_dict into a new model
model2 = nn.Sequential(
    nn.Linear(2, 2),
    nn.ReLU(),
    nn.Linear(2, 1)
)
model2.load_state_dict(torch.load('model_state.pth'))

```
