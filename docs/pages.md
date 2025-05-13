### 题目

> 现有两个整数数组，需要你找出两个数组中同时出现的整数，并按照如下要求输出:
> 
> 1.有同时出现的整教时，先按照同时出现次数(整数在两人数组中都出现并目出现次数较少的那人)进行归类，然后按照出现次数从小到大依次按行输出。  
> 2.没有同时出现的整数时，输出NULL  
> **输入描述**  
> 第一行为第一个整数数组，第二行为第二个整数数组，每行数中整数与整数之间以英文号分，整数的取值范用为200,2001，数组长度的范用为\[1，10000\]之间的整数。  
> **输出描述**  
> 按照出现次数从小到大依次按行输出，每行输出的格式为:  
> 出现次数:该出现次数下的整数升序排序的结果  
> 格式中的"."为英文冒号，整数间以英文逗号分隔.
> 
> 示例1：  
> 输入
> 
> 5,3,6,-8,0,11  
> 2,8,8,8,-1,15  
> 输出
> 
> NULL  
> 说明  
> 两个整数数组没有同时出现的整数，输出NULL。
> 
> 示例2：
> 
> 输入：
> 
> 5,8,11,3,6,8,8,-1,11,2,11,11  
> 11,2,11,8,6,8,8,-1,8,15,3,-9,11  
> 输出
> 
> 1:-1,2,3,6
> 
> 3:8,11
> 
> 说明  
> 两整数数组中同时出现的整数为-1、2、3、6、8、11,其中同时出现次数为1的整数为-1,2,3，6(升序排序),同时出现次数为3的整数为8,11(升序排序),先升序输出出现次数为1的整数，再升序输出出现次数为3的整数。

## 思路

> 1：考察的就是数据结构的熟悉程度。各种map的操作。

## Code

```java
import java.util.Scanner;
import java.util.*;
import java.util.stream.Stream;
import java.util.HashMap;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.util.stream.Collectors;
 
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String input_str = in.nextLine();
        String[] tmp2 = input_str.split(",");
        int[] nums1 = new int[tmp2.length];
        for (int i = 0; i < tmp2.length; i++) {  
            nums1[i] = Integer.parseInt(tmp2[i]);
        }

        String input_str1 = in.nextLine();
        String[] tmp1 = input_str1.split(",");
        int[] nums2 = new int[tmp1.length];
        for (int i = 0; i < tmp1.length; i++) {  
            nums2[i] = Integer.parseInt(tmp1[i]);
        }

        HashMap<Integer, Integer> num1_map = new HashMap<>();
        int i=0;
        while(true){
            if (i>=nums1.length){
                break;
            }else{
                if(num1_map.containsKey(nums1[i])){
                    num1_map.put(nums1[i], num1_map.get(nums1[i])+1);
                } else{
                    num1_map.put(nums1[i], 1);
                }
            }
            i+=1;
        }

        HashMap<Integer, Integer> num2_map = new HashMap<>();
        int j=0;
        while(true){
            if (j>=nums2.length){
                break;
            }else{
                if(num2_map.containsKey(nums2[j])){
                    num2_map.put(nums2[j], num2_map.get(nums2[j])+1);
                } else{
                    num2_map.put(nums2[j], 1);
                }
            }
            j+=1;
        }

        boolean flag = true;
        String output_str = "NULL";
        HashMap<Integer, ArrayList<Integer>> result_map = new HashMap<>();
        for (Integer num : num1_map.keySet()) {
            if (num2_map.containsKey(num)) {
                flag = false;
                int count = Math.min(num1_map.get(num), num2_map.get(num));
                if (result_map.containsKey(count)) {
                    result_map.get(count).add(num);
                } else {
                    result_map.put(count, new ArrayList<>());
                    result_map.get(count).add(num);
                }
            }
        }
        if (!flag) {
            ArrayList<Integer> temp = new ArrayList<>();
            for (Integer num : result_map.keySet()) {
                temp.add(num);
            }
            Collections.sort(temp, new Comparator<Integer>() {
                @Override
                //在这里主要是重写了 Comparator类的compare方法，
                //sort方法可能也用了这个方法进行排序，然后在这里被重写了。
                public int compare(Integer o1, Integer o2) {
                    return o1 - o2;//当大于0就交换，否则不交换。
                }
            });
            output_str = "";
            for (int m=0;m<temp.size();m++){
                output_str += temp.get(m)+":";
                for (int n=0;n<result_map.get(temp.get(m)).size();n++){
                    output_str += result_map.get(temp.get(m)).get(n);
                    if(n!=result_map.get(temp.get(m)).size()-1){
                        output_str += ",";
                    }
                }
                output_str += "\n";
            }
            System.out.println(output_str);
            
        } else {
            System.out.println(output_str);
        }
    
        return;
    }
}
```

### 要求

> 时间限制：C/C++ 1秒，其他语言 2秒
> 
> 空间限制：C/C++262144K，其他语言524288K
