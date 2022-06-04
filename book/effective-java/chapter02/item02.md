# 생성자에 매개변수가 많다면 빌더를 고려하라

## 점층적 생성자 패턴

정적 팩터리와 생성자에는 선택적 매개변수가 많을 때 적절히 대응하기 어려운 제약이 있다. 이런 클래스들은 점층적 생성자 패턴(telescoping constructor pattern)을 통해 보통 구현하는데 매개변수가 2, 3개 정도로 적으면 다행이지만 정말 10개가 넘어가는 경우에는 중간중간 불필요한 값에 대한 초기화도 필수로 설정을 해줘야 한다.

- 점층적 생성자 패턴 예시

```java
public class NutritionFacts {
    private final int servingSize;  // ml, 1회 제공량       (필수)
    private final int servings;     // 회, 총 n회 제공량    (필수)
    private final int calories;     // 1회 제공량당         (선택)
    private final int fat;          // g, 1회 제공량당      (선택)
    private final int sodium;       // mg, 1회 제공량당     (선택)
    private final int carbohydrate; // g, 1회 제공량당      (선택)

    public NutritionFacts(int servingSize, int servings) {
        this(servingSize, servings, 0);    
    }

    public NutritionFacts(int servingSize, int servings, int calories) {
        this(servingSize, servings, calories, 0);   
    }

    public NutritionFacts(int servingSize, int servings, int calories,
                            int fat)
    {
        this(servingSize, servings, calories, fat, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories,
                            int fat, int sodium)
    {
        this(servingSize, servings, calories, fat, sodium, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories,
                            int fat, int sodium, int carbohydrate)
    {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }

}
```

점픙적 생성자 패턴에서 생성자를  호출하려면 다음과 같이 **원하는 매개변수를 모두 포함한 생성자를 호출해야 한다.**

```java
NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
```

이런 방식은 코드 작성 시 **불필요한 매개변수들도 하나하나 다 설정해야 하며 가독성도 매우 떨어진다.**
게다가 **사용자의 실수로 하여금 매개변수 순서가 바뀐다면 버그로 이어질 수 있다!**


## 자바빈즈 패턴

자바빈즈 패턴(JavaBeans pattern)은 매개변수가 없는 생성자로 객체를 만든 후, 세터(setter) 메서드들을 호출하여 원하는 매개변수의 값을 설정하는 방식이다.

- 자바빈즈 패턴 예시

```java
public class NutritionFacts {
    // 매개변수들은 (기본값이 있다면) 기본값으로 초기화된다.
    private int servingSize = -1;  // ml, 1회 제공량(필수) 기본값 없음
    private int servings = -1;     // 회, 총 n회 제공량    (필수) 기본값 없음
    private int calories = 0;     // 1회 제공량당         (선택)
    private int fat = 0;          // g, 1회 제공량당      (선택)
    private int sodium = 0;       // mg, 1회 제공량당     (선택)
    private int carbohydrate = 0; // g, 1회 제공량당      (선택)
    
    public NutritionFacts(){}       

    // 세터 메서드들    
    public void setServingSize(int val) { servingSize = val; }
    public void setServings(int val) { servings = val; }
    public void setCalories(int val) { calories = val; }
    public void setFat(int val) { fat = val; }
    public void setSodium(int val) { sodium = val; }
    public void setCarbohydrate(int val) { carbohydrate = val; }
}
```

점층적 생성자 패턴의 단점들을 자바빈즈 패턴에서는 보완이 되어 보다 인스턴스 생성이 쉽고 가독성이 높아졌다.

```java
NutritionFacts cocaCola = NutritionFacts();
cocaCola.setServingSize(240);
cocaCola.setServings(8);
cocaCola.setCalories(100);
cocaCola.setSodium(35);
cocaCola.setCarbohydrate(27);
```

하지만 자바빈즈 패턴에서도 심각한 단점이 존재한다. 자바빈즈 패턴에서는 **객체 하나를 만들려면 세터 메서드를 여러 개 호출해야 하고, 객체가 완전히 생성되기 전까지는 일관성(consistency)이 무너진 상태에 놓이게 된다.**

이처럼 일관성이 무너지는 문제로인해 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며 스레드 안전성을 위해 추가 코드 작성이 필요하다.(생성 이후 객체를 수동으로 freezing 후 메서드로 값 할당 같은 구현 작업..)

## 빌더 패턴

빌더 패턴(Builder pattern)은 점층적 생성자 패턴의 안전성과 자바빈즈 패턴의 가독성을 겸비하였다.

사용자는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자(혹은 정적 팩터리)를 호출해 빌더 객체를 얻는다. 그런 뒤 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수들을 설정한다. 이후 사용자는 build 메서드를 통해 필요한 객체를 얻을 수 있다.

- 빌더 패턴 예시

```java
public class NutritionFacts {    
    private final int servingSize;    
    private final int servings;    
    private final int calories;    
    private final int fat;    
    private final int sodium;    
    private final int carbohydrate;    

    public static class Builder {
        // 필수 매개변수
        private final int servingSize;
        private final int servings;

        // 선택 매개변수 - 기본값으로 초기화한다.
        private int calories      = 0;
        private int fat           = 0;
        private int sodium        = 0;
        private int carbohydrate  = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;            
            this.servings    = servings;        
        }

        public Builder calories(int val) { 
            calories = val;
            return this;
        }

        public Builder fat(int val) {
            fat = val;
            return this;
        }

        public Builder sodium(int val) {
            sodium = val;
            return this;
        }

        public Builder carbohydrate(int val) {
            carbohydrate = val;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize  = builder.servingSize;
        servings     = builder.servings;
        calories     = builder.calories;
        fat          = builder.fat;
        sodium       = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
}
```

NutritionFacts 클래스는 불변이며, 모든 매개변수의 기본값들을 한곳에 모아뒀다.

빌더의 세터 메서드들은 빌더 자신을 반환하기 때문에 연쇄적으로 호출할 수 있다. 이런 방식을 메서드 호출이 흐르듯 연결된다는 뜻으로 플루언트 API(fluent API) 혹은 메서드 연쇄(method chaining)라 한다.

```java
public static void main(String[] args) {
    NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
                .calories(100)
                .sodium(35)
                .carbohydrate(27)
                .build();
}
```

빌더 패턴은 사용할 때 역시 간단하고 가독성이 있다. 

>빌더 패턴은(파이썬과 스칼라에 있는) 명명된 선택적 매개변수(named optional parameters)를 흉내 낸 것이다.




