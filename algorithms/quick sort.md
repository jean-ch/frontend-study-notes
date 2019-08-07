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
      left++;
    }

    nums[right] = nums[left];
  }

  nums[left] = pivot;
  return left; // left就是pivot的index
}
```
#### Quick sort
```
function partition(arr, start, end) {
  let pivot = arr[end];
  let i = start;
  let j = end;
	while (i < j) {
		while (i < j && arr[i] <= pivot) {
			i++;
		}

		while (i < j && arr[j] >= pivot) {
			j--;
		}

		if (i < j) {
			[arr[i], arr[j]] = [arr[j], arr[i]];
		}
	}
    
  [arr[end], arr[i]] = [arr[i], arr[end]];
	return i;  // i是pivot的index
}

function quicksort(nums) {
  partition(nums, 0, nums.length - 1);
}
```