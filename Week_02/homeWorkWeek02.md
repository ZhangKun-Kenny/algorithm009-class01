HomeWork:

1. HashMap 还在看源码，需要进一步了解吧，总结之后会补上。

2. 有效的字母异位词

class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()){
            return false;
        }

        char[] str1 = s.toCharArray();
        char[] str2 = t.toCharArray();

        Arrays.sort(str2);
        Arrays.sort(str1);

        return Arrays.equals(str1,str2); 
    }
}


3. 两数之和

//学习了用map的方法处理
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // int[] newArr = new int[2];
        // int i , j;
        // for(i = 0; i < nums.length ; i++){
        //     for(j = i + 1; j < nums.length; j++){
        //         if(nums[i] + nums[j] == target){
        //             newArr[0] = i;
        //             newArr[1] = j;
        //         }
        //     }
        // }
        // return newArr;
        Map<Integer,Integer> map = new HashMap<>();

        for (int i = 0 ; i < nums.length; i ++){
            int complement = target - nums[i];

            if (map.containsKey(complement)){
                return new int[] {map.get(complement) , i};
            }

            map.put(nums[i] , i);
        }

        throw new IllegalArgumentException("NO TWO NUMS SOLUTION");
        
    }
}

4. N叉树的前序遍历

class Solution {
    public List<Integer> preorder(Node root) {
    List<Integer> res=new ArrayList<Integer>();
    if(root==null) return res;
    res.add(root.val);
        for(Node r:root.children){
            res.addAll(preorder(r));
        };
        return res;
    }
}

5. 字母异位词分组
class Solution {

    List<List<String>> ans = new ArrayList<>() ;
    HashMap<String,List<String>> map = new HashMap<>() ;
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs == null || strs.length == 0) return ans ;
        for(String s:strs){
            String temp = getString(s) ;
            if(!map.containsKey(temp)){
                map.put(temp,new ArrayList<>()) ;
            }
            map.get(temp).add(s) ;
        }
        for(Map.Entry<String,List<String>> entry:map.entrySet()){
            ans.add(entry.getValue()) ;
        }
        return ans ;
    }

     public String getString(String s){
        char[] ss = s.toCharArray() ;
        Arrays.sort(ss) ;
        return String.valueOf(ss) ;
    }
}

6. 二叉树的中序遍历
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        while(root!=null||!stack.isEmpty()){
            while(root!=null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            list.add(root.val);
            root = root.right;
        }
        return list;
    }
}

7. 二叉树的前序遍历
class Solution {
    List<Integer> lists;
    public Solution(){
         lists=new ArrayList<>();
    }
    public List<Integer> preorderTraversal(TreeNode root) {
         //根左右
         if(root==null) return lists;
         lists.add(root.val);
        preorderTraversal(root.left);
          preorderTraversal(root.right);
         return lists;
    }
}

8. N叉树的层序遍历

class Solution {
private List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> levelOrder(Node root) {
        if (root != null) traverseNode(root, 0);
        return result;
    }

    private void traverseNode(Node node, int level) {
        if (result.size() <= level) {
            result.add(new ArrayList<>());
        }
        result.get(level).add(node.val);
        for (Node child : node.children) {
            traverseNode(child, level + 1);
        }
    }
}


9. 丑数

class Solution {
    public int nthUglyNumber(int n) {
        int a=0,b=0,c=0;
        int[] res = new int[n];
        res[0]=1;
        for(int i=1;i<n;i++){
            res[i]=Math.min(Math.min(res[a]*2,res[b]*3),res[c]*5);
            if(res[i]==res[a]*2) a++;
            if(res[i]==res[b]*3) b++;
            if(res[i]==res[c]*5) c++;
        }
        return res[n-1];
    }
}

10. 前 K 个高频元素

public class TopKFre {
    public List<Integer> topKFrequent(int[] nums, int k) {
        // 使用字典，统计每个元素出现的次数，元素为键，元素出现的次数为值
        HashMap<Integer,Integer> map = new HashMap();
        for(int num : nums){
            if (map.containsKey(num)) {
                map.put(num, map.get(num) + 1);
            } else {
                map.put(num, 1);
            }
        }
        // 遍历map，用最小堆保存频率最大的k个元素
        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                return map.get(a) - map.get(b);
            }
        });
        for (Integer key : map.keySet()) {
            if (pq.size() < k) {
                pq.add(key);
            } else if (map.get(key) > map.get(pq.peek())) {
                pq.remove();
                pq.add(key);
            }
        }
        // 取出最小堆中的元素
        List<Integer> res = new ArrayList<>();
        while (!pq.isEmpty()) {
            res.add(pq.remove());
        }
        return res;
    }

    public static void main(String[] args) {
        TopKFre topKFre = new TopKFre();
        int[] array = {1,2,3,4,1,1,3,3,4,1,2,2,4,4,5,6,3,67,4,3,2,5 };
        System.out.println(topKFre.topKFrequent(array,4));


    }
}