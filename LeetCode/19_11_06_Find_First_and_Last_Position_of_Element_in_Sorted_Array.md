# 原题：

```
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

# 提交
## Java
### 第一次提交
```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n = nums.length;
        if(0 == n){
            return new int[]{-1,-1};
        }else if(1 == n){
            return nums[0]==target?new int[]{0,0}:new int[]{-1,-1};
        }else{
            //二分找左边界
            int l_beg = 0,l_end=n-1,left =-1;
            while(l_beg <= l_end){
                int l_mid = (int)(l_beg + l_end)/2;
                if(nums[l_mid] == target){
                    if(l_mid == 0){
                        left = 0;
                        break;
                    }else if(nums[l_mid-1]<target){
                        left = l_mid;
                        break;
                    }else{
                        l_end = l_mid -1;
                    }
                }else if(target > nums[l_mid]){
                    l_beg = l_mid +1;
                }else{
                    l_end = l_mid -1;
                }
            }
            if(left == -1){
                return new int[]{-1,-1};
            }else{
                //二分找右边界
                int r_beg = left,r_end=n-1,right =-1;
                while(r_beg <= r_end){
                    int r_mid = (int)(r_beg + r_end)/2;
                    if(nums[r_mid] == target){
                        if(r_mid == n-1){
                            right = n-1;
                            break;
                        }else if(nums[r_mid+1]>target){
                            right = r_mid;
                            break;
                        }else{
                            r_beg = r_mid +1;
                        }
                    }else if(target < nums[r_mid]){
                       r_end = r_mid -1;
                    }
                }
                return new int[]{left,right};
            }
        }
    }
}
```
最终结果：0 ms	43.6 MB

用了两次二分查找，分别找左边界和右边界。

## Python
```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        # 思路 二分法两次
        # 第一次找到的是左边界，即nums[m]==target and m ==0   or  nums[m] == target and nums[m-1] == target  
        # 第二次找到的是右边界，即nums[m]==target and m ==l-1 or  nums[m] == target and nums[m+1] == target
        if not nums:
            return [-1,-1]
        import sys
        left_index = -1
        right_index = -1
        # 可以扩充nums来解决边界问题
        nums.insert(0,-sys.maxsize-1)
        nums.append(sys.maxsize)
        left = 1
        right = len(nums)-1
        while(left <= right):
            m = (left + right) // 2
            if nums[m] == target and nums[m-1]<target:
                left_index = m-1
                break 
            elif nums[m]< target:
                left = m+1    
            elif nums[m] > target or (nums[m] == target and nums[m+1]>=target):
                right = m -1
        left = 1
        right = len(nums)-1        
        while(left <= right):
            m = (left + right) // 2
            if nums[m] == target and nums[m+1]>target:
                right_index = m-1
                break 
            elif nums[m]> target:
                right = m -1   
            elif nums[m] <target or (nums[m] == target and nums[m-1]<=target):
                left = m+1    
       
        # print(left_index,right_index)
        return [left_index,right_index]
```
最终结果：

群皮写的python版，十分之精妙@ChinaYiqun
