#### 无重复数字的partition sort
```
function partition(nums, left, right) {
  let pivot = nums[left];

  while (left < right) {
    while (left < right && nums[right] >= pivot) {
      right--;
    }

    nums[left] = nums[right];
    while (left < right && nums[left] <= pivot) {
      left--;
    }

    nums[right] = nums[left];
  }

  nums[left] = pivot;
  return left; // left就是pivor的index
}
```
#### Quick sort
```
function partition(nums, left, right) {
  let pivot = nums[(left + right) / 2];

  while (left <= right) {
    while (left <= right && nums[left] < pivot) {
      left++;
    }

    while (left <= right && nums[right] > pivot) {
      right++;
    }

    if (left <= right) {
      swap(nums, left, right);
      left++;
      right--;
    }

    return left; //left是pivot的index + 1
  }
}

function quicksort(nums) {
  partition(nums, 0, nums.length - 1);
}
```