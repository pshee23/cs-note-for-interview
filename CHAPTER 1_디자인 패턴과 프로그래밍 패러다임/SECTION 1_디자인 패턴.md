# SECTION 1 디자인 패턴

- 목표
    - 라이브러리, 프레임워크의 기본이 되는 디자인 패턴을 공부
    - 어떠한 방식으로 로직을 구성해야 하는지에 대한 시각이 담겨있는 프로그래밍 패러다임 공부

- 용어
    - 라이브러리 :
        - 공통으로 사용될 수 있는 특정한 기능들을 모듈화 한 것.
        - 폴더명, 파일명 등에 대한 규칙이 없고 프레임워크에 비해 자유롭다.
        - '도구'인 라이브러리를 사용해서 '내가' 직접 컨트롤해서 사용
    - 프레임워크 : 
        - 공통으로 사용될 수 있는 특정한 기능들을 모듈화 한 것.
        - 폴더명, 파일명 등에 대한 규칙이 있고 라이브러리에 비해 좀 더 엄격하다.
        - '도구'인 프레임워크를 사용하지만 '프레임워크'가 컨트롤


<br/>

## 1.1 디자인 패턴

- 디자인 패턴이란
    <br/> 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약' 형태로 만들어 놓은 것


    <br/>

    ### 1.1.1 싱글톤 패턴 (Singleton Pattern)

    - 싱글톤 패턴이란
        - 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴.
        - 보통 데이터베이스 연결 모듈에 많이 사용됨
    
    - 특징
        - 하나의 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스 생성 비용이 줄어듬
        - 하지만 의존성이 높아짐

    - 코드
        ```
        // 하나의 인스턴스를 가지는 Singleton 클래스
        class Singleton {
            private static class singleInstanceHolder {
                private static final Singleton INSTANCE = new Singleton();
            }

            public static Singleton getInstance() {
                return singleInstanceHolder.INSTANCE;
            }
        }

        public class Test {
            public static void main(String[] args) {
                Singleton a = Singleton.getInstance();
                Singleton b = Singleton.getInstance();
                if(a == b) {
                    System.out.println(true);
                }
            }
        }
        // result : true
        // a와 b의 Instance number는 동일
        ```
    - 싱글톤 패턴의 단점
        - TDD(Test Driven Development) 할 때 걸림돌이 됨. 
            - TDD시 단위 테스트를 주로 하며 테스트가 서로 독립적이여야 어떤 순서로든 실행할 수 있어야 한다.
            - 하지만 싱글톤 패턴인 경우 미리 생성된 하나의 인스턴스를 기반으로 구현되므로 각 테스트 마다 '독립적'인 인스턴스를 만들기가 어렵다.
        - 의존성 주입
            - 모듈간의 결합을 강하게 만들 수 있는 단점이 있다. 
            - 이때 의존성 주입(DI; Dependency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결 할 수 있다. 
            - 의존성이란; 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 된다는것을 의미
            - 메인 모듈이 직접 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자(dependency injector)가 이 부분을 가로채 메인 모듈이 간접적으로 의존성을 주입하는 방식
            - 이를 통해 메인 모듈(상위 모듈)은 하위 모듈에 대한 의존성이 떨어지게 된다; 디커플링이 된다 
            - 장점
                - 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅, 마이그레이션이 쉬움
                - 구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어주기 때문에 애플리케이션 의존성 방향이 일관되고, 애플리케이션을 쉽게 추론할 수 있으며, 모듈 간의 관계들이 조금 더 명확해짐
            - 단점
                - 모듈들이 더 분리되므로 클래스 수가 늘어 복잡성 증가
                - 약간의 런타임 패널티
            - 의존성 주입 원칙
                - 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
                - 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다.

    <br/>
    <br/>

    ### 1.1.2 팩토리 패턴 (Factory Pattern)
    - 팩토리 패턴이란 : 
        - 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴
        - 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 
            하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴
        - 특징 : 
            - 느슨한 결합 : 상위와 하위 클래스가 분리되기 때문
            - 유연성 : 상위 클래스에서는 인스턴스 생성 방식에 대해 알필요가 없기 때문
            - 유지 보수성 증가 : 객체 생성 로직이 따로 떼어져 있기 때문에 코드 리팩터링시 한 곳만 고칠 수 있음
        - 코드 : 
            ```
            abstract class Coffee {
                public abstract int getPrice();

                @Override
                public String toString() {
                    return "Hi this coffee is " + this.getPrice();
                }
            }
            // 상위 클래스 : CoffeFactory : 중요한 뼈대 결정
            // static로 getCoffee정의 : 정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능
            // 메모리를 절약할 수 있으며, 개별 인스턴스에 묶이지 않고, 클래스 내의 함수를 정의할 수 있는 장점이 있음
            class CoffeFactory {
                public static Coffee getCoffee(String type, int price) {
                    if("Latte".equalsIgnoreCase(type)) 
                        return new Latte(price);
                    else if ("Americano".equalsIgnoreCase(type))
                        return new Americano(price);
                    else
                        return new DefaultCoffee();
                }
            }
            // 하위 클래스들 : DefaultCoffee, Latte, Americano
            // 구체적인 내용을 결정
            class DefaultCoffee extends Coffee {
                private int price;

                public DefaultCoffee() {
                    this.price = -1;
                }
                @Override
                public int getPrice() {
                    return this.price;
                }
            }
            
            class Latte extends Coffee {
                private int price;

                public Latte(int price) {
                    this.price = price;
                }
                @Override
                public int getPrice() {
                    return this.price;
                }
            }

            class Americano extends Coffee {
                private int price;

                public Americano(int price) {
                    this.price = price;
                }
                @Override
                public int getPrice() {
                    return this.price;
                }
            }

            public class HelloWorld {
                public static void main(String[] args) {
                    Coffee latte = CoffeFactory.getCoffee("Latte", 4000);
                    Coffee ame = CoffeFactory.getCoffee("Americano", 3000);

                    System.out.println("Factory latte ::" + latte);
                    System.out.println("Factory ame ::" + ame);
                }
            }
            ```
    <br/>
    <br/>

    ### 1.1.3 전략 패턴 (Strategy Pattern)
    - 전략 패턴이란: 
        - 정책 패턴(Policy Pattern)이라고도 함
        - 객체의 행위를 바꾸고 싶은 경우 '직접' 수정하지 않고 전략이라고 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체하는 패턴
        - 예시) 물건을 구매할때 네이버페이, 카카오페이 등의 다양한 결제 방법. 결제 방식의 '전략'만 바꾼다
        - 예시기반 코드
            ```
            interface PaymentStategy {
                public void pay(int amount);
            }

            class KAKAOCardStrategy implements PaymentStrategy {
                private String name;
                private String cardNumber;
                private String cvv;
                private String dateOfExpiry;

                public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate) {
                    this.name = nm;
                    this.cardNumber = ccNum;
                    this.cvv = cvv;
                    this.dateOfExpiry = expiryDate;
                }
                @Override
                public void pay(int amount) {
                    System.out.println(amount + " paid using KAKAOCard.");
                }
            }

            class LUNACardStrategy implements PaymentStrategy {
                private String emailId;
                private String password;

                public LUNACardStrategy(String email, String pwd) {
                    this.emailId = email;
                    this.password = pwd;
                }
                @Override
                public void pay(int amount) {
                    System.out.println(amount + " paid using LUNACard.");
                }
            }

            class Item {
                private String name;
                private int price;
                public Item(String name, int cost) {
                    this.name = name;
                    this.price = cost;
                }
                public String getName() { return name; }
                public int getPrice() { return price; }
            }

            class ShoppingCart{
                List<Item> items;
                public ShoppingCart() { this.items = new ArrayList<Item>(); }
                public void addItem(Item item) {
                    this.items.add(item);
                }
                public void removeItem(Item item) {
                    this.items.remove(item);
                }
                public int calculateTotal() {
                    int sum = 0;
                    for(Item item : items) {
                        sum += item.getPrice();
                    }
                    return sum;
                }
                public void pay(PaymentStrategy paymentMethod) {
                    int amount = calculateTotal();
                    paymentMethod.pay(amount);
                }
            }

            public class HelloWorld {
                public static void main(String[] args) {
                    ShoppingCart cart = new ShoppingCart();
                    Item A = new Item("kundolA", 100);
                    Item B = new Item("kundolB", 300);
                    cart.addItem(A);
                    cart.addItem(B);
                    // pay by LUNACard
                    cart.pay(new LUNACardStrategy("kindol@example.com", "pukubababo"));
                    // pay by KAKAOCard
                    cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789", "123", "12/01"));
                }
            }
            /*
            400 paid using LUNACard.
            400 paid using KAKAOCard.
            */
            ```
            - 쇼핑 카트에 아이템을 담아 LUNACard 또는 KAKAOCard라는 두 개의 전략으로 결제하는 코드
            
            <br/>

            > 컨텍스트 :<br/> 
                프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미. <br/>
                개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말함

            <br/>

            - passport
                - 전략 패턴을 활용한 라이브러리
                - Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리
                - 여러가지 '전략'을 기반으로 인증할 수 있음
                - 서비스 내 회원가입(아이디/비밀번호) 기반으로 인증하는 LocalStrategy, 페이스북, 네이버 등 다른 서비스를 기반으로 인증하는 OAuth 전략
        
    <br/>
    <br/>

    ### 1.1.4 옵저버 패턴 (Observer Pattern)
    - 옵저버 패턴이란 :
        - 

        
            
