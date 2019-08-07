#### Merge sort
```
function mergeSort(nums) {
  partition(nums, 0, nums.length - 1, []);
}

function partition(nums, start, end, temp) {
  if (start >= end) {
    return;
  }

  let mid = (start + end) / 2;
  partition(nums, start, mid - 1, temp);
  partition(nums, mid, end, temp);
  merge(nums, start, mid, end, temp);
}

function(nums, start, mid, end, tmp) {
  if (start >= mid || mid >= end) {
    return;
  }

  let left = start;
  let right = mid;
  let index = start;
  while (left <= mid && right <= end) {
    if (nums[left] < nums[right]) {
        temp[index++] = nums[left++];
    } else {
      temp[index++] = nums[right++];
    }
  }

  while (left <= mid) {
    temp[index++] = nums[left++];
  }

  while (right <= end) {
    temp[index++] = nums[right++];
  }

  for (let i = start; i <= end; i++) {
    nums[i] = temp[index];
  }
}
```