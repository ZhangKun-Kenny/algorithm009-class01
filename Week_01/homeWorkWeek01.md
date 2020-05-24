删除排序数组中的重复项:

class Solution {
    public int removeDuplicates(int[] nums) {

       if(nums.length == 0) {return 0;} 
       
       int len = 0;
	   //
       for(int i = 1; i < nums.length; i++){
           if(nums[i] != nums[len]){
               len +=1;
               nums[len] = nums[i];              
           }
       }
       return len+1;
    }
}


旋转数组：
class Solution {
    public void rotate(int[] nums, int k) {
    
        int n = nums.length;
		// 考虑 k比n大
		k %= n;
		if (k == 0 || n <= 1) {
			// 不变
			return;
		}
		reverse(nums, 0, n - 1);
		reverse(nums, 0, k - 1);
		reverse(nums, k, n - 1);
    }
    
    public void reverse(int[] nums, int start, int end) {
		while (start < end) {
			int temp = nums[start];
			nums[start++] = nums[end];
			nums[end--] = temp;
		}
	}
}


合并两个有序链表

class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);

        //创建并维护一个prev指针
        ListNode prev = prehead;

        while (l1 != null && l2 != null) {

            //若两个链表都不为空，判断各个链表节点的值大小，并且使得prev.next指向较小的节点
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }

            //上面得到两个链表较小的值后，prev往后移一位，继续下面的比较
            prev = prev.next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;

    }
}

合并两个有序数组
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        //归并排序的merge过程

        int i=m-1,j=n-1;
        int index=m+n-1;
        while(index>=0){
            //前两个判断要放在前面，防止空指针异常
            if(i<0){
                nums1[index--]=nums2[j--];
            }else if(j<0){
                nums1[index--]=nums1[i--];
            }
            else if(nums1[i]>nums2[j] && i>=0){
                nums1[index--]=nums1[i--];
            }else if(nums1[i]<=nums2[j]&& j>=0){
                nums1[index--]=nums2[j--];
            }
        }
    }

}

两数之和
class Solution {
    public int[] twoSum(int[] nums, int target) {

    int[] newArr = new int[2];
		int i ,j;
		for(i = 0; i <nums.length; i++) {
			for(j = i+1;j<nums.length;j++) {
				if(nums[i] + nums[j] == target) {
					
					newArr[0] = i;
					newArr[1] = j;
					//return newArr;
					
				}
			}
			
		}
		return newArr;
	}

}

移动零

class Solution {
    public void moveZeroes(int[] nums) {
        int j=0;
        for (int i=0;i<nums.length;++i){
            if (nums[i]!=0){
                nums[j]=nums[i];
                if (i!=j){
                nums[i]=0;
                }
                ++j;
            }
        }
    }
}

加一

class Solution {
    public int[] plusOne(int[] digits) {
        int len = digits.length;

        //先从整数最后一位判断，非9，直接+1，return即可
        //如果末位是9，末位赋值0，继续倒数第二位判断，同第一次一样判断，直到遇到+1，return 即可。
        for(int i = len - 1; i >= 0; i --){
            if(digits[i] != 9){
                digits[i]++;
                return digits;
            }

            digits[i] = 0;
        }

        // 如果输入的数组全部是数字9，新建一个数组，长度是digits.length + 1,首位为1，其他为0

        int[] newStr = new int[len + 1];
        newStr[0] = 1;

        return newStr;

    }
}

设计循环双端队列
class MyCircularDeque {

    private int[] elements;
    private int size;

    /** Initialize your data structure here. Set the size of the deque to be k. */
    public MyCircularDeque(int k) {
        elements = new int[k];
    }
    
    /** Adds an item at the front of Deque. Return true if the operation is successful. */
    public boolean insertFront(int value) {
        if(isFull()){
            return false;
        }
        for (int i =size -1;i>=0;i--){
            elements[i + 1] = elements[i];
        }

        elements[0] = value;
        size++;
        return true;
    }
    
    /** Adds an item at the rear of Deque. Return true if the operation is successful. */
    public boolean insertLast(int value) {
        if(isFull()){
            return false;
        }

        elements[size] = value;

        size++;

        return true;

    }
    
    /** Deletes an item from the front of Deque. Return true if the operation is successful. */
    public boolean deleteFront() {
        if(isEmpty()){
            return false;
        }

        elements[0] = 0;
        for(int i = 0; i < size -1; i++){
            elements[i] = elements[i+1];
        }

        size --;
        return true;
    }
    
    /** Deletes an item from the rear of Deque. Return true if the operation is successful. */
    public boolean deleteLast() {
        if(isEmpty()){
            return false;
        }

        elements[size - 1] = 0;
        size --;

        return true;
    }
    
    /** Get the front item from the deque. */
    public int getFront() {
        return size == 0 ? -1 : elements[0];
    }
    
    /** Get the last item from the deque. */
    public int getRear() {
        return size == 0 ? -1 : elements[size - 1];
    }
    
    /** Checks whether the circular deque is empty or not. */
    public boolean isEmpty() {
        return size == 0;
    }
    
    /** Checks whether the circular deque is full or not. */
    public boolean isFull() {
        return size == elements.length;
    }
}

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertLast(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleteLast();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
 
 接雨水（还有不理解的地方，需要课下多研究）
 
 class Solution {
    public int trap(int[] height) {

		int n = height.length;
		int result = 0;
		if(n == 0 || n == 1) {
			return result;
		}
		for (int i = 1; i < n - 1; i++) {
			int leftMax = 0;
			for (int j = 0; j < i; j++) {
				leftMax = Math.max(leftMax, height[j]);
			}
			int rightMax = 0;
			for (int j = i + 1; j < n; j++) {
				rightMax = Math.max(rightMax, height[j]);
			}
			int min = Math.min(leftMax, rightMax);
			if(min > height[i]) {
				result += min - height[i];
			}
		}
		return result;
	}

}