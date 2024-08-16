### Garbage Colletction
- Garbage Colletction이란, 말 그대로 쓰레기를 모으는 기능이다. Heap 메모리 영역에서 동적으로 할당된 객체 중에 불필요한 객체를
자동으로 해제하는 기능을 말한다.
- 메모리 누수를 방지할 수 있다.

#### 동작 방식
- Mark and Sweep
##### Mark
- Root set(Stack 영역의 지역변수/파라미터, Method 영역의 정적 변수, JNI에 의해 생성된 객체)과 참조 관계가 있는(Reachable) 객체를
찾는 과정이다. 관계가 없으면(Unreachable)

##### Sweep
- Mark 단계에서 살이있는 객체로 표시되지 않으면(Unreachable) 가비지로 간주되어 해당 객체에 할당된 메모리를 해제한다.


#### JVM의 메모리 영역
![image](https://github.com/user-attachments/assets/da172079-2347-42b7-b382-84c97f8eac07)
- 자바는 메모리 영역을 구분하여 사용한다.
- 효율적인 처리가 가능하다. 영역을 나눔으로써 일부 메모리 영역만 스캔할 수 있다. 이러면 GC 비용을 줄일 수 있다.
- Permanent 영역에는 Class 의 Meta정보, Method의 Meta 정보, Class와 관련된 배열 객체 Meta 정보, Static 객체, 상수화된 String 객체, JVM 내부적인 객체들과 JIT의 최적화 정보
- **그런데!!** Java 8에서는 Permanent 영역을 없애버렸다!(Permanent Gen Space Error)

#### Garbage Colletction이 발생하는 과정
- Eden: 새로운 객체가 저장되는 영역
- Survivor: Eden에서 살아남아 이동된 객체의 영역
- Eden 영역이 다 차면 Young Generation에서 발생하는 Minor GC가 발생한다. 이 때 Mark and Swee 과정이 발생하고 살아남은 객체는  
Survivor 0 객체로 옮겨진다. Age도 함께 증가한다.(Stop and Copy)
- Minor GC는 작업이 완료될 때까지 작업을 제외한 모든 스레드를 일시정지한다!
- Survivor 0에 객체가 있는 경우 다음 Minor GC 후에 Survior 0으로 이동하는 것이 아닌라 Survivor 0에 있는 객체와 함께 Survivor 1로 옮겨진다.(메모리 단편화 방지를 위해)
- 반복 되다가 증가한 age가 특정 값 이상(JVM 디폴트는 31)이 되면 객체를 Old Generation으로 옮긴다.
- Old Generation 영역이 다 차게 되면 Major GC가 발생한다.
- Major GC은 시간이 오래 걸리고 실행 중에 프로세스가 정지된다.(Stop thr world) 즉, GC를 실행하는 스레드를 제외한 나머지 스레드는 작업을 멈춘다.
-
- #### Garbage Collection은요
- 자동으로 메모리 관리가 가능하다: JVM이 알아서 관리해준다.
- 메모리 누수를 방지할 수 있다: 사용하지 않는 객체를 메모리 영역에서 해제함으로써 누수를 방지할 수 있다.
