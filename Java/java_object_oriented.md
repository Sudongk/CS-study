## 객체 지향 프로그래밍(Object-Oriented Programming, OOP)
- `정의` : 프로그래밍에서 필요한 데이터를 추상화시켜, **상태와 행위를 가진 객체**로 만들고, 객체들의 유기적인 결합과 협력으로 파악하고자 하는 컴퓨터 프로그래밍의 패러다임을 의미

### 객체 지향 프로그래밍의 장점
- 프로그램을 유연하고 변경 맟 유지보수가 용이하도록 만들 수 있다.
- 가독성이 좋고 직관적인 코드 작성에 용이하다.

## 객체(Object)
- `정의` : 객체 지향 프로그래밍의 가장 기본적인 단위이자 시작점

객체 지향 프로그래밍에서는 객체를 추상화시켜 속성(attribute)과 기능(behavior)을 정의하여 각각 변수(variable)와 함수(function)으로 정의합니다.

```
class TV {
    // 속성 정의
    private String brand;
    private String model;
    private int screenSize;
    private boolean isSmartTV;
    private String displayType;

    // 생성자
    public TV(String brand, String model, int screenSize, boolean isSmartTV, String displayType) {
        this.brand = brand;
        this.model = model;
        this.screenSize = screenSize;
        this.isSmartTV = isSmartTV;
        this.displayType = displayType;
    }

    // 기능(행동) 정의
    public void powerOn() {
        System.out.println("TV 전원을 켭니다.");
    }

    public void changeChannel(int channel) {
        System.out.println("채널을 " + channel + "번으로 변경합니다.");
    }

    public void adjustVolume(int volumeLevel) {
        System.out.println("볼륨을 " + volumeLevel + "로 조절합니다.");
    }

    public void connectToInternet() {
        if (isSmartTV) {
            System.out.println("인터넷에 연결합니다.");
        } else {
            System.out.println("이 TV는 인터넷 연결을 지원하지 않습니다.");
        }
    }
}
```

### 객체 지향 프로그래밍의 4가지 특성
객체 지향 프로그래밍을 통해 소프트웨어를 개발하면 코드를 재사용함으로써 반복적인 코드 최소화, 유연하고 변경에 용이한 프로그램 개발이 가능하다.

객체 지향 프로그래밍의 4가지 특징

### [ 추상화(Abstraction) ]
- `정의` : 객체의 공통적인 속성과 기능을 추출하여 정의하는 것

```
public interface Phone {
    public abstract void start()
    void takePicture();
    void makeCall();
}

public class Galaxy implements Phone {
    @Override
    public void takePicture() {
        System.out.println("갤럭시로 사진을 찍습니다");
    }

    @Override
    public void makeCall() {
        System.out.println("갤럭시로 전화를 겁니다");
    }
}

public class IPhone implements Phone {
    @Override
    public void takePicture() {
        System.out.println("아이폰으로 사진을 찍습니다");
    }

    @Override
    public void makeCall() {
        System.out.println("아이폰으로 전화를 겁니다");
    }
}
```

### [ 상속(Inheritance) ]
- `정의` : 기존의 클래스를 재활용하여 새로운 클래스를 작성하는 자바 문법 요소

```
//추상화를 통한 상위클래스 정의
public class Phone { 
    protected String model;
    protected String color;
    protected int size;

    void takePicture() {
        System.out.println("사진을 찍습니다");
    }

    void makeCall() {
        System.out.println("전화를 겁니다");
    }
}

public class Galaxy extends Phone {

    boolean isRecordingSupportive;

    void samsungPay() {
        System.out.println("삼성 페이로 지불합니다");
    }
}

public class IPhone extends Phone {

    boolean isICloudSupportive;

    //메서드 오버라이딩 -> 기능 재정의
    @Override
    void takePicture() {
        System.out.println("아이폰으로 사진을 찍습니다");
    }


    void applePay() {
        System.out.println("애플 페이로 지불합니다");
    }
}
```

### [ 다형성(Polymorphism) ]
- `정의` : 어떤 객체의 속성이나 기능이 상황에 따라 여러 가지 형태를 가질 수 있는 성질
- `예시` : 상위 클래스 타입의 참조변수로 하위 클래스 객체 참조, 메서드 오버라이딩, 메서드 오버로딩
  > 다형성이란 한 타입의 참조변수를 통하여 여러 타입의 객체를 참조할 수 있도록 만든 것을 의미  


```
public interface Phone {
    void takePicture();
    void makeCall();
}

public Galaxy implements Phone {
    @Override
    public void takePicture() {
        System.out.println("갤럭시로 사진을 찍습니다");
    }

    @Override
    public void makeCall() {
        System.out.println("갤럭시로 전화를 겁니다");
    }
}

public class IPhone extends Phone {
    @Override
    void takePicture() {
        System.out.println("아이폰으로 사진을 찍습니다");
    }

    @Override
    public void makeCall() {
        System.out.println("아이폰으로 전화를 겁니다");
    }
}

public class Main {
    public static void main(String[] args) {
        // 원래 사용했던 객체 생성 방식
        Phone phone = new Phone();
        Galaxy galaxy = new Galaxy();

        // 다형성을 활용한 객체 생성 방식
        Phone galaxy = new Galaxy();
        Phone iphone = new IPhone();
    }
}
```
기존 방식은 하위 클래스 객체를 생성하여 하위 클래스 타입의 참조변수에 할당하였지만, 다형성을 활용한 객체 생성 방식은 하위 클래스 객체를 생성하여 상위 클래스 타입의 참조변수에 할당

```
public class Client {
    void usePhone(Phone phone) {
        phone.takePicture();
        phone.makeCall();
    }
}

public class Main {
    public static void main(String[] args) {
        Client client1 = new Client();
        Client client2 = new Client();
        Galaxy galaxy = new Galaxy();
        IPhone iphone = new Iphone();

        client1.usePhone(galaxy);
        client2.usePhone(iphone);  
    }
}
```
인터페이스를 통해 간접적으로 연결하였기에 `결합도가 낮아졌다`

### [ 캡슐화(Encapsulation) ]
- `정의` : 클래스 안에 서로 연관있는 속성과 기능들을 하나의 캡슐로 만들어 외부로부터 보호하는 것을 의미
- 데이터 보호 : 외부로부터 클래스에 정의된 속성과 기능들을 보호
- 데이터 은닉 : 내부의 동작을 감추고 외부에는 필요한 부분만 노출

