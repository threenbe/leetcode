# Word Search II

Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

# My solution

```Java
class Solution {
    class TrieNode {
        HashMap<Character, TrieNode> children = new HashMap<Character, TrieNode>();
        char ch;
        String word = null;//If this is set, then this node marks the end of a word
        
        public TrieNode() {}
        public TrieNode(Character c) {
            this.ch = c;
        }
        
        @Override
        public String toString(){
            return "value="+ch;
        }
    }
    public List<String> findWords(char[][] board, String[] words) {
        TrieNode root = new TrieNode();
        
        constructTrie(root, words);
        
        //now traverse the board and see if we can find words in the trie
        List<String> wordsFound = new ArrayList<String>();
        int numRows = board.length;
        int numCols = board[0].length;
        for (int r = 0; r < numRows; r++) {
            for (int c = 0; c < numCols; c++) {
                if (root.children.containsKey(board[r][c])) {
                    traverse(board, r, c, root, wordsFound);
                }
                
            }
        }
        
        return wordsFound;
    }
    
    private void traverse(char[][] board, int row, int col, TrieNode root, List<String> wordsFound) {
        
        char currentChar = board[row][col];
        TrieNode childNode = root.children.get(currentChar);
        
        if (childNode != null && childNode.word != null) {
            wordsFound.add(childNode.word);
            childNode.word = null;
        }
        
        board[row][col] = '#'; //mark as visited
        
        int numRows = board.length;
        int numCols = board[0].length;
        
        int[] rowOffset = {-1, 1, 0, 0};
        int[] colOffset = {0, 0, -1, 1};
        
        
        if (childNode != null) {
            for (int i = 0; i < 4; i++) {
                int nextRow = row + rowOffset[i];
                int nextCol = col + colOffset[i];
                if (nextRow >= 0 && nextRow < numRows
                   && nextCol >= 0 && nextCol < numCols
                   && childNode.children.containsKey(board[nextRow][nextCol])) {
                    traverse(board, nextRow, nextCol, childNode, wordsFound);
                }
            }
        }
        
        board[row][col] = currentChar;
        
    }
    
    private void constructTrie(TrieNode node, String[] words) {
        for (String word : words) {
            TrieNode current = node;
            for (Character c : word.toCharArray()) {
                if (current.children.containsKey(c)) {
                    current = current.children.get(c);
                }
                else {
                    TrieNode newNode = new TrieNode(c);
                    current.children.put(c, newNode);
                    current = newNode;
                }
            }
            current.word = word;
        }
    }
}
```
