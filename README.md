# PRReview
PRReview Project

add Java Code


```Java
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadSafeHashing {

    private static final ConcurrentHashMap<String, String> hashMap = new ConcurrentHashMap<>();

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(10); // 创建一个固定大小的线程池

        String[] data = {"data1", "data2", "data3", "data4", "data5"}; // 待加密数据

        for (String item : data) {
            executorService.submit(() -> {
                try {
                    String hash = hashData(item);
                    hashMap.put(item, hash); // 将哈希值保存到ConcurrentHashMap中
                    System.out.println("Data: " + item + " Hash: " + hash);
                } catch (NoSuchAlgorithmException e) {
                    e.printStackTrace();
                }
            });
        }

        executorService.shutdown(); // 关闭线程池
    }

    private static String hashData(String data) throws NoSuchAlgorithmException {
        MessageDigest digest = MessageDigest.getInstance("SHA-256"); // 使用SHA-256算法
        byte[] hash = digest.digest(data.getBytes());
        StringBuilder hexString = new StringBuilder();
        for (byte b : hash) {
            String hex = Integer.toHexString(0xff & b);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }
        return hexString.toString(); // 返回哈希值的十六进制字符串表示
    }
}
```
