##### 无重复数字的partition
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
    if (nums[left] < nums[right]>) {
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