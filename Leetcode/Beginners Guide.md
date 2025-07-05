#### 1480. Running Sum of 1d Array ####
```python
class Solution(object): #my solution
    def runningSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        curr_total = 0
        for i in range(0, len(nums)):
            curr_total = nums[i] + curr_total
            nums[i] = curr_total
        return nums
        
class Solution(object): #suggested solution
    def runningSum(self, nums):
        for i in range(1, len(nums)):
            nums[i] += nums[i-1]
        return nums
```
Space Complexity: $O(1)$
Time Complexity: $O(n)$
#### 1672. Richest Customer Wealth ####
``` python
class Solution(object): #my solution
    def maximumWealth(self, accounts):
        """
        :type accounts: List[List[int]]
        :rtype: int
        """
        max = 0
        curr = 0
        for i in range(0, len(accounts)):
            for j in range(0, len(accounts[i])):
                curr += accounts[i][j]
            if curr > max:
                max = curr
            curr = 0
        return max
        
class Solution: #solution I like
    def maximumWealth(self, accounts: List[List[int]]) -> int:
        return max([sum(acc) for acc in accounts])
```

Time Complexity: $O(n~x~m)$
Space Complexity: $O(1)$

#### 412. Fizz Buzz ####
``` python
class Solution(object):
    def fizzBuzz(self, n): #my solution
        """
        :type n: int
        :rtype: List[str]
        """
        lst = []
        for i in range(1, n + 1):
            if i % 3 == 0 and i % 5 == 0:
                lst.append("FizzBuzz")
            elif i % 3 == 0:
                lst.append("Fizz")
            elif i % 5 == 0:
                lst.append("Buzz")
            else:
                lst.append(str(i))
        return lst
```

Time Complexity: $O(n)$
Space Complexity: $O(1)$
#### 1342. Number of Steps to Reduce a Number to Zero ####
``` python
class Solution(object):
    def numberOfSteps(self, num): #my solution
        """
        :type num: int
        :rtype: int
        """
        count = 0
        while num != 0:
            if num % 2 == 0:
                num = num // 2 #can also use bit shifting
                count += 1
            else:
                num -= 1
                count += 1
        return count
```
Time Complexity: $O(logn)$
Space Complexity: $O(n)$