# Word Ladder

A transformation sequence from word beginWord to word endWord using a dictionary wordList is a sequence of words beginWord -> s1 -> s2 -> ... -> sk such that:

* Every adjacent pair of words differs by a single letter.
* Every si for 1 <= i <= k is in wordList. Note that beginWord does not need to be in wordList.
* sk == endWord
* 
Given two words, beginWord and endWord, and a dictionary wordList, return the number of words in the shortest transformation sequence from beginWord to endWord, or 0 if no such sequence exists.

## My solution:

```Java
class Solution {
    Map<String, List<String>> transformationMappings = new HashMap<String, List<String>>();
    
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        //we want the shortest transformation sequence from beginWord to endWord
        //each word's neighbor differs by one letter
        //BFS will give us the shortest path
        //in BFS, you pop a node from the queue and then add all of its neighbors to said queue
        initializeMappings(beginWord.length(), wordList);
        Queue<String> queue = new LinkedList<String>();
        Set<String> visited = new HashSet<String>();
        queue.add(beginWord);
        visited.add(beginWord);
        int level = 0;
        while(!queue.isEmpty()) {
            int size = queue.size();
            level++;
            //generally this for loop isn't necessary,
            //but we have it here so that we can more easily
            //count the level that we're at in our graph.
            //basically, if the queue's size is A when we begin
            //processing level B, then we want to process all A
            //nodes at level B before moving on to level B+1.
            //in a regular BFS implementation where we aren't counting
            //the levels, this shouldn't matter since everything goes into
            //the queue in the same order regardless, but here it does matter.
            for (int i = 0; i < size; i++) {
                String currentWord = queue.poll();
                if (currentWord.equals(endWord)) {
                    //end of transformation sequence
                    return level;
                }
                //add current word's neighbors to the queue
                List<String> neighbors = getNeighbors(currentWord, wordList);
                for (String s : neighbors) {
                    if (!visited.contains(s)) {
                        visited.add(s);
                        queue.add(s);
                    }
                }
            }
        }
        return 0;
    }
    
    private List<String> getNeighbors(String word, List<String> wordList) {
        List<String> neighbors = new ArrayList<String>();
        for (int i = 0; i < word.length(); i++) {
            String neighbor = word.substring(0, i) + '*' + word.substring(i+1, word.length());
            List<String> neighborsForThisTransformation = transformationMappings.get(neighbor);
            if (neighborsForThisTransformation != null) {
                neighbors.addAll(neighborsForThisTransformation);
            }
        }
        return neighbors;
    }
    
    private void initializeMappings(int wordLength, List<String> wordList) {
        for (String word : wordList) {
            for (int i = 0; i < wordLength; i++) {
                String neighbor = word.substring(0, i) + '*' + word.substring(i+1, wordLength);
                List<String> transformationList = transformationMappings.getOrDefault(neighbor, new ArrayList<String>());
                transformationList.add(word);
                transformationMappings.put(neighbor, transformationList);
            }
        }
    }
}
```
