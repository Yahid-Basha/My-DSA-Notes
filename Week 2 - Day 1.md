- [Minimum boats to save people](#boats-to-save-people)

## Container with Most Water

### Two Pointer

```Java
  public int maxArea(int[] heights) {
      int l = 0;
      int r = heights.length-1;
      int maost_water = 0;
      while(l < r){
	  	int width = r-l;
		int height = Math.min(heights[l], heights[r]);
		most_water = Math.max(most_water, width*height);
		if(heights[l] < heights[r]){
			l++;
		}else{
			r--;
		}
	  }
	  return most_water;
  }
  ```
 ---
  
  ### Optimised a bit
  ---
  
  ```Java
  	public int maxArea(int[] heights) {
        int l = 0;
        int r = heights.length-1;
        int maost_water = 0;
		
        while(l < r){
  	  		int width = r-l;
  			int height = Math.min(heights[l], heights[r]);
  			most_water = Math.max(most_water, width*height);
			
			// w' * h' < w*h for every h' <= h because w' is less than w for every increment in l
			
			while(l < r && heights[l] <= h){
				l++;
			}
			while(l < r && heights[r] <= h){
				r--;
			}
		}
		return most_water;
	}
```


## Boats to Save People

##### Medium

- You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit.

- _Return the minimum number of boats to carry every given person._

Example 1:


Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
Example 2:

Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
Example 3:

Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
 

Constraints:

1 <= people.length <= 5 * 104
1 <= people[i] <= limit <= 3 * 104

##### Approach

 ---

 ## Approach

 1. **Sort the array**

    * Sorting ensures we can always try to pair the lightest person with the heaviest person.
    * If the heaviest cannot pair with the lightest, they cannot pair with anyone else (since everyone else is heavier than the lightest).

 2. **Two-pointer greedy strategy**

    * Start `l` at the lightest (index `0`) and `r` at the heaviest (last index).
    * If `people[l] + people[r] <= limit`, they share a boat → increment `l` and decrement `r`.
    * Otherwise, the heaviest (`r`) goes alone → decrement `r`.
    * Increment `boats` every loop iteration (since we always use one boat per step).

 3. **Why this is optimal**

    * By pairing the heaviest possible person with the lightest, we minimize unused space per boat.
    * Sorting + two pointers guarantees we never leave a lighter person unpaired when they could have fit.
    * This avoids the waste of brute-force pairing and is O(n log n) due to sorting.

 ---

 ## Dry Run Example

 **Input:**

 ```
 people = [3, 2, 2, 1], limit = 3  
 ```

 **Sorted:**

 ```
 [1, 2, 2, 3]  
 ```

 **Steps:**

 * `l = 0 (1)`, `r = 3 (3)` → `1 + 3 > 3` → Boat with `3` alone → `boats = 1`, `r--` → `r = 2`
 * `l = 0 (1)`, `r = 2 (2)` → `1 + 2 <= 3` → Pair them → `boats = 2`, `l++`, `r--` → `l = 1`, `r = 1`
 * `l == r` → One person left (`2`) → Boat alone → `boats = 3`

 **Output:** `3`

 ---
 

##### Code

###### Readable:

```Java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int boats = 0;
        int l = 0;
        int r = people.length-1;
        while(l <= r){
            if( l == r){
                boats++;
            }
            if(people[l]+people[r] == limit){
                l++;
                r--;
                boats++;
            }else{
                r--;
                boats++;
            }
        }
        return boats;
    }
}
```
###### Less Lined
```Java
class Solution {
    public int numRescueBoats(int[] people, int limit) {
        Arrays.sort(people);
        int boats = 0;
        int l = 0;
        int r = people.length-1;
        while(l <= r){
            if(people[l]+people[r] == limit){
                l++;
            }
            r--;
            boats++;
        }
        return boats;
    }
}
```








