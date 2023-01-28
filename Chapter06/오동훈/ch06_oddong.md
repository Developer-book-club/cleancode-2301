# 6장 객체와 자료 구조
---

## 1. 자료 추상화
- 변수 사이에 함수라는 계층을 넣는다고 구현이 저절로 감춰지지는 않는다. 구현을 감추려면 추상화가 필요하다!
- 인터페이스 조회.설정 함수만으로는 추상화가 이뤄지지 않는다. 자료를 세세하게 공개하기보다는 추상적인 개념으로 표현하는 편이 좋다.


```java
# 구체적인 Point 클래스 (구현을 외부로 노출)

public class Point {
  private double x;
  private double y;
}
```

```java
// 추상적인 Point 클래스 (구현을 완전히 숨김)

public interface Point {
  double getX();
  double getY();
  void setCatesian(double x, double y);
  double getR();
  double getTheta();
  void setPolar(double r, double theta);
}
```

```java
# 구체적인 Vehicle 클래스 (구현을 외부로 노출)
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}
```

```java
추상적인 Vehicle 클래스 (구현을 완전히 숨김)
public interface Vehicle {
  double getPercentFuelRemaining();
}
```
## 2. 자료/객체 비대칭



## 3. 디미터 법칙
- 모듈은 자신이 조작하는 객체의 내부 사정을 몰라야 한다는 법칙으로, 객체는 자료를 숨기고 함수를 공개한다.

* 휴리스틱(heuristic) : 발견법(發見法)이라고도 하며, 불충분한 시간이나 정보로 인하여 합리적인 판단을 할 수 없거나, 체계적이면서 합리적인 판단이 굳이 필요하지 않은 상황에서 사람들이 빠르게 사용할 수 있게 보다 용이하게 구성된 간편 추론의 방법

### 1. 기차 충돌
- 여러 객체가 한 줄로 이어진 기차처럼 보이는 코드를 기차 충돌(train wreck)이라고 한다. 좋지 않은 방식이므로 피하자

```java
# 기차 충돌 안좋은 예
String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();

# 좋은 예
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
String outputDir = scratchDir.getAbsolutePath();
```

### 2. 잡종 구조
### 3. 구조체 감추기
## 4. 자료 전달 객체
### 1. 활성 레코드