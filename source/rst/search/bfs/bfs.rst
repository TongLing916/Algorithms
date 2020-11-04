############################
广度优先搜索
############################

知识点
======

例题
====

`Leetcode 102. 二叉树的层序遍历 <https://leetcode-cn.com/problems/binary-tree-level-order-traversal/>`_

.. code-block:: c++
    class Solution {
     public:
      vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) {
          return res;
        }

        queue<TreeNode*> nodes;
        nodes.push(root);
        while (nodes.size()) {
          const int breadth = nodes.size();
          vector<int> level;
          level.reserve(breadth);
          for (int i = 0; i < breadth; ++i) {
            TreeNode* const node = nodes.front();
            nodes.pop();

            level.emplace_back(node->val);
            if (node->left) {
              nodes.push(node->left);
            }
            if (node->right) {
              nodes.push(node->right);
            }
          }
          res.emplace_back(level);
        }

        return res;
      }
    };

`Leetcode 103. 二叉树的锯齿形层次遍历 <https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/>`_

.. code-block:: c++
