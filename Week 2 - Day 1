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
