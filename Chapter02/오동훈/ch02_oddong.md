# 2장 의미 있는 이름

---

## 1. 의도를 분명히 밝혀라
- 변수나 함수, 클랙스에 대한 주석이 필요하지 않도록 네이밍을 해야한다.

```java
### 지향해야 할 네이밍 ###
int d; # 경과 시간(단위: 날짜)


### 의도가 드러나는 네이밍 ###
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

- 코드가 하는 일을 짐작할 수 있게 해라. 너무 함축적이면 이해하기 어렵다.

```java
### 짐작하기 어려운 코드 ###
public List<int[]> getThem{
	List<int[]> list1 = new ArrayList<int[]>();
	for (int[] x : theList)
		if (x[0] == 4)
			list1.add(x);
	return list1;
}


### 앞선 코드보다 명확한 코드 ###
public List<int[]> getFlaggedCells(){
	List<Cell> flaggedCells = new ArrayList<Cell>();
	for (Cell cell : gameBoard)
		if (cell.isFlagged())
			flaggedCells.add(Cell);
	return flaggedCells;
}
```
> 단순히 이름만 변경했을 뿐인데 함수가 하는 일을 이해하기 쉬워졌다. 이게 좋은 이름이 주는 위력이다.

## 2. 그릇된 정보를 피하라
- 널리 쓰이는 의미가 있는 단어들을 다른 의미로 사용하면 안된다.


## 3. 의미 있게 구분하라
- 읽는 사람이 차이를 알도록 이름을 지어라

```java
ex) 차이를 알 수 없는 아래와 같은 네이밍은 피하도록 하자
1. moneyAmount - money
2. customerInfo - customer
3. accountData - account
4. theMessage - message

5. getActiveAccount() - getActiveAccounts() - getActiveAccountInfo()
```

## 4. 발음하기 쉬운 이름을 사용하라
- 너무 함축적으로 네이밍을 지으면 대화할 때 어려우니, 길더라도 발음하기 쉽게 풀어쓰자.


## 5. 검색하기 쉬운 이름을 사용하라

- ex1로 구성되어 있는 단어를 가지고 검색하면 정말 많이 검색될 것이다. 하지만 2번과 같이 구성되어 있는 변수를 검색한다면 몇 개 내외로 찾을 수 있을 것이다. 그러니 검색하기 쉬운 이름을 사용하려고 노력해보자.
```java
# ex 1.
for (int j = 0; j < 34; j++){
	s += (t[j] * 4) / 5;
}


# ex 2.
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j = 0; j< NUMBER_OF_TASKS; j++){
	int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
	int realTaskWeeks = (realTaskDays / WORK_DAYS_PER_WEEK);
	sum += realTaskWeeks;
}
```

## 6. 인코딩을 피하라
- 헝가리식 표기법 <br>
  👉 당시 컴파일러가 타입을 점검하지 않았기 때문에 타입을 기억할 단서가 필요했고, 지금은 그렇지 않으니 변수 이름에 타입을 인코딩 하지 말자


- 멤버 변수 접두어 <br>
  👉 접두어는 피하자


- 인터페이스 클래스와 구현 클래스 <br>
👉 ??


## 7. 자신의 기억력을 자랑하지 마라
- 문자 하나만 사용하는 변수 이름(ex). i, j, k)은 루프에서 반복 횟수를 세는 정도로만 사용하자

## 8. 클래스 이름
- 명사나 명사구로 짓고, 동사는 피하자

```java
좋은 ex) Customer, WikiPage, Account, AddressParser
나쁜 ex) Manager, Processor, Data, Info
```

## 9. 메서드 이름
- 동사나 동사구가 적합하다.

```java
좋은 ex) postPayment, deletePage, save 

접근자, 변경자, 조건자는 javabean 표준에 따라 값 앞에 get, set, is를 붙인다.
ex) getName, setName, isPosted
```

## 10. 기발한 이름은 피하라
- 재미난 이름보다 명료한 이름을 선택해라


## 11. 한 개념에 한 단어를 사용하라
- 일관성 있는 어휘는 코드를 사용할 프로그래머가 반갑게 여길 선물이다.

## 12. 말장난을 하지 마라
- 한 단어를 두 가지 목적으로 사용하지 말자.

```java
ex) add
보통 add는 두 값을 더하는 함수라고 생각하는 것이 일반적인데, 어떤 집합에 값 하나를 추가하는 것도 add라고 하면 안된다.
기존 add 메소드와 맥락이 다르고 일관성이 어긋나므로, 후자와 같은 경우 의미가 더 가까운 insert, append 등의 이름을 사용하자.
```

## 13. 해법 영역에서 가져온 이름을 사용하라
- 해법 영역(Solution Domain) : 개발자라면 당연히 알고 있을 전산용어, 알고리즘 이름, 패턴 이름, 수학 용어 등을 사용하자.

## 14. 문제 영역에서 가져온 이름을 사용하라
- 문제 영역(Problem Domain) : 실제 도메인의 전문가에게 물어 파악할 수 있도록 문제 영역에서 이름을 가져오자.

## 15. 의미 있는 맥락을 추가하라
- 클래스, 함수, 이름 공간에 넣어 맥락을 부여하고, 모든 방법이 실패하면 마지막 수단으로 접두어를 붙이자.

```java
# 변수를 훑어보면 주소와 관련 있음을 알 수 있지만, 하나만 덜렁 있으면 알 수 없다.
String firstName, lastName, street, houseNumber, city, state, zipcode

# addr라는 접두어를 추가해 맥락을 명확하게 해주자.
addrFirstName, addrLastName, addrState ...

# Address라는 클래스를 만들어 의미 부여해보자.
class Address{
  String addrFirstName;
  String addrLastName;
  String addrStreet;
  String addrHouseNumber;
  String addrCity;
  String addrState;
  String addrZipcode
}
```


## 16. 불필요한 맥락을 없애라
- 의미가 분명하다면 일반적으로 짧은 이름일수록 더 좋다.