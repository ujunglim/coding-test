# 문제 링크

광물 캐기

# 1. 내 풀이

P: 갖고 있는 곡괭이의 개수 배열 picks([다이아, 철,돌]), 광물들의 순서를 담은 문자열 배열 minerals
R: 최소한의 피로도
E:
P:
- 다이아-철-돌 순으로 쓰는게 좋음

```javascript
    function solution(picks, minerals) {
      let answer;
    
      const fatigueTable = {
        diamond: [1, 5, 25],
        iron: [1, 1, 5],
        stone: [1, 1, 1],
      };
    
      const len = minerals.length;
      const availableTools = [...picks];
    
      const dfs = (idx, tired) => {
      const availableToolCnt = availableTools.reduce((a, b) => a + b, 0);
      if (len <= idx || 0 >= availableToolCnt) {
        answer = Math.min(answer, tired);
        return;
      }
    
      const max = len < idx + 5 ? len : idx + 5;
    
      for (let k = 0; k < 3; k++) {
        if (availableTools[k] <= 0) continue;
    
        let needTired = 0;
        for (let i = idx; i < max; i++) {
            needTired += fatigueTable[minerals[i]][k];
        }
    
        availableTools[k] -= 1;
        dfs(max, tired + needTired);
        availableTools[k] += 1;
      }
  };

    dfs(0, 0);
    return answer;
  }
```

# 2. 느낀 점

- 깔끔하고 재미있는 문제였습니다!

# 3. 배운 점