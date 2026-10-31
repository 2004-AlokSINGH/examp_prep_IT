---
https://leetcode.com/problems/longest-substring-without-repeating-characters/

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {

        int lt = 0;
        int rt = 0;
        int ans = 0;

        Map<Character, Integer> chrMap = new HashMap<>();

        while (rt < s.length()) {

            // Add current character
            chrMap.put(
                s.charAt(rt),
                chrMap.getOrDefault(s.charAt(rt), 0) + 1
            );

            // Shrink window if duplicate exists
            while (chrMap.get(s.charAt(rt)) > 1) {
                chrMap.put(
                    s.charAt(lt),
                    chrMap.get(s.charAt(lt)) - 1
                );
                lt++;
            }

            // Update answer
            ans = Math.max(ans, rt - lt + 1);

            rt++;
        }

        return ans;
    }
}

```

---

---
https://leetcode.com/problems/longest-repeating-character-replacement/submissions/2186130114/

```py
class Solution(object):
    def characterReplacement(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        lt=rt=0
        d={}
        ans=0
        maxf=0
        while  rt<len(s):
            d[s[rt]]=d.get(s[rt],0)+1
            maxf=max(maxf,d[s[rt]])

            while (rt-lt+1)-maxf>k:
                d[s[lt]]-=1
                lt+=1
            ans=max(ans,rt-lt+1)

            rt+=1
        return ans
```
---

---
https://leetcode.com/problems/fizz-buzz/description/

```py
class Solution(object):
    def fizzBuzz(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        ans=[]
        for i in range(1,n+1):
            if i%3==0 and i%5==0:
                ans.append("FizzBuzz")
            elif i%3==0:
                ans.append("Fizz")
            elif i%5==0:
                ans.append("Buzz")
            else:
                ans.append(str(i))
        return ans
```


https://leetcode.com/problems/valid-anagram/description/

```py
class Solution(object):

    def isAnagram(self, s, t):

        if len(s) != len(t):
            return False

        freq = {}

        for ch in s:
            freq[ch] = freq.get(ch, 0) + 1

        for ch in t:
            if ch not in freq:
                return False

            freq[ch] -= 1

            if freq[ch] < 0:
                return False

        return True
```
---


