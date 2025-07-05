This has labeled examples of the correct behavior.

The training set consists of inputs and corresponding labels e.g.

| Task                    | Inputs         | Labels              |
| ----------------------- | -------------- | ------------------- |
| Object Recognition      | Image<br>      | Object category<br> |
| Image Captioning        | Image          | Caption             |
| Document Classification | Text           | Document category   |
| Speech-to-text          | Audio waveform | Text                |
##### Step 1 #####
Our task is to build a system that can use the provided training set in order to make predictions on new data. 

##### Step 2 #####
- Create mathematical notation for the problem using [[Supervised Learning Notation]]-> understand how to represent data. 
- Machine learning algorithms need to handle lots of types of data: images, text, audio, credit card details etc.
- Represent inputs as input vectors in $x \in R^d$
- Vectors are a great tool since we can then use linear algebra to create transformations of inputs into desired outputs
- Representation: mapping to another space that is easy to manipulate

###### Training Set
- The data can be arranged in one big vector but not the most ideal way. 
- A training set consists of a collection of pairs of an input vector $x \in R^d$ and its corresponding target, or label, y:
	- Regression: y is a real number (e.g. stock price) $y \in R$
	- Classification: y is an element of a discrete set {1, ..., C} $y \in R^C$
	- y can also be an image or a graph
- The training set is denoted as $D = {{ (x^{(1)}, y^{(1)}), ..., (x^{(N)}, y^{(N)}) }}$
	- $x \in R^d$ is the d dimensional vector of input data
	- $y \in R^K$ (K = 1 if y is a real number, K = |C| if y indicates which class $c \in C$ the label of the datapoint belongs to.)

##### Step 3
- Crafting an optimization problem that represents our goal: supervised learning
- Starting off with [[Nearest Neighbor]]



