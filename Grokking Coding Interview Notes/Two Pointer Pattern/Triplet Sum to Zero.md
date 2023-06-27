1. Sort our array to make it easier to find duplicates.
2. Set an array to hold the triplets that have a sum of zero.
3. Set a for loop to loop through the array
	1. Check if duplicate and the index is greater than 0 , if so we skip and do a `continue` 
	2. Otherwise, we want to search for the pair -> this part is similar to a two sum pair problem where we use left and right pointers to figure out the sums necessary.
	3. So we will make a function that can handle this for us so we don't need to create more variables that aren't necessary.
	4. Function will be searchPair() and the params will be
		1. array which is the array
		2. targetSum which is the negative version of the current number in the array
		3. left pointer which is the current position in the for loop + 1
		4. and the triplets array -> we can also set this as a class level variable if we wanted to remove this param
	5. return triplets!
4. SearchPair function -> searchPair(arr, targetSum, left, triplets)
	1. Create the right pointer which is the arr length minus 1
	2. Create while loop as long as left > right (so we do not get index out of bounds)
		1. Create variable currentSum to sum arr[left] and arr[right] to compare with later.
		2. If statement to check if current sum == target sum 
			1. Woo we have the triplets and we want to push them onto the array
			2. Update left pointer
			3. update right pointer
			4. While loop where left < right && arr[left] === arr[left -1]
				1. Add to left 
				2. This while loop is really to avoid duplicates again
			5. While loop where left < right && arr[right] == arr[right +1];
				1. Subtract from right
				2. This while loop skips to avoid duplicates again
		3. else if our targetSum > currentSum
			1. That means we need to move our left one space.
		4. else we just need to subtract 1 from right to continue searching
5. 