In Python len(arr) gives the number of items in the array. 
In other programming languages like Java, arr.len gives you the capacity (or space allocated) in that array and not the number of items in that array (has be to kept track of yourself).
### Problems ###
#### 485. Max Consecutive Ones ####
``` python
class Solution(object):
    def findMaxConsecutiveOnes(self, nums): #my solution
        """
        :type nums: List[int]
        :rtype: int
        """
        lst = []
        start = 0
        for i in range(0, len(nums)):
            if nums[i] == 0:
                curr = i - start
                lst.append(curr)
                start = i + 1
            elif i == len(nums) - 1:
                curr = i - start + 1
                lst.append(curr)
        return max(lst)

class Solution: 
	def findMaxConsecutiveOnes(self, nums: list[int]) -> int: #their solution
		ans = 0 
		summ = 0 
		for num in nums: 
			if num == 0: 
				summ = 0 
			else: 
				summ += num 
				ans = max(ans, summ)
		return ans
                
```
Time Complexity: $O(n)$
Space Complexity: $O(1)$

#### 1295. Find Numbers with Even Number of Digits ####
``` python
class Solution(object):
    def findNumbers(self, nums): #my solution
        """
        :type nums: List[int]
        :rtype: int
        """
        total = 0
        for num in nums:
            if len(str(num)) % 2 == 0:
                total += 1
        return total

class Solution: 
	def findNumbers(self, nums: list[int]) -> int: #their solution
		ans = 0 
		for num in nums: 
			if 9 < num < 100 or 999 < num < 10000 or num == 100000: 
				ans += 1 
		return ans
```

#### 977. Squares of a Sorted Array ####
``` python
class Solution(object):
    def sortedSquares(self, nums): #my solution brute-forced
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        for i in range(0, len(nums)):
            nums[i] = nums[i] ** 2
        nums.sort()
        return nums

class Solution: 
	def sortedSquares(self, nums: list[int]) -> list[int]: #their solution not brute-forced
		n = len(nums) 
		l = 0 
		r = n - 1 
		ans = [0] * n 
		
		while n: 
			n -= 1 
			if abs(nums[l]) > abs(nums[r]): 
				ans[n] = nums[l] * nums[l] 
				l += 1 
			else: 
				ans[n] = nums[r] * nums[r] 
				r -= 1 
		return ans
```

Time Complexity: $O(n)$
Space Complexity: $O(n)$

#### 1089. Duplicate Zeros ####
``` python
class Solution(object):
    def duplicateZeros(self, arr): #not my solution, could not figure out how to keep it in the original length
    # dont completely understand -> revisit
        """
        :type arr: List[int]
        :rtype: None Do not return anything, modify arr in-place instead.
        """
        zeros = arr.count(0)
        i = len(arr) - 1
        j = len(arr) + zeros - 1

        while i < j:
            if j < len(arr):
                arr[j] = arr[i]
            if arr[i] == 0:
                j -= 1
                if j < len(arr):
                    arr[j] = arr[i]
            i -= 1
            j -= 1
```

#### 88. Merge Sorted Array ####
``` python
class Solution(object):
    def merge(self, nums1, m, nums2, n): #my solution
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        two_ptr = n - 1 
        total_len = n + m - 1
        one_ptr = m - 1
        while two_ptr >= 0 and one_ptr >= 0:
            if nums1[one_ptr] >= nums2[two_ptr]:
                nums1[total_len] = nums1[one_ptr]
                total_len -= 1
                one_ptr -= 1
            elif nums1[one_ptr] < nums2[two_ptr]:
                nums1[total_len] = nums2[two_ptr]
                two_ptr -= 1
                total_len -= 1
        if two_ptr > -1:
            nums1[0:two_ptr + 1] = nums2[0:two_ptr + 1]

class Solution: #their solution -> uses the len of nums2 as a counter
	def merge(self, nums1: list[int], m: int, nums2: list[int], n: int) -> None: 
		i = m - 1 # nums1's index (the actual nums) 
		j = n - 1 # nums2's index 
		k = m + n - 1 # nums1's index (the next filled position) 
		while j >= 0: 
			if i >= 0 and nums1[i] > nums2[j]: 
				nums1[k] = nums1[i] 
				k -= 1 
				i -= 1 
			else: 
				nums1[k] = nums2[j] 
				k -= 1 
				j -= 1
```

#### 27. Remove Element ####
``` python
class Solution: #simple solution, I over-complicated it
	def removeElement(self, nums: list[int], val: int) -> int: #their solution -> couldn't do it on my own
		i = 0 
		for num in nums: 
			if num != val: 
				nums[i] = num 
				i += 1 
		return i
```

#### 26. Remove Duplicates from Sorted Array ####
``` python
class Solution(object): #my solution
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        j = 1
        i = 0
        while i < len(nums) - 1:
            if nums[i] == nums[i + 1]:
                i += 1
            else:
                nums[j] = nums[i + 1]
                j += 1
                i += 1
        return j

class Solution: #their solution
	def removeDuplicates(self, nums: list[int]) -> int: 
		i = 0 
		for num in nums: 
			if i < 1 or num > nums[i - 1]: 
				nums[i] = num 
				i += 1 
		return i        
```

#### 1346. Check if N and Its Double Exist
``` python
class Solution(object): #my solution
    def checkIfExist(self, arr):
        """
        :type arr: List[int]
        :rtype: bool
        """
        if not arr: 
            return False
        
        for i in range(0, len(arr)):
            for j in range(0, len(arr)):
                if i != j and arr[i] == 2 * arr[j]:
                    return True
        return False

class Solution: #their solution
	def checkIfExist(self, arr: list[int]) -> bool: 
		seen = set() 
		for a in arr: 
			if a * 2 in seen or a % 2 == 0 and a // 2 in seen: 
				return True 
			seen.add(a) 
		return False
```

#### 941. Valid Mountain Array
``` python
class Solution(object): #my solution
    def validMountainArray(self, arr):
        """
        :type arr: List[int]
        :rtype: bool
        """
        if not arr or len(arr) < 3:
            return False
            
        tip = max(arr)
        tip_index = arr.index(tip)
        
        if tip_index == 0 or tip_index == len(arr) - 1:
            return False
            
        for i in range(1, tip_index):
            if arr[i - 1] >= arr[i]:
                return False
        for j in range(tip_index + 1, len(arr)):
            if arr[j - 1] <= arr[j]:
                return False
        
        return True

class Solution: #their solution
	def validMountainArray(self, arr: list[int]) -> bool: 
		if len(arr) < 3: 
			return False 
		
		l = 0 
		r = len(arr) - 1 
		
		while l + 1 < len(arr) and arr[l] < arr[l + 1]: 
			l += 1
		while r > 0 and arr[r] < arr[r - 1]: 
			r -= 1 
		
		return l > 0 and r < len(arr) - 1 and l == r
```

#### 1299. Replace Elements with Greatest Element on Right Side ####
``` python
class Solution(object): #chat gpt solution
    def replaceElements(self, arr):
        """
        :type arr: List[int]
        :rtype: List[int]
        """
        max_right = -1 
        for i in range(len(arr) - 1, -1, -1):
            curr = max_right
            max_right = max(max_right, arr[i])
            arr[i] = curr
        return arr

class Solution: #their solution
	def replaceElements(self, arr: list[int]) -> list[int]: 
		maxOfRight = -1 
		for i in reversed(range(len(arr))): 
			arr[i], maxOfRight = maxOfRight, max(maxOfRight, arr[i]) 
		return arr
```