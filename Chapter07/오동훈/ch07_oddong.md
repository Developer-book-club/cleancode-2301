

# 7장 오류 처리
    
## 1. 오류 코드보다 예외를 사용하라

- 호출 즉시 에러 처리를 하지 않기 때문에 코드가 길어지고 명확하게 이해하기가 어려워진다.

```java
# 예외처리 하지 않은 Case

public class DeviceController {
    public void sendShutDown() {
        DeviceHandle handle = getHandle(DEV1);
        // 디바이스 상태를 점검한다.
        if (handle != DeviceHandle.INVALID) {
            // 레코드 필드에 디바이스 상태를 저장한다.
            retrieveDeviceRecord(handle);
            //디바이스가 일시정지 상태가 아니라면 종료한다.
            if (record.getStatus() != DEVICE_SUSPENDED) {
                pauseDevice(handle);
                clearDeviceWorkQueue(handle);
                closeDevice(handel);
            } else {
                logger.log("Device suspended. Unable to shut down");
            }
        } else {
            logger.log("Invalid handle for: " + DEV1.toString());
        }
    }
}
```

```java
#  예외처리 한 Case

public class DeviceController {
    public void sendShutDown() {
        try {
            tryToShutDown();
        } catch (DeviceShutDownError e) {
            logger.log(e);
        }
    }
 
    private void tryToShutDown() throws DeviceShutDownError {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);
 
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
    }
 
    private DeviceHandle getHandle(DeviceId id) {
        ...    	
        throw new DeviceShutDownError("Invalid handle for:" + id.toString());
        ...
    }
}
```

## 2. Try-Catch-Finally 문부터 작성하라   
- 예외가 발생할 코드를 짜는 경우엔 `try-catch-finally`문으로 시작하는게 좋다.


## 3. 미확인unchecked 예외를 사용하라    


## 4. 예외에 의미를 제공하라
- 예외를 던질 때는 전후 상황을 충분히 덧붙인다.
- 오류 메세지에 정보를 담아 예외와 함께 던지고, 실패한 연산 이름과 실패 유형도 언급해준다.

Java를 사용해보지 않아 IllegalArgumentException를 처음봤는데, 해당 함수를 사용하면서 안에 에러 메세지를 같이 넘겨주는 방식으로 사용하면 어디서 발생한 에러인지 판단하기 쉽기 때문에 아래와 같이 사용한다고 합니당

```java
throw new IllegalArgumentException();

throw new InvalidSearchArgumentException("검색조건은 빈칸일 수 없습니다.");
```

## 5. 호출자를 고려해 예외 클래스를 정의하라


## 6. 정상 흐름을 정의하라
- 예외가 발생하는 특수상황자체가 없도록 구현을 하자.

```java
try {
	MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
	m_total += expenses.getTotal();
} catch (MealExpensesNotFound e) {
	m_total += getMealPerDiem();
}
```
위와 같이 `getMeals` 메소드에서 예외가 발생하지 않았다면 반환된 인스턴스에서 `getTotal()`을 호출하여 더하고 만약 `getMeal`에서 이러가 `MealExpensesNotFound`예외가 발생한다면 `getMealPerDien()` 메소드를 호출해 반환된 값을 더해주는데, 차라리 특수한 예외상황 자체가 안나도록 만들어주면 코드가 더 간결해질 수 있다. 


## 7. null을 반환하지 마라    
## 8. null을 전달하지 마라