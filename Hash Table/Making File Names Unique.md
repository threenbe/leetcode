# Making File Names Unique

Given an array of strings names of size n. You will create n folders in your file system such that, at the ith minute, you will create a folder with the name names[i].

Since two files cannot have the same name, if you enter a folder name that was previously used, the system will have a suffix addition to its name in the form of (k), where, k is the smallest positive integer such that the obtained name remains unique.

Return an array of strings of length n where ans[i] is the actual name the system will assign to the ith folder when you create it.

https://leetcode.com/problems/making-file-names-unique/

## My solution:

```Java
class Solution {
    public String[] getFolderNames(String[] names) {
        String[] result = new String[names.length];
        int resultIndex = 0;
        // key -> value : base file name -> number of duplicate files with that base file name
        Map<String, Integer> fileNames = new HashMap<>();
        
        for (String name : names) {
            if (!fileNames.containsKey(name)) {
                fileNames.put(name, 0);
                result[resultIndex++] = name;
            }
            else {
                int numDuplicates = fileNames.get(name) + 1;
                String newName = name + "(" + numDuplicates + ")";
                while (fileNames.containsKey(newName)) {
                    numDuplicates++;
                    newName = name + "(" + numDuplicates + ")";
                }
                fileNames.put(name, numDuplicates);
                //If the system takes a file with a base name of,
                //say, "wano" and it's the 5th duplicate, which gets
                //stored as "wano(5)", then a subsequent attempt by the
                //user to create a file named "wano(5)" will use "wano(5)"
                //as its base instead of "wano," and thus get stored as
                //"wano(5)(1)" instead of "wano(6)."
                fileNames.put(newName, 0);
                result[resultIndex++] = newName;
            }
        }
        return result;
    }
}
```
