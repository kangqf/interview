## DFS 与 BFS
DFS：优先考虑深度，只有深度到了极限才会回溯考虑广度。
BFS：优先考虑广度，只有广度到了极限才会回溯考虑深度。

### DFS 模板
``` cpp
class Node;

bool dfs(Node nodes, int depth)
{
    // 路径长度满足要求，表示此次搜索有解
    if (depth == 4)
    {
        return true;
    }

    // 遍历跟节点n相邻的节点nextNode
    for( ; nodes.nextNodes; nodes = nodes.nextNodes)
    {
        // 例如搜索到V1了，那么V1要设置成已访问
        if(!visited[n])
        {
            visited[n] == true;
            // 如果搜索出有解，
            // 你必须一层一层递归的告诉上层已经找到解
            if(dfs(n, depth+1))
                return true;
            // 重新设置成未访问，因为它有可能出现在下一次搜索的路径中
            visited[n] == false;
        }
    }

    // 本次搜索无解
    return false;
}
```