# 문제 링크

석유 시추

# 1. 내 풀이

```javascript
function solution(land) {
    let max = 0; // 답

    const n = land.length;    // 세로
    const m = land[0].length; // 가로

    // 1번 부터 시작해 만나는 석유 덩어리에 번호를 매긴다.
    let oil_index = 1;

    // 2차원 배열 visited를 만들어 석유 덩어리 번호를 저장
    const visited = new Array(n).fill().map(_ => new Array(m).fill(0));
    
    // 덩어리 번호와 해당 석유 덩어리의 사이즈를 저장하는 map
    const oilMap = new Map();

    const dx = [-1, 0, 1, 0];
    const dy = [0, 1, 0, -1];

    // 붙어있는 석유를 다 확인하는 BFS
    function bfs(init_x, init_y) {
        let oil = 0;
        const queue = [{x:init_x, y:init_y}];

        // 석유 덩어리 번호 저장
        visited[init_x][init_y] = oil_index;

        while(queue.length) {
            const coord = queue.shift();
            let x = coord.x;
            let y = coord.y;

            if (land[x][y] === 1) {
                oil++;
            }
            
            // 상하좌우 탐색
            for (let i=0; i<4; i++) {
                let nx = x + dx[i];
                let ny = y + dy[i];

                // 유효하지 않은 좌표일 경우 스킵
                if (nx < 0 || nx >= n || ny < 0 || ny >= m || visited[nx][ny]) 
                    continue;
                
                // 탐색중인 부분이 석유면 그 부분도 표시
                if (land[nx][ny] === 1) {
                    visited[nx][ny] = oil_index;
                    queue.push({x: nx, y: ny});
                }
            }
        }

        oilMap[oil_index++] = oil;
        return oil;
    }

    // 지도를 모두 확인하면서 석유 덩어리의 정보를 만들어 낸다.
    for (let i=0; i<n; i++) {
        for (let j=0; j<m; j++) {
            // 원소에 석유가 있고 아직 방문하지 않은 경우
            if (land[i][j] > 0 && visited[i][j] === 0) 
                bfs(i, j);
        }
    }

    for (let i=0; i<m; i++) {
        let sum = 0;
        // 같은 덩어리 중복 없도록 set 사용
        const set = new Set();
        for(let j=0; j<n; j++) {
            if (visited[j][i] !== 0) 
                set.add(visited[j][i]);
        }
        // 모인 덩어리들의 석유량 더하기
        set.forEach(item => {
            sum += oilMap[item];
        })

        max = Math.max(sum, max);
    }
    return max;
}
```

## 풀이 방법
P: 석유가 묻힌 땅과 석유덩어리를 나타내는 2차원 배열 land  
R: 시추관 하나(컬럼)을 설치해 뽑을 수 있는 가장 많은 석유량  
E:  
P:

1. 석유 덩어리별 번호를 담는 2차원 배열 visited를 만들고
2. 덩어리 번호와 해당 석유 덩어리의 사이즈를 저장하는 oilMap 만듦
3. BFS 사용해 붙어있는 석유를 모두 확인

# 2. 느낀 점


# 3. 배운 점

## 스터디에서 배운 내용

보통 dfs는 한 길로 가서 끝까지 탐색하는 경우에서 하시는 편!
그런데 위 문제는 한 곳으로 가서 상하좌우를 스택에 쌓고 확장하는 문제라 bfs가 더 적절해보였다