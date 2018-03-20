## String

ver:9.0.4


#### 类的定义</br>
**source & study**

```
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    	......
    }

```


* 被关键字final修饰，阻止其被继承，这是出于安全考虑（String 类可能被恶意继承并篡改）。*这里我们需要注意的是当一个class被final修饰的时候，它的方法也被隐式地被声明为final*，但变量不会被隐式声明。
* 继承了java.io.Serializable（启用其序列化功能）, Comparable<String>（意味着“该类支持排序”）, *CharSequence*（它只包括length(), charAt(int index), subSequence(int start, int end)这几个API接口），以后再具体分析。



#### 关键属性
**source & study**

```
	/**
     * The value is used for character storage.
     *
     * @implNote This field is trusted by the VM, and is a subject to
     * constant folding if String instance is constant. Overwriting this
     * field after construction will cause problems.
     *
     * Additionally, it is marked with {@link Stable} to trust the contents
     * of the array. No other facility in JDK provides this functionality (yet).
     * {@link Stable} is safe here, because value is never null.
     */
    @Stable
    private final byte[] value;

	/**
     * The identifier of the encoding used to encode the bytes in
     * {@code value}. The supported values in this implementation are
     *
     * LATIN1
     * UTF16
     *
     * @implNote This field is trusted by the VM, and is a subject to
     * constant folding if String instance is constant. Overwriting this
     * field after construction will cause problems.
     */
    private final byte coder;

    
    
```

* value 被用作字符存储， 被虚拟机信任，在创建之后被overwriting会导致问题，另外@Stable注解可以确保数组的内容被信任，JDK中只有这里提供了这个功能，在这里是安全的，因为储存的值永不为null。
* coder 这是JDK9的新特性：字符串优化，

>*字符串优化*
>
>JDK 6 引入了可选的 Compressed String 功能，JDK 9 引入了 Compact String ，它们两者的设计目的都是优化字符串在 JVM 中的内存占用。因为 Java 内部使用 UTF-16，占用两
>字节，但从统计角度来说只需 8 比特的情况占大多数，例如：LATIN-1。大多数情况下，字符串实例常占用比它实际需要的内存多一倍的空间。

>Java 6中，在启动参数里使用 -XX:+UseCompressedStrings 可以启用 Compressed String 功能，字符串将以 byte[] 的形式存储，代替原来的 char[]，此功能最终在 JDK 7 中被
除，主要原因在于它将带来一些无法预料的性能问题（存储为 byte[] 后，字符串的操作方法都依赖于字符数组的表现形式，而非字节数组。介于此，很多的方法都需将压缩后的字节数组解压缩为
字符数组，无形中影响了性能）。

>Java 9重新采纳字符串压缩这一概念：通过引入了一个 final 修饰的成员变量 coder, 由它来保存当前字符串的编码信息。若该字符串为 LATIN-1 编码，则使用8个比特来存储，若干 UTF-16 编码，则使用16个比特来编码。
>
>copied from http://tang.love/2017/11/03/new_features_in_java9/


```    
	/** Cache the hash code for the string */
    private int hash; // Default to 0
    
```
* 缓存字符串的哈希值，默认为0


####常用方法

**source & study**

>equals(Object o)

```
	/**
     * Compares this string to the specified object.  The result is {@code
     * true} if and only if the argument is not {@code null} and is a {@code
     * String} object that represents the same sequence of characters as this
     * object.
     *
     * <p>For finer-grained String comparison, refer to
     * {@link java.text.Collator}.
     *
     * @param  anObject
     *         The object to compare this {@code String} against
     *
     * @return  {@code true} if the given object represents a {@code String}
     *          equivalent to this string, {@code false} otherwise
     *
     * @see  #compareTo(String)
     * @see  #equalsIgnoreCase(String)
     */
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String aString = (String)anObject;
            if (coder() == aString.coder()) {
                return isLatin1() ? StringLatin1.equals(value, aString.value)
                                  : StringUTF16.equals(value, aString.value);
            }
        }
        return false;
    }

```

* 

>compareTo()

```

```

* 

>contains()

```

```
* 


>replace()

```

```

* 


>trim()

```

```

* 

>charAt()

```

```

* 

>chars()

```

```

* 


>indexOf()

```

```

* 



>split()

```

```

* 




>isEmpty()

```

```

*



>contentEquals()

```

```

*

>getBytes()

```

```

*


* 构造方法，











## StringBuffer




