HomeWork:

1：柠檬水找零

class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0, ten = 0;
        for (int bill : bills) {
        switch (bill) {
            case 5: five++; break;
            case 10: five--; ten++; break;
            case 20: {
                if (ten > 0) {
                    ten--; five--;
                } else {
                    five -= 3;
                }
                break;
            }
            default: break;
        }
        if (five < 0) {
            return false;
        }
    }
    return true;

    }
}


2：买卖股票的最佳时机 II

class Solution {
    public int maxProfit(int[] prices) {

        int res = 0;
        int len = prices.length;
        for (int i = 0; i < len - 1; i++) {
            int diff = prices[i + 1] - prices[i];
            if (diff > 0) {
                res += diff;
            }
        }
        return res;
    
    }
}

3：分发饼干

class Solution {
    public int findContentChildren(int[] g, int[] s) {
         //贪心的思想是，用尽量小的饼干去满足小需求的孩子，所以需要进行排序先
        int child = 0;
        int cookie = 0;
        Arrays.sort(g);  //先将饼干 和 孩子所需大小都进行排序
        Arrays.sort(s);
        while (child < g.length && cookie < s.length ){ //当其中一个遍历就结束
            if (g[child] <= s[cookie]){ //当用当前饼干可以满足当前孩子的需求，可以满足的孩子数量+1
                child++;
            }
            cookie++; // 饼干只可以用一次，因为饼干如果小的话，就是无法满足被抛弃，满足的话就是被用了
        }
        return child; 

    }
}
