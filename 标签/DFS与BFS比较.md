1 、BFS

空间是指数级别的，比较大

不会有爆栈的风险

最短最小



伪码

~~~C++
Q.push(head)
    while(!Q.empty()){
        temp = Q.front();
        Q.pop();
        if(temp为目标状态)
            输出
        if(temp不合法)
            continue;
        if(temp合法)
            Q.push(temp+#)//表示所有temp在搜索树中所有的子节点
    }
~~~



2、DFS

空间与深度成正比  比较小

有爆栈的风险，比如树的深度有10W层

不能搜最短最小



伪码





~~~C++
void dfs(状态A){

if(A不合法)  return;



if(A为目标状态) 输出；

if(A不为目标状态)  dfs(A+#) //递归调用



}
~~~



