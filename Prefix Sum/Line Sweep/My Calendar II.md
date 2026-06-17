**Question**: [My Calendar II](https://leetcode.com/problems/my-calendar-ii/)

**Approach**:
In this question, we can't really create an array to store # events per time, since the `0 <= startTime < endTime <= 1e9`.
So, we use a map to store the events in an order, eventTime is key, and change is the value.
Then for every `book()`, we do a line sweep and check if # events exceeded 2. If it did, we return `false`, else return `true` 

TC: `O(n)` for each `book()`, where n is the # of keys present in map
SC: `O(n)` used for map

**Code**:
```cpp
class MyCalendarTwo {
public:
    map<int, int>mp;
    /*
        We initialize a map that does the following:
        1. Sorts the event time 
        2. Stores the event inc or dec
    */
    MyCalendarTwo() {
        
    }
    
    bool book(int startTime, int endTime) {
        // Add the event in the map
        mp[startTime] += 1;
        mp[endTime] -= 1;

        int sm = 0; // Calculates # of current bookings

        // Perform Line Sweep
        for(auto &[key, val] : mp){
            sm += val;
            /*
                If after the event, we find that there is a triple booking,
                then we revert the changes, and return false
            */
            if(sm == 3){
                mp[startTime]--;
                mp[endTime]++;
                return false;
            }
        }
        return true;
    }
};
```