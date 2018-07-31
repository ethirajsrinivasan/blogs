## A Simple Tensorflow Classification Model in Ruby

The main objective of this blog is to build a simple linear classification model in ruby using Tensorflow architecture. The main tensorflow compenents required to build and test the model  are Operation, Placeholder, Variable and Session. These components are written as ruby classes. Lets start with the Operation class.

```ruby
class Operation
  attr_accessor  :input_nodes, :output, :input

  def initialize(*input_nodes)
    @input_nodes = input_nodes
  end

  def compute
    raise "Should be implemented"
  end

end
```

The `Operation` class forms the basis of all the computation in tensorflow. The `compute` method carries out the computation required. Let us create few classes which inherit the `Operation` class and we will use sigmoid as the activation function for the model

```ruby
require Matrix
class Add < Operation
  def initialize(*nodes)
    @input_nodes=nodes
  end

  def compute(x,y)
    return x.element(0,0) + y
  end
end

class Matmul < Operation
  def initialize(*nodes)
    @input_nodes=nodes
  end

  def compute(x,y)
    return Matrix[x] * Matrix[y].transpose
  end
end

class Sigmoid < Operation
  def initialize(*nodes)
    @input_nodes=nodes
  end

  def compute(z)
    1/(1+Math.exp(-z))
  end
end
```

Load the Matrix library at the begining for matrix computation. Now that we have created basic operation classes we will proceed to other components.

```ruby
class Placeholder
  attr_accessor  :value, :output
end

class Variable
  attr_accessor  :value, :output

  def initialize(value)
    @value = value
  end
end
```

The `Placeholder` class holds objects which are input or output of the model. These object act as constant and their values dont change during the session. The `Variable` holds objects whose values are either weights or bias given to the model. In the actual tensorflow model these values are updated using the optimizer(eg AdamOptimizer, Stocastic Gradient Descent etc). We wont be updating any value in our approach. Now we will define the session class.

```ruby
class Session

  def run(operation,feed_dict={})
    nodes = traverse_postorder(operation)
    nodes.each do |node|
      if node.is_a? Placeholder
        node.output = feed_dict[node.object_id]
      elsif node.is_a? Variable
        node.output = node.value
      else
        node.input = node.input_nodes.map { |input| input.output }
        node.output = node.compute(*node.input)
      end
    end
    operation.output
  end
  
  def traverse_postorder(operation)
    node_operators = []
    recurse(operation,node_operators)
  end

  def recurse(node,node_operators)
    if node.is_a?(Operation)
      node.input_nodes.each do |input_node|
        recurse(input_node,node_operators)
      end
    end
    node_operators << node
  end
  
end
```

The `Session` class takes care of all the executions. It first converts the set of operations to postfix order. The operations are then executed one by one. Since we have created all the components required, lets jump into action. For simplicity I have already solved the problem for the data shown below and used the solved weight and bias values

![](https://raw.githubusercontent.com/ethirajsrinivasan/blogs/master/ruby_tensorflow/linear_classifier.png)

```ruby
A = Variable.new([1,1])
b = Variable.new(-5)
x = Placeholder.new()
y = Matmul.new(A,x)
z = Add.new(y,b)
a = Sigmoid.new(z)
sess = Session.new()
```
Lets see the example for two extremes of the function

```ruby
result = sess.run(a,feed_dict={x.object_id=>[0,-10]})
puts(result)%
```
```sh
3.059022269256247e-07
```
The point `(0,-10)` lies towards the lower left corner of the graph hence the sigmoid value tends towards 0
```ruby
result = sess.run(a,feed_dict={x.object_id=>[8,10]})
puts(result)%
```
```sh
0.999997739675702
```
For the point `(8,10)` which lies near the upper right corner of the graph the sigmoid value tends towards 1. 

Hope this example gives a good understanding of basic components of tensorflow. Happy learning !!!
