- Data set with N data points: $(x^{(1)}, t^{(1)}), (x^{(2)}, t^{(2)}), ..., (x^{(N)}, t^{(N)})$
- For the $i^{th}$ data point:
	- $x^{(i)} \in R^D$ is the input feature vector and 
	- $t^{(i)} \in R$ is the scalar target
![[Pasted image 20250917194246.png]]
- Goal is to learn a function $f:R^D -> R$ such that 
	- $t^{(i)} \approx y^{(i)} = f(x^{(i)})$ for every data point i 
#### Regression Problem Examples
- Predicting Housing Price
	- Input Vector: Square footage, location, no. of bedrooms and bathrooms
	- Scalar Output: price of the house
- Predicting rainfall 
	- Input Vector: Temperature, humidity, wind
	- Scalar Output: Amount of Rainfall
- Predicting company revenue 
	- Input Vector: Previous sales data
	- Scalar Output: Company's revenue
#### Model
- Our goal is to learn a function f. 
- A model encodes a set of assumptions about f or a set of restrictions on f. 
- The model restricts the hypothesis space or the possible functions f that we learn. 
- The model itself: 
	- $y^{(i)} = w_1x_1^{(i)} + ... + w_Dx_D^{(i)} +b$
	- Compact Form: $y^{(i)} = \sum w_jx_j^{(i)} + b$
![[Pasted image 20250917201339.png]]
#### Loss Function
- $L(y^{(i)}, t^{(i)})$ quantifies how bad the hypothesis is for the $i^{th}$ example if the algorithm predicts $y^{(i)}$ but the target is $t^{(i)}$. 
- Squared Error Loss Function: 
	- $1/2 (y^{(i)} - t^{(i)})^2$
#### Cost Function (Mean Squared Error MSE)
- The cost of a model is the average loss of the model across the training set. 
![[Pasted image 20250917201702.png]]
- In practice, loss and cost is used interchangeably
![[Pasted image 20250917201810.png]]
#### Point of Vectorizing
- Faster: Operations run in parallel on CPU/GPU -> reduced computation time
- Cleaner Code: no explicit loops -> easier to read and maintain
- Better memory use: Efficient handling of large datasets
- Optimized support: libraries (e.g. NumPy) built for vectorized operations
#### Direct Solution to Linear Regression 
- How to choose w to minimize the cost function $\epsilon$ with respect to w?
	- Set to derivatives to zero and solve for the parameters
	- ![[Pasted image 20250917204651.png]]
	- ![[Pasted image 20250917204935.png]]
- Advantages:
	- Deriving the solution requires no iterations
- Disadvantages:
	- Computing the wtv man can be expensive if D is large 
		- Matrix inversion is an $O(D^3)$ algorithm
	- Does not generalize to other models or loss functions
#### Linear Regression Properties
Advantages: 
- Easy to understand and interpret
- Computationally efficient to train
- Good baseline model
Disadvantages:
- Assumes linear relationship
- Sensitive to outliers
- Limited to continuous features
- Struggles if some features are highly correlated
- High bias model