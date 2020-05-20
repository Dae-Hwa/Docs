# 중첩된 If 없애기

if문이 필연적으로 겹처질 때가 있다.

```java
public int calcluate(String operator, int left, int right){
    if(operator.equals("+")){
        return left+right;
    }else if(operator.equals("-")){
        return left-right;
    }else if(operator.equals("*")){
        return left*right;
    }else if(operator.equlas("/")){
        return left/right;
    }
}
```

먼저 해볼 수 있는 조치는 기능을 분리해보는 것이다.

> 메소드 먼저 분리, 책임 분리

```java
class PlusCalculator{
    public int calculate(int left, int right){
        return left + right;
    }
}
class MinusCalculator{
    public int calculate(int left, int right){
        return left - right;
    }
}
class MulitplyCalculator{
    public int calculate(int left, int right){
        return left * right;
    }
}
class DivisionCalculator{
    public int calculate(int left, int right){
        return left / right;
    }
}
```

하지만 분리해도 크게 다르지 않은것처럼 보일 수 있다.

```java
public int calcluate(String operator, int left, int right){
    if(operator.equals("+")){
        return new PlusCalculator().calculate(left, right);
    }else if(operator.equals("-")){
		return new MinusCalculator().calculate(left, right);
    }else if(operator.equals("*")){
        return new MultiplyCalculator().calculate(left, right);
    }else if(operator.equlas("/")){
        return new DivisionCalculator().calculate(left, right);
    }
}
```

이런 경우 다형성을 이용해볼 수 있다.

```java
public interface Calculator {
  int calculate(int left, int right);
}

class PlusCalculator implements Calculator {
  @Override
  public int calculate(int left, int right) {
    return left + right;
  }
}

class MinusCalculator implements Calculator {
  @Override
  public int calculate(int left, int right) {
    return left - right;
  }
}

class MulitplyCalculator implements Calculator {
  @Override
  public int calculate(int left, int right) {
    return left * right;
  }
}

class DivisionCalculator implements Calculator {
  @Override
  public int calculate(int left, int right) {
    return left / right;
  }
}
```

```java
public int calculate (Calculator calculator, int left, int right){
    return calculator.calculate(left, right);
}
```



```java
class CalculatorMapper {
    private static Map<String, Calculator> calculatorMap = new HashMap<>();
    
    static{
        calculatorMap.put("+", new PlusCalculator());
        calculatorMap.put("-", new MinusCalculator());
        calculatorMap.put("*", new MultiplyCalculator());
        calculatorMap.put("/", new DivisionCalculator());
    }
    
    public static Map<String, Calculator> getCalculatorByOperator(String operator) {
        return calculatorMap.get(operator);
    }
}
```

```java
public int calculator (String operator, int left, int right) {
    return CalculatorMapper.getCalculatorByOperator(operator)
        .calculate(left, right);
}
```

> 이후 enum사용 케이스 설명











