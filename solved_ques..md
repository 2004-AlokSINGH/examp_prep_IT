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


---

https://leetcode.com/problems/valid-parentheses/
```py
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        d={ ')':'(','}':'{',']':'[' }

        stk=[]
        opn=['(','{','[']
        opn=set(opn)

        i=0
        while i<len(s):
            if s[i] in opn:
                stk.append(s[i])
            else:                
                if len(stk)>0 and stk[-1]!=d[s[i]]:
                    print(stk , i)
                    return False
                elif len(stk)==0:
                    return False
                else:
                    stk.pop()
            i+=1
        return True if len(stk)==0 else False
```

```py
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: int
        """
        freq={}
        for c in s:
            freq[c]= freq.get(c,0)+1

        for i,c in enumerate(s):
            if freq[c]==1:
                return i
        return -1

```
**IMP--**

https://leetcode.com/problems/move-zeroes/
```py


```
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        lt=0
        n=len(nums)
        # first move all non zero to left side
        # then add remaining 0
        i=0
        while i<n:
            if nums[i]==0:
                i+=1
                continue
            elif nums[i]!=0:
                nums[lt],nums[i]= nums[i],nums[lt]
                lt+=1
            i+=1
        

---

