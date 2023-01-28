# Chapter 7. 오류 처리
    
#### 오류 코드보다 예외를 사용하라
- 오류가 발생하면 예외를 던지는 게 낫다.
- 비즈니스 로직과 오류 처리하는 알고리즘이 분리된다.
```java
public class DeviceController()
{  
    public void sendShutDown()
    {
        try
        {
            tryToShutDown();
        }
        catch(DeviceShutDownError e)
        {
            logger.log(e);
        }
    }
    
    private void tryToShutDown() throws DeviceShutDownError
    {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);
        
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
    }
    
    private DeviecHandle getHandle(DeviceID id)
    {
        throw new DeviceShutDownError("Invalid handle for: " +  id.toString());
    }
}
```
#### Try-Catch-Finally 문부터 작성하라
- 예외가 발생할 코드를 짤 때는 try-catch-finally 문으로 시작하는 편이 낫다.
- catch 블록에서 예외 유형을 좁히자.
```java
try
{

}
catch(Exception e)
{

}

try
{

}
catch(FileNotFoundException e)
{

}
```
- try-catch 구조로 범위를 정의한 후 TDD를 사용해 필요한 나머지 논리를 추가한다.
#### 미확인unchecked 예외를 사용하라
- 하위 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 전부 고쳐야 한다. </br>
최하위 함수를 변경해 새로운 오류를 던진다고 가정하자. 확인된 오류를 던진다면 함수는 선언부에 throws절을 추가해야 한다. </br>
그러면 변경한 함수를 호출하는 모든 함수가 </br>
1. catch 블록에서 새로운 예외를 처리하거나 </br>
2. 선언부에 throw절을 추가해야 한다. </br>
결과적으로 throws 경로에 위치하는 모든 함수가 최하위 함수에서 던지는 예외를 알아야 하므로 캡슐화가 깨진다.
#### 예외에 의미를 제공하라
- 오류 메시지에 정보를 담아 예외와 함께 던진다. 실패한 연산 이름과 실패 유형도 언급한다.
- 애플리케이션이 로깅 기능을 사용한다면 catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨준다.
#### 호출자를 고려해 예외 클래스를 정의하라
- 외부 API를 사용할 때는 Wrapper Class가 최선이다. 외부 API를 감싸면 외부 라이브러리와 프로그램 사이에서 의존성이 크게 줄어든다. 나중에 다른 라이브러리로 갈아타도 비용이 적다.
- Wrapper Class에서 외부 API를 호출하는 대신 테스트 코드를 넣어주는 방법으로 프로그램을 테스트하기도 쉬워진다.
- Wrapper Class를 사용하면 특정 업체가 API를 설계한 방식에 발목 잡히지 않는다. 프로그램이 사용하기 편리한 API를 정의하면 그만이다.
```java
// LocalPort 클래스는 ACMEPort 클래스가 던지는 예외를 잡아 변환하는 Wrapper Class
public class LocalPort
{
    private ACMEPort innerPort;
    
    public LocalPort(int portNumber)
    {
        innerPort = new ACMEPort(portNumber);
    }
    
    public void open()
    {
        try
        {
            innerPort.open();
        }
        catch(DeviceResponseException e)
        {
            throw new PortDeviceFailure(e);
        }
        catch(ATMUnlockedException e)
        {
            throw new PortDeviceFailure(e);
        }
    }
}
```
#### 정상 흐름을 정의하라
- 특수 사례 패턴(Special Case Pattern): 클래스를 만들거나 객체를 조작해 특수 사례를 처리하는 방식.
- 클래스나 객체가 예외적인 상황을 캡슐화해서 처리하므로 클라이언트 코드가 예외적인 상황을 처리할 필요가 없어진다.
#### null을 반환하지 마라
- 메서드에서 null을 반환해야 한다면 대신 예외를 던지거나 특수 사례 객체를 반환한다.
- 사용하려는 외부 API가 null을 반환한다면 Wrapper Method를 구현해 예외를 던지거나 특수 사례 객체를 반환하는 방식을 고려한다.
- null 대신 empty object를 return 하는 게 낫다.
#### null을 전달하지 마라
- 새로운 예외 유형을 만들어 던지는 방법이 있다.
- assert문을 사용하는 방법도 있다.
- 파라미터로 null이 넘어오면 코드에 문제가 있다는 뜻이다.
#### 결론
- 비즈니스 로직과 오류 처리 알고리즘을 분리하라.
