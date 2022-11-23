# Lombok 생성자 자동 생성
Lombok(롬복)은 Java 라이브러리로 반복되는 getter, setter, toString 등의 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리입니다. 
Spring boot를 사용하면서 가장 헷갈렸던 생성자 자동생성 어노테이션인 NoArgsConstructor/ RequiredArgsConsturctor/ AllArgsConstructor를 정리할려고 합니다.

## NoArgsConstructor/ RequiredArgsConsturctor/ AllArgsConstructor
생성자를 자동으로 생성해주는 어노테이션


## NoArgsConstructor
파라미터가 없는 생성자를 만듭니다.

**Lombok**
``` java
@NoArgsConstructor
public class Lombok {
    private String one;

    public Lombok(String one){
        this.one = one;
    }
}

```
**Vanilla Java**
```java
public class Lombok {
    private String one;

    public Lombok(String one) {
        this.one = one;
    }
    public Lombok(){
    }
}

```


## RequireArgsConsturctor
이 애노테이션은 추가 작업을 필요로 하는 필드에 대한 생성자를 생성하는 애노테이션입니다.

초기화 되지 않은 모든 final 필드, @NonNull로 마크돼있는 모든 필드들에 대한 생성자를 자동으로 생성해줍니다.

@NonNull로 마크돼있는 필드들은 null-check가 추가적으로 생성되며, @NonNull이 마크되어 있지만, 파라미터에서 null값이 들어온다면 생성자에서 NullPointerException이 발생합니다.

파라미터의 순서는 클래스에 있는 필드 순서에 맞춰서 생성됩니다.

**Lombok**
``` java
@RequireArgsConstructor
public class Lombok {
    private String one;
    private final String two;
    @NonNull
    private String three;
}

```
**Vanilla Java**
```java
public class Lombok {
    private String one;
    private final String two;
    @NonNull
    private String three;
    
    public Lombok(String two, @NonNull String three){
        if (three == null){
            throw new NullPointException("three is marked non-null but is null");
        } else {
            this.two = two;
            this.three = three;
        }
    }
}
```
final 변수와 @NonNull 마크 변수에 해당하는 생성자가 생성된 것을 보실 수 있습니다.

그리고 null-check 로직이 자동적으로 생성된 것도 보실 수 있습니다.
## AllArgsConsturctor
이 애노테이션은 클래스에 존재하는 모든 필드에 대한 생성자를 자동으로 생성해줍니다.

만약 필드중에서 @NonNull 애노테이션이 마크되어 있다면 생성자 내에서 null-check 로직을 자동적으로 생성해줍니다.

**Lombok**
``` java
@AllArgsConstructor
public class Lombok {
    private String one;
    private String two;
    @NonNull
    private String three;
}

```
**Vanilla Java**
```java
public class Lombok {
    private String one;
    private String two;
    @NonNull
    private String three;

    public Lombok(String one, String two, @NonNull String three){
        if (three == null){
            throw new NullPointException("three is marked non-null but is null");
        } else {
            this.one = one;
            this.two = two;
            this.three = three;
        }
    }
}
```
모든 필드를 가진 생성자를 자동으로 생성해주는 것을 볼 수 있습니다.

## Reference
https://dingue.tistory.com/14  
https://projectlombok.org/features/constructor