**Question**: [Perfect Rectangle](https://leetcode.com/problems/perfect-rectangle)

**Approach**:
It can create a rectangle, only when certain conditions are met:
1. The area of all the rectangles and the possible big rectangle formed should be the same
2. There should only be 4 outer corners.
3. The outer corners should only be the ones that are used to form the possible big rectangle, no others.

We can differentiate between the corneres by checking how many times are they repeated, if they are repeated evenly, then they are even corners.
Else if they are found only once, then they are an outer corner.

TC: `O(nlogn)` (because of map), can be optimized further to `O(n)` if used `unordered_map`, and key is `long long` (encode the x, y coordinates to achieve this)
SC: `O(n)` used for map

**Code**:
```cpp
class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        /*  
            so if i find the top-right and bottom-left points
            and also find the area of rectangle between them, and compare
            I can return true or false.

            But we also need to check if only 4 outer corners are left.
            For each rectangle, there are 4 corners, either all the corners are shared (interior rectangle), 1 corner is left which is the outer corner.

            All the interior corners will be their even number of times, so we can cancel them out by counting them. 
        */ 
        int minX = INT_MAX, minY = INT_MAX;
        int maxX = INT_MIN, maxY = INT_MIN;
        long long areaMerged = 0;
        unordered_map<vector<int>, int>corners;
        for(vector<int>&v : rectangles){
            int x1 = v[0], y1 = v[1], x2 = v[2], y2 = v[3];
            minX = min(minX, min(x1, x2));
            minY = min(minY, min(y1, y2));
            maxX = max(maxX, max(x1, x2));
            maxY = max(maxY, max(y1, y2));
            areaMerged += static_cast<long long>(abs(x2-x1))*static_cast<long long>(abs(y2-y1));

            // Counting the corners
            corners[{x1, y1}]++;
            corners[{x2, y2}]++;
            corners[{x1, y2}]++;
            corners[{x2, y1}]++;
        }
        long long maxRectArea = static_cast<long long>(abs(maxX - minX))*static_cast<long long>(maxY - minY);
        
        // Now for the corners 
        int outerCorners = 0;
        for(auto &[key, val] : corners){
            if(val%2)   outerCorners++;
            if(outerCorners > 4)    return false; 
        }

        /*
            Also, the outer corner should be the ones that are the corners of
            the big rectangle.
        */
        bool cornersOfBigRectangle = corners[{minX, minY}] == 1 && corners[{minX, maxY}] == 1
                                    && corners[{maxX, minY}] == 1 && corners[{maxX, maxY}] == 1;

        return areaMerged == maxRectArea && cornersOfBigRectangle;
    }
};
```