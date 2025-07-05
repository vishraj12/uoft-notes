- For each input $x \in R^d$, we have a corresponding label *y*
- Our goal: For a new query input x, we need to assign the point to a label
- Idea: Set the label of the query input to be the label of the closest point (neighbor) in the training set.
##### Defining Neighbor
- As inputs $x \in R^d$, Euclidean distance can be utilized:
	- $distance(x^{(a)}, x^{(b)}) = ||x^{(a)} - x^{(b)}||_2 = \sqrt{\sum_{j = 1}^d (x_j^{(a)} - x_j^{(b)})^2}$
- Find the nearest input vector to x in the training set and copy its label.
##### Pseudocode for Nearest Neighbor 
- Given the query $x^q \in R^d$ and training set $|D| = N$:
	- dv = [] (empty list to store all the distances from the query to the points in the set)
	- midx = 0 (index of the nearest point -> set to 0 for now)
	- mindist = $\infty$ (distance set as infinity for now -> arbitrary)
- loop through all the points in the training set and calculate the Euclidean distance between the points and the query and append to dv
- find the index of the minimum distance by looping through dv. 
```python
	for i = 0 to N-1:
    if dv[i] < mindist:
        mindist = dv[i]
        midx = i
```
return the index (midx) and distance (mindist)

##### Decision Boundaries
- Can be visualized using a Voronoi diagram:
	- Discretize space into tiny boxes and for each box, run the KNN algorithm to classify it
	- Merge boxes close together that have the same label
- Decision boundary is the boundary between regions of input space assigned to different categories
##### Noise
- The algorithm is sensitive to label noise or mis-labeled data (class noise) 
	- Solution: Smooth by having k nearest neighbors vote