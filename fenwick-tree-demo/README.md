# 树状数组

We have an array arr[0 . . . n-1]. We would like to
1） Compute the sum of the first i elements.
2） Modify the value of a specified element of the array arr[i] = x where 0 <= i <= n-1.

BITree[0] is a dummy node.
BITree[y] is the parent of BITree[x], if and only if y can be obtained by removing the last set bit from the binary representation of x, that is y = x – (x & (-x)).

## lowbit(index)

lowbit(index)=index & (-index)

parent(i)=i-lowbit(i) and parent(i)=i+lowbit(i)
这两个操作都是在不断提高结果中尾部1的位置，即k的值。
区别是第一个操作是跳跃式，循环次数取决于i的二进制表示中的1的个数，
第二个操作是逐步提升的，循环次数取决于i的二进制表示中的高位0的个数。

## the parent of BITree[index]

The parent can be obtained by removing the last set bit from the current index, i.e., index = index – (index & (-index))

## getSum(x): Returns the sum of the sub-array arr[0,…,x]

// Returns the sum of the sub-array arr[0,…,x] using BITree[0..n], which is constructed from arr[0..n-1] 


BITree[0..n]
arr[0..n-1]

···
int query(int x)
{
int sum = 0;
for(; x > 0; x -= x&-x)
sum += BIT[x];
return sum;
}
···

## update(x, val): Updates the Binary Indexed Tree (BIT) by performing arr[index] += val

// Note that the update(x, val) operation will not change arr[]. 

index = index + (index & (-index))

The update function needs to make sure that all the BITree nodes which contain arr[i] within their ranges being updated.

···
void update(int x, int delta)
{
for(; x <= n; x += x&-x)
BIT[x] += delta;
}
···

x += x&-x 究竟是什么含义？这是一个不断让k+1的操作，即将尾部为1的操作逐渐向左移动，直到溢出。操作的次数取决于i的二进制中尾部1的高位0的个数。

## 个人学习

1) 根节点是值为0的假节点 BITree[0]=0
2) 根节点的直接子节点是2的整数幂整数。这些子节点i直接存的就是sum(1, i)。除了最后一个直接子节点，每个直接子节点都有i-1个子节点。这些i-1个子节点是[i+1, 2*i-1]。
3) 所有的奇数节点是叶子节点。这些子节点i直接存的就是arr[i]。当n为偶数时，将是整棵树中唯一不是奇书的叶子节点。
4) 一个整数i的二进制表示形式里有多少个1，正好表示该节点到根节点的深度，也表示将[1, i]这i个整数分成几组。
5) 一个数i的lowbit(i)=2^k, 表示这个整数和它的父节点间的连续整数个数为2^k。即BITree[i]=arr[i..i-2^k+1]=arr[i] + arr[i-1] + ... + arr[i-2^k+1]。
6) ** BITree[i]是第一个包含arr[i]的节点。这个对应的是Update操作，似乎也是树状数组中最难理解的一段。

## 个人学习 （2）

树状数组的节点i的二进制编码的尾部0特征：
1) 尾部1的位置k决定了这个节点可以有多少个子节点（2^k-1）。节点i和它所有的子节点的二进制形式的共同部分是位置k及比位置k高的所有位。节点i的尾部有多少个0,决定了这个节点的变化潜力。
2) 以节点8来举例子, 1000。尾部有3个0，既表示他会有3个直接子节点，也表示他还可以往下延申3层。节点8的下一层必然是包含两个1，由他的3个尾部0中的某一个变成1得到。从小到大：1001,1010,1100。再往下一层的节点必须包括3个1。由于这个1必须由尾部0变化的来, 1001已经变不出来了; 1010可以变出1011; 1100 可以变出 1101, 1110。最后一层必须包括4个1, 由1110变出111得到。
3) 以上视角是逐层来展开的，算是广度优先。也可以用深度优先来理解这个过程。以节点8来举例子, 1000。尾部有3个0，当3个0全部填充成1，完成填充操作。[1001],[1010,1011],[1100,1101,[1110,1111]]。

树状数组的节点i的二进制编码的高位0特征：
1) 节点高位有几个0，就意味着还有几次尾部1进位的空间。如果没有高位0，尾部1一进位就溢出到下一个更大的幂节点了。
2) 节点高位0的意思是在这个节点i的右侧（树的右侧，而不是i的子节点），还有多少种高度相同或更小的路径可以到达幂节点（根节点0的直接子节点）。
3) x += x&-x 的移动路径就很清晰了。a. 优先在该节点的右侧兄弟节点同层移动。b. 移动到同层最右侧之后，下一个位置对父节点执行step a操作。重复操作，直到遇到幂节点，根据树状数组的大小n来终止。
4) 再理解为什么x += x&-x跳到的节点也存储了arr[i]。a. 在右侧兄弟节点同层移动这个操作好理解，BITree[i]=arr[i-2^k+1..i],就是从父节点到自己的所有整数。同层就是同一个父节点，右侧节点肯定比右侧节点的数值大。既右侧节点的存储内容包含了左侧节点的保存范围。b. 那跳到了父节点的右侧兄弟节点这个操作怎么理解？也好理解，新的位置覆盖的范围是(父节点的父节点,父节点的右侧兄弟节点], 显然这个范围覆盖了arr[i]，因为i必然大于父节点的父节点，i必然小于在其右侧的树节点。
