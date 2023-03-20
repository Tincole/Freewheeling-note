## nSum问题

[参考链接](https://mp.weixin.qq.com/s/fSyJVvggxHq28a0SdmZm6Q)

技巧：双指针，从两端向中间移动

```js
const numTarget = (nums,n,start,target)=>{
    let res = [];
    if (n<2) return res;
    //twoSum问题
    if (n === 2){
      let i = start,len=nums.length,j = len - 1;
      while(i < j){
        let left = nums[i],right = nums[j];
        if (left+right<target){
          while(left === nums[i]) i++;
        } else if (left+right>target){
          while(right === nums[j]) j--;
        } else {
          res.push([left,right]);
          while(left === nums[i]) i++;
          while(right === nums[j]) j--;
        }
      }
    } else {
      for (let i=start;i<nums.length;i++){
        const result = numTarget(nums,n-1,i+1,target - nums[i]);
        result.forEach(item=>{
          item.push(nums[i]);
          res.push(item);
        })
        //去重
        while(nums[i]===nums[i+1]) i++;
      }
    }
    return res;
  }
  //先排序
  nums.sort((a,b)=>a-b);
  return numTarget(nums,4,0,target);
```

