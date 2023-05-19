# Fast modulo

```java
    static long recursion(int a, int b, int c) {
        if (b == 0)
            return 1;
        long res = recursion(a, b / 2, c);
        if (b % 2 != 0) {
            return (a * ((res * res) % c)) % c;

        } else {
            return (res * res) % c;
        }
    }
```

# Johepers

```java
    public static int rec(int n, int k) {
        if (n == 1) {
            return 0;
        }
        int x = rec(n - 1, k);
        int y = (x + k) % n;
        return y;
    }
```

# Longest sub seq

```cpp
int find_loc_to_insert(vector<int> &arr, int target)
{
    int left = 0;
    int right = arr.size() - 1;
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return right;
}
int longest_len_using_bs(int nums[], int length)
{
    vector<int> t;
    int j = 0;

    while (j < length)
    {
        int curr = nums[j];
        if (t.size() == 0 || t.back() < curr)
        {
            t.push_back(curr);
            j++;
            continue;
        }
        int loc = find_loc_to_insert(t, curr) + 1;
        t.erase(t.begin() + loc);
        t.insert(t.begin() + loc, curr);
        j++;
    }

    printf("Array\n");
    for (int i = 0; i < t.size(); i++)
    {
        printf("%d => %d\n", i, t.at(i));
    }

    return t.size();
}
```

# Merge

```java
	public static void merge(int arr[], int l, int m, int r)
	{
		int n1 = m - l + 1;
		int n2 = r - m;
		int L[] = new int[n1];
		int R[] = new int[n2];
		for (int i = 0; i < n1; ++i)
			L[i] = arr[l + i];
		for (int j = 0; j < n2; ++j)
			R[j] = arr[m + 1 + j];
		int i = 0, j = 0;
		int k = l;
		while (i < n1 && j < n2) {
			if (L[i] <= R[j]) {
				arr[k] = L[i];
				i++;
			}
			else {
				arr[k] = R[j];
				j++;
			}
			k++;
		}
		while (i < n1) {
			arr[k] = L[i];
			i++;
			k++;
		}
		while (j < n2) {
			arr[k] = R[j];
			j++;
			k++;
		}
	}

	public static void sort(int arr[], int l, int r)
	{
		if (l < r) {
			int m = l + (r - l) / 2;
			sort(arr, l, m);
			sort(arr, m + 1, r);
			merge(arr, l, m, r);
		}
	}
```

# Quick

```java
	static void swap(int[] arr, int i, int j)
	{
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}

	static int partition(int[] arr, int low, int high)
	{
		int pivot = arr[high];
		int i = (low - 1);

		for (int j = low; j <= high - 1; j++) {
			if (arr[j] < pivot) {
				i++;
				swap(arr, i, j);
			}
		}
		swap(arr, i + 1, high);
		return (i + 1);
	}

	static void quickSort(int[] arr, int low, int high)
	{
		if (low < high) {
			int pi = partition(arr, low, high);
			quickSort(arr, low, pi - 1);
			quickSort(arr, pi + 1, high);
		}
	}
```

# Huffman

```java
public class Huffman {

    public static void main(String[] args) {
        Character s[] = { 'a', 'a', 'b', 'b', 'c', 'd', 'd', 'd' };
        HashMap<Character, Integer> hm = new HashMap<Character, Integer>();
        for (Character character : s) {
            if (hm.containsKey(character))
                hm.put(character, hm.get(character) + 1);
            else
                hm.put(character, 1);
        }

        for (Character character : hm.keySet())
            System.out.println(character + " " + hm.get(character));

        TreeNode[] nodes = new TreeNode[hm.size()];

        int j = 0;
        for (Character k : hm.keySet())
            nodes[j++] = new TreeNode(Character.toString(k), hm.get(k));

        Integer min_1_idx = -1, min_2_idx = -1;
        while (nodes.length != 1) {
            TreeNode min_1 = new TreeNode(null, 1000000), min_2 = new TreeNode(null, 1000000);
            for (int i = 0; i < nodes.length; i++) {
                TreeNode curr_node = nodes[i];
                if (curr_node.freq < min_1.freq) {
                    min_1 = curr_node;
                    min_1_idx = i;
                } else if (curr_node.freq < min_2.freq) {
                    min_2 = curr_node;
                    min_2_idx = i;
                }
            }
            TreeNode[] new_nodes = new TreeNode[nodes.length - 1];
            Integer jx = 0;
            for (int i = 0; i < nodes.length; i++) {
                if (i == min_1_idx || i == min_2_idx)
                    continue;
                new_nodes[jx++] = nodes[i];
            }
            new_nodes[jx++] = new TreeNode(min_1, min_2);
            nodes = new_nodes;
        }
        System.out.println("Nodes max");
        nodes[0].in_order(nodes[0], new LinkedList<Integer>());
        System.out.println(nodes[0].hm_codes);
    }
}

class TreeNode {
    String data;
    int freq;
    TreeNode left;
    TreeNode right;
    HashMap<Character, String> hm_codes = new HashMap<Character, String>();

    public TreeNode(String data, int freq) {
        this.data = data;
        this.freq = freq;
        this.left = null;
        this.right = null;
    }

    public TreeNode(TreeNode left, TreeNode right) {
        this.data = "(" + left.data + left.data + ")";
        this.freq = left.freq + right.freq;
        this.left = left;
        this.right = right;
    }

    public void in_order(TreeNode r, LinkedList<Integer> code_data) {
        if (r == null)
            return;
        LinkedList<Integer> cloned_for_left = (LinkedList<Integer>) code_data.clone();
        cloned_for_left.push(0);
        in_order(r.left, cloned_for_left);
        LinkedList<Integer> cloned_for_right = (LinkedList<Integer>) code_data.clone();
        cloned_for_right.push(1);
        in_order(r.right, cloned_for_right);
        if (!r.data.contains("(")) {
            String s = "";
            Iterator<Integer> itr = code_data.iterator();
            while (itr.hasNext())
                s = s + itr.next();
            hm_codes.put(r.data.charAt(0), s);
        }
    }
}
```
