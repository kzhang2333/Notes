## Shortest Word Edit Path

Given two words `source` and `target`, and a list of words `words`, find the length of the shortest series of edits that transforms `source` to `target`.

Each edit must change exactly one letter at a time, and each intermediate word (and the final `target` word) must exist in `words`.

If the task is impossible, return `-1`.

```
hit: [hot]
        hot: [dot, lot] 
        dot: [dog, lot]
        lot: [log]
        log: [cog]
        hit -- hot -- lot -- log -- cog = 5
        
        hit:
            ait
            bit 
            cit
            ...
            zit
            hat
            hbt
            ...
            hzt
            hia
            ..
            hiz
            
        */
```

## Code

```java
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        // find neighbors for each letter
        Set<String> set = new HashSet<>();
        for (String word : wordList) set.add(word);
        Map<String, List<String>> neighbors = new HashMap<>();
        generateNeighbor(beginWord, neighbors, set);
        for (String word : wordList) generateNeighbor(word, neighbors, set);
        System.out.println(neighbors.toString());
        
        // bfs
        Deque<String> queue = new LinkedList<>();
        Set<String> visited = new HashSet<String>();
        queue.offer(beginWord);
        visited.add(beginWord);
        int level = 1;
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                String curWord = queue.poll();
                for (String word : neighbors.get(curWord)) {
                    if (word.equals(endWord)) return level +1 ;
                    if (!visited.contains(word)) { 
                        queue.offer(word);
                        visited.add(word);
                    }
                }
            }
            level++;
        }
        return 0;
    }
    
    void generateNeighbor(String word, Map<String, List<String>> map, Set<String> set) {
        char[] arr = word.toCharArray();
        List<String> neighbors = new ArrayList<>();
        for (int i = 0; i < arr.length; i++) {
            char temp = arr[i];
            for (char c = 'a'; c <= 'z'; c++) {
                if (temp == c) continue;
                arr[i] = c;
                String str = new String(arr);
                if (set.contains(str)) neighbors.add(str);
            }
            arr[i] = temp;
        }
        map.put(word, neighbors);
    }
```



## Hints

- What algorithms might be useful for finding a shortest distance?
- We should use a breadth-first search.
- Typically in a breadth first search, there is some graph, which for each node has some number of neighbors. How might you find which words in the word list are neighbors?
- There are two approaches to finding neighbors of a word.
  - One approach is for each letter in the original word, change it and check if that word is in the list of words.
  - Another approach is for each word in the word list, to check if exactly one letter is different between it and the original word.

## Time & Space complexity

N is number of word, M is max length in words

Time: We did 2 tasks, 

1. build graph(find neighbors): for each word, we change each letter from a --> z, 26 * n * m, because we used String contructor ( copy char array to String), which cost O(m)
2. bfs: iterate each node, O(n)

Space: we need to store all neighbors for each word, in worst case, a word can be neighbor of all other words, so space complexity is O(n*m)

