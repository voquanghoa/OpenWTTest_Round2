# Lời giải cho bài thi vòng 2:

## Bài 1. 

Dễ thấy rằng ta chỉ có 2^(n-2) trường hợp nên chỉ cần duyệt hết tất cả các phương án, phương án nào phù hợp thì đếm lại là xong

```kotlin
fun count(nums: IntArray, index: Int, S: Int): Int {
   if(index == nums.size - 1){
       return if(S==0) 1 else 0
   }

   return count(nums, index + 1, S - nums[index]) + count(nums, index + 1, S + nums[index])
}
```

Tham khảo !(https://leetcode.com/problems/target-sum/)[https://leetcode.com/problems/target-sum/] với lời giải là

```kotlin
class Solution {
    fun count(nums: IntArray, index: Int, S: Int): Int {
        if(index == nums.size){
            return if(S==0)  1 else 0
        }
        
        return count(nums, index + 1, S - nums[index]) + count(nums, index + 1, S + nums[index])
    }
    
    fun findTargetSumWays(nums: IntArray, S: Int): Int {
        return count(nums, 0, S)
    }
}
```

## Bài 2

Bài này không thể trực tiếp đếm số trường hợp được, do đó ta phải tính một cách gián tiếp. Đây không phải là cách tối ưu nhất nhưng khả thi, đó là: với mỗi giá trị của mảng a, ta tính tích số lần xuất hiện trên b, c, d. Kết quả cuối cùng được cộng tất cả lại (tất nhiên đừng quên lấy % cho 1000)

```kotlin
private fun IntArray.count(value: Int): Int{
    return this.count { it == value }
}

private fun solve(a: IntArray, b: IntArray, c: IntArray, d: IntArray): Int {

    var count = 0L
    for(x in a){
        count += b.count(x) * c.count(x) * d.count(x)
    }
    return (count % 1000).toInt()
}
```

Hoặc

```kotlin
private fun IntArray.count(value: Int): Int{
    return this.count { it == value }
}

private fun solve(a: IntArray, b: IntArray, c: IntArray, d: IntArray): Int {
    return a.sumBy { b.count(it) * c.count(it) * d.count(it) % 1000 } % 1000
}
```

## Bài 3:

Đọc địa chỉ IP.

Có nhiều cách để giải bài này. 

1. Phương pháp nhánh cận
```kotlin
    fun restoreIpAddresses(s: String): List<String> {

        val result = mutableListOf<String>()

        fun restoreIpAddresses(ip: String, s: String, count: Int){
            if(s.isEmpty()||(count == 0)){
                if(s.isEmpty() && (count == 0)) {
                    result.add(ip)
                }
                return
            }

            val prefix = if(count == 4) ip else "$ip."


            restoreIpAddresses(prefix + s.substring(0, 1), s.substring(1), count - 1)

            if(s.length > 1 && s.substring(0, 2).toInt() >=10){
                restoreIpAddresses(prefix + s.substring(0, 2), s.substring(2), count - 1)
            }

            if(s.length > 2 && s.substring(0, 3).toInt() <= 255 && s.substring(0, 3).toInt() >= 100){
                restoreIpAddresses(prefix + s.substring(0, 3), s.substring(3), count - 1)
            }
        }

        restoreIpAddresses("", s, 4)

        return result
    }
```

2. Duyệt theo từng giá trị IP

```kotlin
class Solution {
    
    fun restoreIpAddresses(s: String): List<String> {

        val result = mutableListOf<String>()

        for(a in 0 .. 255){
            val ip1 = "$a"
            if(s.startsWith(ip1)) for(b in 0..255){
                    val ip2 = "$ip1$b"
                    if(s.startsWith(ip2)) for(c in 0..255){
                        val ip3 = "$ip2$c"
                        if(s.startsWith(ip3)) for(d in 0..255){
                            if(s == "$ip3$d"){
                                result.add("$a.$b.$c.$d")
                            }
                        }
                    }
                }
        }

        return result
    }
}
```

3. Duyệt theo chỉ số của từng IP 

```kotlin
404 Code not found
```

Tham khảo: !(https://leetcode.com/problems/restore-ip-addresses/)[https://leetcode.com/problems/restore-ip-addresses/]

## Bài 4.

Dùng Dictionary/Hashmap hoặc sort mảng rồi thực hiện việc đếm.

