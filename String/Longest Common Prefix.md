# Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

**Note:** All given inputs are in lowercase letters a-z.

## My solution:

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string ret = "";
        char curr;
        int curr_idx = 0;
        if (strs.size() == 1) return strs[0];
        for(int i = 0; i < strs.size(); i++){
            //cout << i << endl;
            if (i == 0){
                if (curr_idx >= strs[i].size()){
                    return ret;
                } else {
                    //cout << strs[i][curr_idx] << endl;
                    curr = strs[i][curr_idx];
                }
            }
            else if (i == strs.size()-1){
                if (curr_idx >= strs[i].size()){
                    return ret;
                } else {
                    //cout << strs[i][curr_idx] << endl;
                    if (curr == strs[i][curr_idx]){
                        ret += curr;
                        curr_idx++;
                        i = -1;
                    }
                }
            }
            else {
                if (curr_idx >= strs[i].size()){
                    return ret;
                } else {
                    //cout << strs[i][curr_idx] << endl;
                    if (curr != strs[i][curr_idx]){
                        return ret;
                    }
                }
            }
        }
        return ret;
    }
};
```
