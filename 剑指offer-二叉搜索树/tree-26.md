1.题目

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

2.思路

根据二叉搜索树的特点：左结点的值<根结点的值<右结点的值，我们不难发现，使用二叉树的中序遍历出来的数据的数序，就是排序的顺序。
因此，首先，确定了二叉搜索树的遍历方法。
![image](https://github.com/zzh501937684/work/blob/master/%E5%89%91%E6%8C%87offer-%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91/pic02.jpg)

接下来，我们看下图，我们可以把树分成三个部分：值为10的结点、根结点为6的左子树、根结点为14的右子树。根据排序双向链表的定义，
值为10的结点将和它的左子树的最大一个结点链接起来，同时它还将和右子树最小的结点链接起来。
![image](https://github.com/zzh501937684/work/blob/master/%E5%89%91%E6%8C%87offer-%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91/pic01.jpg)

按照中序遍历的顺序，当我们遍历到根结点时，它的左子树已经转换成一个排序的好的双向链表了，并且处在链表中最后一个的结点是当前值最大的结点。
我们把值为8的结点和根结点链接起来，10就成了最后一个结点，接着我们就去遍历右子树，并把根结点和右子树中最小的结点链接起来。


3.c++
```c++
class Solution {
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        //用于记录双向链表尾结点
        TreeNode* pLastNodeInList = NULL;
        
        //开始转换结点
        ConvertNode(pRootOfTree, &pLastNodeInList);
        
        //pLastNodeInList指向双向链表的尾结点，我们需要重新返回头结点
        TreeNode* pHeadOfList = pLastNodeInList;
        while(pHeadOfList != NULL && pHeadOfList->left != NULL){
            pHeadOfList = pHeadOfList->left;
        }
        return pHeadOfList;
    }
    
    void ConvertNode(TreeNode* pNode, TreeNode** pLastNodeInList){
        //叶结点直接返回
        if(pNode == NULL){
            return;
        }
        TreeNode* pCurrent = pNode;
        //递归左子树
        if(pCurrent->left != NULL)
            ConvertNode(pCurrent->left, pLastNodeInList);
        
        //左指针
        pCurrent->left = *pLastNodeInList;
        //右指针
        if(*pLastNodeInList != NULL){
            (*pLastNodeInList)->right = pCurrent;
        }
        //更新双向链表尾结点
        *pLastNodeInList = pCurrent;
        //递归右子树
        if(pCurrent->right != NULL){
            ConvertNode(pCurrent->right, pLastNodeInList);
        }
    }
};
```
