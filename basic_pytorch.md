# Tensors

A lot like numpy arrays, but

1. lots of different methods
2. can be run/stored (or whatever) on the gpu on command
3. support **autograd**

#### Fun little fact: 

You can create numpy arrays from torch tensors and VV very easily. When you do though, the two counterparts (ndarray and tensor) become kind of "entangled" in that altering one will alter the other.

## Autograd

Broadly speaking, pytorch sometimes automatically tracks all operations that occur or a given tensor, `t`. Having done so, pytorch can later be called upon to calculate the gradient of the values in `t` with respect to a given scalar value `s`, if `t` was involved in producing `s`. 

Every tensor has an attribute called `grad`, which is `None` by default. Each Tensor also has a  `.requires_grad` attribute. If the latter is `True` for a given Tensor `t`, then every tensor, `w` created as a result of applying some operation to `t` will (1) keep track of which operation that was and (2) also have `requires_grad == True`. The operation is stored in the `.grad_fn` attribute of `w`. The `.grad_fn` of tensors created by the user is `None`.

```python
a = torch.randn(3, 3, requires_grad=True)
print(a.grad_fn) # None

b = a * 2
print(b.grad_fn) # <MulBackward object at 0x7f2caf235cc0>

c = b.sum()
print(b.grad_fn) # <SumBackward0 object at 0x7f2c6e2596d8>
```

Presumably this object also keeps track of which tensors were involved in the relevant operation.

Having kept track of the exact operations applied to each tensor, pytorch is well equipped to calculate the gradient of any Tensor in a given chain of `.requires_grad==True` Tensors with respect to any other. For instance, it should be very easy for pytorch to find the gradient of `a` with respect to `b`, since it has a record (in `b.grad_fn`) that `b` was created by multiplying `a` buy 2. 

No gradients have actually been calculated yet though. 

```python
print(a.grad) # None
print(b.grad) # None
print(c.grad) # None
```

To calculate a gradient you need to use the `.backward()` method on a Tensor, say `o`. Doing so will make pytorch calculate the gradient of each Tensor in the string that led to the creation of `o`, with respect to `o`.

```python
c.backward()
print(a.grad) # a Tensor with the same size() as a
```

However, you can only call `.backward()` without arguments on a Tensor that represents a single scalar value (like `c`). If you wanted to calculate the gradient of your Tensors with respect to a multi-valued Tensor `m`, you would need to pass a tensor, `n` of the same `.size()` as `m` in to the `.backward` function (with each value in `m` representing the hypothetical upstream gradient for that value of `m`). In reality, when you call `.backward()` with no arguments, you are really calling `.backward(torch.Tensor(1))`.

# nn.Module

This is a special object representing a network. It will (or at least should) have a bunch of **layers** defined on it and stored in attribute variables. It should also have a `.forward` method, which takes an input (in pytorch it is expected that this will be a **minibatch**) and returns class scores, or some other useful output, by passing the input through the mentioned layers. 

For example:

```python

class Net(nn.Module):

    def __init__(self):
        super(Net, self).__init__()
        # 1 input image channel, 6 output channels, 5x5 square convolution
        # kernel
        self.conv1 = nn.Conv2d(1, 6, 5)
        self.conv2 = nn.Conv2d(6, 16, 5)
        # an affine operation: y = Wx + b
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        # Max pooling over a (2, 2) window
        x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        # If the size is a square you can only specify a single number
        x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        x = x.view(-1, self.num_flat_features(x))
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x

    def num_flat_features(self, x):
        size = x.size()[1:]  # all dimensions except the batch dimension
        num_features = 1
        for s in size:
            num_features *= s
        return num_features

net = Net()

```

## neural network layers

These layers will usually contain a bunch of weights in the form of a tensor. When a tensor is assigned as an attribute on a `nn.Module`, it is converted to a `Parameter`, which is just a Tensor with special qualities. One of these, of course, is `.requires_grad == true`. These parameter tensors can be accessed though `nn.Module.parameters()`. 

Since all the parameter tensors are on autograd, calling `.backward(some_tensor)` on the output of the `nn.Module`'s `.forward()` function will generate gradients for all the parameters (or at least all of the ones that were involved in `.forward()`).

## Loss functions

Although, in practice, you wouldn't call `.backward()` directly on the output of `nn.Module.forward()`, you'd create a **loss function** that would calculate a (scalar) loss (usually a measure of dissatisfaction) from that output, and call `.backward()` on that.

Generally in pytorch, loss functions take two arguments

1. output: the output of `nn.Module.forward()`
2. target: the ideal output of `nn.Module.forward()`

For example:

```python
output = net(input) # where net is a nn.Module
target = torch.randn(10)  # a dummy target, for example
target = target.view(1, -1)  # make it the same shape as output
criterion = nn.MSELoss()

loss = criterion(output, target)
print(loss) # tensor(1.3032, grad_fn=<MseLossBackward>)
print(loss.grad_fn) # <MseLossBackward object at 0x7f2c6e2596d8>
```

## nn.Module gradient calculation

As you might expect, you just need to call `loss.backward()`. You can access the gradients on the Module's parameters by accessing its layers (which, recall, are stored as attribute variables). A *very* important thing to remember to do though is reset the Module's gradient before calculating a new one. The gradient updates involved in `.backward()` are always cumulative and will stack on top of each other if you're not careful. 

Use `nn.Module.zero_grad()` to set all the `.grad` of all its parameters to tensors full of 0's.

```python
net.zero_grad()

print(net.conv1.bias) # tensor([ 0.0518, -0.1236, -0.1465, -0.1974, -0.0926,  0.0684], requires_grad=True)
print(net.conv1.bias.grad) # tensor([0., 0., 0., 0., 0., 0.])

loss.backward()

print(net.conv1.bias.grad) # tensor([-0.0022, -0.0088,  0.0092, -0.0287,  0.0049,  0.0047])
```

## gradient updates

At this point we've already done all the heavy lifting, all that remains is to iterate over all the parameters in the net and update them according to their respective gradients.

```python
learning_rate = 0.01
for f in net.parameters():
    f.data.sub_(f.grad.data * learning_rate) # subtract the gradient * learning rate in place
    # Tensor.data is just the tensor without extra stuff like
    # the value of required_grad
```

But pytorch is all about convenience, so they also have a bunch of **optimizer** classes that contain the parameters of a `nn.Module` and a `.step()` method, which automatically updates them according to:

1. The current gradients
2. Some, special predefined function (like sgd or nesterov or whatnot)

These optimizers can also be used (as well as the `nn.Module` itself) to reset the gradients of the parameters that they are in charge of.

```python
import torch.optim as optim

# create your optimizer
optimizer = optim.SGD(net.parameters(), lr=0.01)

optimizer.zero_grad()   # zero the gradient buffers
output = net(input)
loss = criterion(output, target)
loss.backward()
optimizer.step()    # Does the update
```