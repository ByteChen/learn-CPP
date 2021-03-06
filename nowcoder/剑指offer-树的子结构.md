## [树的子结构](https://www.nowcoder.com/questionTerminal/6e196c44c7004d15b1610b9afca8bd88)
输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

## 代码
```C++
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        
        if(pRoot1 == NULL || pRoot2 == NULL)
            return false;
        //find same root->val
        queue<TreeNode*> q;
        q.push(pRoot1);
        TreeNode* temp;
        while(!q.empty()) //这部分实际上直接递归遍历树就好，更简洁
        {
            temp = q.front();
            if(compare(temp, pRoot2))
                return true;
            if(temp->left)
                q.push(temp->left);
            if(temp->right)
                q.push(temp->right);
            q.pop();
        }
        return false;
    }
    
    bool compare(TreeNode* a, TreeNode* b)
    {
        if(a == NULL)
            return false;
        if(a->val != b->val)
            return false;
        bool ifLeftSame = true, ifRightSame = true;
        if(b->left)
            ifLeftSame = compare(a->left, b->left);
        if(b->right)
            ifRightSame = compare(a->right, b->right);
        if(ifLeftSame && ifRightSame)
            return true;
        return false;
    }
};
```

* 20180401 补充递归遍历树的方法，更加简洁
```C++
//递归
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        bool result = false;
        if(pRoot1 && pRoot2) {
            if(pRoot1->val == pRoot2->val)
                result = compare(pRoot1, pRoot2);	////compare()仍然使用上面写的
            if(!result)					//若没找到。若找到了的话就不再继续往下找了，节省时间
                result = HasSubtree(pRoot1->left, pRoot2);
            if(!result)
                result = HasSubtree(pRoot1->right, pRoot2);
        }
        return result;
    }
```
