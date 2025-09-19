#### Point of Decision Trees
- Simple but powerful learning algorithm
- Widely used in Kaggle competitions
- Superb performance
- Mimics our daily decision-making processes
#### Structure
- Each internal node tests a feature
- The feature test determines the branches
- Each leaf node has a prediction
- Classification Tree:
	- At each leaf node, the prediction is the most common target value among all the data points. 
- Regression Tree: 
	- At each leaf node, the prediction is the mean target value among all the data points. 
- Can be used on continuous and discrete features
- Finding optimal decision tree is hard:
	- Optimal tree -> classifies all training examples correctly (can require one leaf per example and won't generalize)
		- Finding the smallest such tree is NP-complete (very hard to solve, but easy to check)
	- Decision trees can represent any function arbitrarily well
		- Universal function approximators -> can capture arbitrarily complex patterns
	- We will aim for a useful tree instead
- When to stop splitting data to check for maximum information gain?
	- When all the examples have the same label
	- When there are no features left
	- When a depth limit is reached
	- When the loss stops decreasing enough
####  Solution: Optimal Decision Tree
- Greedy strategy
- At each step, choose the feature (and split) that most reduces a loss
- Common loss functions:
	- Uncertainty
	- Impurity
	- Squared error
- Want to reduce uncertainty as quickly as possible -> quantify uncertainty using entropy
- Choose the feature and split that gives the largest information gain
#### Entropy
- Entropy of a distribution quantifies the uncertainty inherent in its possible outcomes
- Entropy Formula: $- \sum ~ p(x) ~ log_2~p(x)$ -> p(x) is the probability of each of the different outcomes 
- High Entropy:
	- Uniform-like distribution
	- Flat histogram
	- Sampled values are less predictable
- Low Entropy:
	- Concentrated on a few outcomes
	- Histogram concentrated
	- Sampled values are more predictable
- Properties: 
	- Entropy is always non-negative
	- Observing a variable never increases uncertainty of another variable.
	- If X and Y are independent, observing X does not change uncertainty about Y so IG = 0
	- If X fully determines Y, then X removes all uncertainty about Y then IG = H(Y) as H(Y|X) = 0
#### Information Gain 
- Observing X (feature) reduces uncertainty about Y (target)
- To observe Y, we observe the random variable X (its feature)
- Information Gain Formula: ]
	- Uncertainty about Y before observing X: H(Y) = $- \sum~p(y) ~log_2~p(y)$
		- Entropy formula
	- Uncertainty about Y after observing X: H(Y|X) = $- \sum~p(x) ~\sum ~p(y|x)~log_2~p(y|x)$
		- Expected conditional entropy
	- Information Gain: $IG(Y|X) = H(Y) -H(Y|X)$
		- mutual information
#### Advantages and Limitations
- Advantages:
	- Easy to understand and interpret
	- No need to normalize features
	- Handles both continuous and categorical features
	- Applies to both classification and regression
- Limitations:
	- Prune to overfitting
	- Strategies:
		- Set maximum depth
		- Stop if too few examples remain
		- Stop if further splits give minimal improvement
		- Select stopping criteria and split thresholds using a validation set
		- Ensemble methods:
			- Bagging: Average predictions from many trees
			- Boosting: Improve the trees sequentially
	- Greedy heuristic may not produce the optimal tree
	- Complex and less interpretable for large datasets
#### Worked Example:
- Count: 
	3 lemons, 4 oranges
	3/7 lemons, 4/7 oranges
	H(Y) = -3/7log_2 3/7 - 4/7 log_2 4/7
	= 0.985
	H(Y|X) =
	W <= 6.75 : 2 lemons 1 orange: 2/7, 1/7 -> 2/3, 1/3 -> -1/3 log_2 1/3 - 2/3 log_2 2/3
	W> 6.75 : 1 lemon 3 oranges: 1/7, 3/7 -> 1/4, 3/4 -> -1/4 log_2 1/4 - 3/4 log_2 3/4
	Information Gain = 0.985 - 0.857 = 0.128
	
