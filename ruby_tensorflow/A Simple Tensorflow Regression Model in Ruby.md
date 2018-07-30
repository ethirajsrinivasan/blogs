## A Simple Tensorflow Regression Model in Ruby

The main objective of this blog is to build a simple linear regression model in ruby using Tensorflow architecture. The main tensorflow compenents required to build and test models  are Operation, Placeholder, Variable and Session. These components are written as ruby classes. Lets start with the Operation class.

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

The `Operation` class forms the basis of all the computation in tensorflow. The `compute` method carries out the computation required. Let us create few classes which inherit the `Operation` class

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

Load the Matrix class at the begining which is equivalent to python's `numpy` library. Now that we have created basic operation classes we will proceed other components

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

The `placeholder` class holds objects which are input and output of the model. These object act as constant and their values dont change during the computation. The `Variable` holds values which are changable. These values are either weights or bias given to the model. Now we will define the session class.
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

The `session` class takes care of all the execution. It first converts the set of operations to postfix order. The operations are then executed one by one. Since we have created all the components required. Lets jump into action. We will first create the placeholders  
