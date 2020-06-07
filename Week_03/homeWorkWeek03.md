HomeWork:

1：二叉树的最近公共祖先：

class Solution {
   private TreeNode ans;

    public Solution() {
        this.ans = null;
    }

    private boolean dfs(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return false;
        boolean lson = dfs(root.left, p, q);
        boolean rson = dfs(root.right, p, q);
        if ((lson && rson) || ((root.val == p.val || root.val == q.val) && (lson || rson))) {
            ans = root;
        } 
        return lson || rson || (root.val == p.val || root.val == q.val);
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        this.dfs(root, p, q);
        return this.ans;
    }

}



2：从前序与中序遍历序列构造二叉树。（看了很多大神的其他解法，真的很精妙，但是目前理解起来有些困难）

class Solution {
    private Map<Integer, Integer> indexMap;

    public TreeNode myBuildTree(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
        if (preorder_left > preorder_right) {
            return null;
        }

        // 前序遍历中的第一个节点就是根节点
        int preorder_root = preorder_left;
        // 在中序遍历中定位根节点
        int inorder_root = indexMap.get(preorder[preorder_root]);
        
        // 先把根节点建立出来
        TreeNode root = new TreeNode(preorder[preorder_root]);
        // 得到左子树中的节点数目
        int size_left_subtree = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root.left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
        root.right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        return root;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        // 构造哈希映射，帮助我们快速定位根节点
        indexMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < n; i++) {
            indexMap.put(inorder[i], i);
        }
        return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }

}

3：组合 （抄了一个比较精妙的答案0.0）
class Solution {
   private ArrayList<List<Integer>> res;
    // 求解C(n,k), 当前已经找到的组合存储在c中, 需要从start开始搜索新的元素
    private void generateCombinations(int n, int k, int start, List<Integer> list) {
        if (list.size() == k) {
            res.add(new ArrayList<>(list));
            return;
        }
        //减枝  i <= n-(k-list.size())+1
        for (int i = start; i <= n - (k - list.size()) + 1; i++) {
            list.add(i);
            generateCombinations(n, k, i + 1, list);
            list.remove(list.size() - 1);

        }
    }

    public List<List<Integer>> combine(int n, int k) {

        res = new ArrayList<>();
        if (n <= 0 || k <= 0 || k > n) {
            return res;
        }
        List<Integer> list = new ArrayList<>();
        generateCombinations(n, k, 1, list);

        return res;

    }

}

4：全排列

class Solution {
    public void backtrack(int n,
                        ArrayList<Integer> output,
                        List<List<Integer>> res,
                        int first) {
    // 所有数都填完了
    if (first == n)
      res.add(new ArrayList<Integer>(output));
    for (int i = first; i < n; i++) {
      // 动态维护数组
      Collections.swap(output, first, i);
      // 继续递归填下一个数
      backtrack(n, output, res, first + 1);
      // 撤销操作
      Collections.swap(output, first, i);
    }
  }

  public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new LinkedList();

    ArrayList<Integer> output = new ArrayList<Integer>();
    for (int num : nums)
      output.add(num);

    int n = nums.length;
    backtrack(n, output, res, 0);
    return res;
  }

}
5: 全排列Ⅱ

class Solution {

List<List<Integer>> res;
 
	public List<List<Integer>> permuteUnique(int[] nums) {
		res = new ArrayList<List<Integer>>();
		if (nums == null || nums.length < 1)
			return res;
		Arrays.sort(nums);
		ArrayList<Integer> list = new ArrayList<Integer>();
		boolean[] used = new boolean[nums.length];
		solve(list, nums, used);
 
		return res;
	}
 
	private void solve(ArrayList<Integer> list, int[] nums, boolean[] used) {
		if (list.size() == nums.length) {
			res.add(new ArrayList<Integer>(list));
			return;
		}
 
		for (int i = 0; i < nums.length; i++) {
			if (used[i])
				continue;
			/*
			 * 只需要判断i和i-1(而不需要判断i与i-2...) 相同的元素一定是相邻的。
			 * 如果i-2，i-1，i相同，那么在上一轮循环就已经判断了i-1,i，本轮循环不需要重复判断
			 */
			if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1])
				continue;
			list.add(nums[i]);
			used[i]=true;
			solve(list, nums, used);
			used[i]=false;
			list.remove(list.size()-1);
		}
 
	}
}
