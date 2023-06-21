# Array

List：~~27,26,80,189,41~~~~,299,134,~~118,119,169,229,274,275,243,244,245,217,219,220,55,45,121,122,123,188,309,11,42,334,128,164,287,135,330,321,327,289,57,56,252,253,239,295,53,325,209,238,152,228,163,88,75,283,376,280,324,278,35



## 27移除元素

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowindex = 0;
        for (int fastindex = 0; fastindex < nums.size(); fastindex++){
            if(nums[fastindex] != val){
                nums[slowindex++] = nums[fastindex];
            }
        }
        return slowindex;
    }
};
```

思路：用快慢指针，快指针进行是否==val的判断，慢指针用于实际更改和存储数组。



## 26删除有序数组中的重复项

给你一个 **升序排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slowIndex = 1, len = nums.size();
        if(len==0) return 0;
        for(int fastIndex = 1; fastIndex < len; fastIndex++){
            if(nums[fastIndex] != nums[fastIndex-1]){
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

思路：和27类似，用快慢指针，快指针进行是否和前一项相等的判断，慢指针用于实际更改和存储数组。



## 80删除有序数组中的重复项 II

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使得出现次数超过两次的元素只出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

自己解法：

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int len = nums.size(), slowIndex = 1, dupCount = 0;
        if(len == 0) return 0;
        for(int fastIndex = 1; fastIndex < len; fastIndex++){
            if((nums[fastIndex] == nums[fastIndex-1]) && (dupCount < 1)){
                nums[slowIndex++] = nums[fastIndex];
                dupCount++;
            }else if ((nums[fastIndex] == nums[fastIndex-1]) && (dupCount >= 1)){
                if(fastIndex == len-1 || nums[fastIndex] == nums[fastIndex+1]) dupCount++;
                else dupCount = 0;
            }else{
                nums[slowIndex++] = nums[fastIndex];
                dupCount = 0;
            }
        }
        return slowIndex;
    }
};
```

思路：笨办法，加一个dupCount计算重复了几次，超过1次就开始往后找一样的，直到不重复为止。

好解法：

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int len = nums.size(), slowIndex = 2;
        if(len <= 2) return len;
        for(int fastIndex = 2; fastIndex < len; fastIndex++){
            if((nums[fastIndex] != nums[slowIndex-2])){
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

思路：因为本题要求相同元素最多出现两次而非一次，所以我们需要检查上上个应该被保留的元素 ，也就是slowIndex-2和fastIndex的不同即可。



## 189轮转数组

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int len = nums.size();
        vector<int> buffer(len);
        for (int i = 0; i < len; i++){
            buffer[(i+k)%len] = nums[i];
        }
        nums.assign(buffer.begin(),buffer.end());
    }
};
```

思路1：额外空间法。直接创建一个新数组，按照规则存新的元素之后赋值回原来的数组

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int len = nums.size();
        k %= len;
        reverse(nums,0,len-1);
        reverse(nums,0,k-1);
        reverse(nums,k,len-1);
    }
    void reverse(vector<int>& nums,int start, int end){
        while(start<end){
            swap(nums[start++],nums[end--]);
        }
    }
};
```

思路2：数组翻转法思路如下：

```kotlin
nums = "----->-->"; k =3
result = "-->----->";

reverse "----->-->" we can get "<--<-----"
reverse "<--" we can get "--><-----"
reverse "<-----" we can get "-->----->"
this visualization help me figure it out :)
```



## 41缺失的第一个正数

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。



```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int& num: nums){
            if (num <=0) num = n + 1;
        }
        for (int i = 0; i<n; i++){
            int num = abs(nums[i]);
            if (num <= n){
                nums[num-1] = -abs(nums[num-1]);
            }
        }
        for (int i = 0; i< n; i++){
            if (nums[i] > 0) return i+1;
        }
        return n+1;
    }
};
```

思路1：

![image-20230619125658783](https://raw.githubusercontent.com/JeanDiable/MyGallery/main/img/image-20230619125658783.png)



```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i++){
            while(nums[i] >0 && nums[i]<=n && nums[nums[i]-1] != nums[i]) {
                swap(nums[nums[i]-1],nums[i]);
            }
        }
        for(int i = 0; i < n; i++){
            if (nums[i] != i+1)  return i+1;
        }
        return n+1;
    }
};
```

思路2：

![image-20230619130423251](https://raw.githubusercontent.com/JeanDiable/MyGallery/main/img/image-20230619130423251.png)



## 299猜数字游戏

你在和朋友一起玩 [猜数字（Bulls and Cows）](https://baike.baidu.com/item/猜数字/83200?fromtitle=Bulls+and+Cows&fromid=12003488&fr=aladdin)游戏，该游戏规则如下：

写出一个秘密数字，并请朋友猜这个数字是多少。朋友每猜测一次，你就会给他一个包含下述信息的提示：

- 猜测数字中有多少位属于数字和确切位置都猜对了（称为 "Bulls"，公牛），
- 有多少位属于数字猜对了但是位置不对（称为 "Cows"，奶牛）。也就是说，这次猜测中有多少位非公牛数字可以通过重新排列转换成公牛数字。

给你一个秘密数字 `secret` 和朋友猜测的数字 `guess` ，请你返回对朋友这次猜测的提示。

提示的格式为 `"xAyB"` ，`x` 是公牛个数， `y` 是奶牛个数，`A` 表示公牛，`B` 表示奶牛。

请注意秘密数字和朋友猜测的数字都可能含有重复数字。

```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        int bulls = 0, cows = 0;
        vector<int> cntS(10), cntG(10);
        for (int i = 0; i< secret.length();i++){
            if(secret[i]==guess[i]) {bulls++;}
            else{
                ++cntG[guess[i]-'0'];
                ++cntS[secret[i]-'0'];
            }
        }
        for (int i = 0; i<10;i++){
            cows+= min(cntG[i],cntS[i]);
        }
        return to_string(bulls) + "A" + to_string(cows) + "B";
    }
};
```

思路：遍历查找完全一样的，统计bulls的个数。使用「哈希表」进行分别统计 secret 和 guess 的词频，某个数字 x 在两者词频中的较小值，即为该数字对应的奶牛数量，统计所有数字 [0,9] 的奶牛数量总和。



## 134加油站

在一条环路上有 `n` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 `gas` 和 `cost` ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 `-1` 。如果存在解，则 **保证** 它是 **唯一** 的。

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int sum = 0, tag = INT_MAX, ind = 0;;
        int n = gas.size();
        for (int i =0; i<n; i++){
            sum += gas[i];
            sum -= cost[i];
            if (sum < tag) {
                tag = sum;
                ind = i;
            }
        }
        if(sum < 0) return -1;
        else if(tag >= 0) return 0;
        else return (ind+1)%n;
    }
};
```

思路：寻找亏空最严重的那个加油站放到最后走，等其他人多余出来的救他，如果最后总加的油比消耗的少，那就是没有解。



## 118杨辉三角

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>>res(numRows);
        for(int i = 0; i < numRows; i++){
            res[i].resize(i+1);
            res[i][0] = res[i][i]=1;
            for(int j = 1; j < i; j++){
                res[i][j] = res[i-1][j-1]+res[i-1][j];
            }
        }
        return res;
    }
};
```

思路：直接按照杨辉三角的性质构造即可。首尾单独设置成1即可。



## 119杨辉三角2



```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<vector<int>>res(rowIndex+1);
        for(int i = 0; i <= rowIndex; i++){
            res[i].resize(i+1);
            res[i][0] = res[i][i]=1;
            for(int j = 1; j < i; j++){
                res[i][j] = res[i-1][j-1]+res[i-1][j];
            }
        }
        return res[rowIndex];
    }
};
```

思路：直接按照118的代码，最后提取最后一行即可。



## 169多数元素

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        while(true){
            int cand = nums[rand()%nums.size()];
            int count = 0;
            for (int num: nums){
                if(num == cand){
                    count++;
                    if(count > nums.size()/2) return cand;
                }
            }
        }
        return -1;
    }
};
```

思路：因为众数数量大于一半，所以随机选取是众数的概率至少是0.5，所以随机选取n次取到众数的期望是2。加上每次选好众数candidate之后用循环去判断到底是不是众数的复杂度是O(n)。所以时间复杂度是n，空间复杂度是O(1)，没有开新的空间。



```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int,int> hash;
        for(int num: nums){
            hash[num]++;
            if(hash[num] > nums.size()/2) return num;
        }
        return -1;
    }
};
```

思路：哈希表存储，时间复杂度O(n),空间复杂度也是O(n).
