# 2024091203030-wufangyu-JAVA-07
## Task1
1.
Exception
* NullPointerException：在栈内存中保存的地址为null后尝试使用其指向堆内存中的对象就会报空指针异常
* ArithmeticException：运算不合法时出现，如0作为除数时
* IllegalArgumentException：方法接受的参数不符合定义的范围或参数类型错误时会报该异常
* 处理态度：可以捕获并处理异常来使程序出现问题时按照自己设计的预期退出或者继续运行

Error
* OutOfMemoryError：通常是堆内存不足，无法存储新的对象时发生，原因有可能是短时间创建了大量对象且没有进行垃圾回收，或是某些垃圾对象回收有异，或是存储了非常大的对象。
* NoClassDefFoundError：类路径设置不正确导致的。如第三方库没有被正确添加到类路径中，eg:从github上下载的项目手动引入到自己的项目中时没有正确添加到类路径中时
* 处理态度：一般是记录错误信息并且尽快终止程序
2.
checked异常
* IOException输入输出异常：读取不存在的文件，或网络连接中断等输入输出时出现问题
* ClassNotFoundException类未找到异常：尝试加载一个类但是该类在类路径中无法找到时抛出

unchecked异常
* ArithmeticException：运算不合法时出现，如0作为除数时
* IllegalArgumentException：方法接受的参数不符合定义的范围或参数类型错误时会报该异常

区别：
checked异常在写代码即编译的时候就必须给出处理方式，而unchecked异常则在不强制在编译时处理。
## Task2
3.
取款成功：
* 首先创建一个account对象，并且随机生成了一个balance（小于等于200）（要取款成功则balance应该大于等于150.0）
* 接着输出"当前余额: " + account.getBalance() //输出“当前余额：（balance的值）”
* account对象调用withdraw方法取出150.0元，此时amount<balance，则运行balance -= amount不会抛出异常
* 输出“取款成功”
* 输出“程序结束”

取款失败
* 首先创建一个account对象，并且随机生成了一个balance（小于等于200）（要取款失败则balance小于150.0）
* 接着输出"当前余额: " + account.getBalance() //输出“当前余额：（balance的值）”
* accoun对象调用withd方法尝试取款，但由于amount>balance，则抛出InsufficientFundsException异常
* 进入catch块并输出“错误：余额不足，无法取款。当前余额:（balance的值）”
* 输出“程序结束”

4.
```java
package com.glimmer;

import java.io.*;
//定义EmptyFileException异常
class  EmptyFileException extends Exception{
    public EmptyFileException(String message){
        super(message);
    }
}

public class datatest {
    public static void main(String[] args) {
        //定义文件路径
        String data = "D:/allproject/java07/src/com/glimmer/data.txt";
            try(BufferedReader br = new BufferedReader(new FileReader(new File(data)))){
                int sum = 0;
                int numbers = 0;
                String line = br.readLine() ;
                //文件为空时抛出异常
                if(line == null){
                    throw new EmptyFileException(("文件为空"));
                }
                //计算文件内数据总和
                while(line !=null){
                    int everyline = Integer.parseInt(line);
                    sum +=everyline;
                    numbers++;
                }
                //求平均数
                if (numbers != 0) {
                    double average = (double)sum/numbers;
                    System.out.println("整数的平均数为："+average);
                }
                //捕获异常
            } catch (IOException e) {
            throw new RuntimeException(e);
            } catch (EmptyFileException e) {
            System.err.println(e.getMessage());
            } catch (FileNotFoundException e) {
            System.err.println("文件未找到："+e.getMessage());
            }catch (NumberFormatException e) {
            System.err.println("读取到的内容格式错误：" + e.getMessage());
            }
        }
    }
}

```