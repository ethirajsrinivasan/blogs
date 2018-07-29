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
class Add < Operation
  def initialize(*nodes)
    @input_nodes=nodes
  end

  def compute(x,y)
    return x.element(0,0) + y
  end
end

class Mul < Operation
  def initialize(*nodes)
    @input_nodes=nodes
  end

  def compute(x,y)
    return x * y
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
```

