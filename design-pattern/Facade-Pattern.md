Facade Pattern
* what
  Facade Pattern이란 어떠한 서브 시스템의 일련의 인터페이스에 대한 통합된 인터페이스를 제공하는 방법이다. 간단한 예를 들자면, 퍼사드는 건물의 정면을 의미하는데, 하위 시스템들에 대해 건물의 입구를 제공하여 하위 시스템들로 접근하기 위해 건물의 입구를 통과하도록 설계하는 것과 같다.

* when
  ![](Facade01-1024x613.png)
  위 처럼, 여러 클라이언트가 다양한 서비스들을 한꺼번에 필요로 하는 경우가 있다고 가정하자. 이 때, 우리는 각각의 클라이언트들의 요청에 알맞은 서비스를 매핑해주는 것이 아니라, 각 서비스들의 상위 관문인 퍼사드 인터페이스를 통해 요청하도록 하고, 자세한 비즈니스 로직은 퍼사드를 통해 구현되도록 다음과 같이 변경할 수 있다.

![](Facade02-1024x613.png)

요약하자면, 퍼사드는 클라이언트의 요청을 적절한 하위 클래스의 시스템에 위임하는 역할, 하위 시스템 클래스는 하위 시스템의 기능을 구현하여 적절한 비즈니스 로직을 취하도록 하는 역할, 그리고 클라이언트는 어떠한 작업을 수행하기 위해 퍼사드에게 요청하는 역할을 수행한다.

* 학습로그에 퍼사드를 포함한 이유?
  이번 미션에서는 100% 퍼사드 패턴을 만족한 구현 방식으로 서비스 - 다오 구조를 구현한 것은 아니었다. 단순히 서비스와 dao가 1:1 매핑이 되도록, 서비스가 다른 서비스를 참조하지 않도록 구성하기 위해 서비스들의 상위 역할을 하는 클래스를 분리하고, 해당 상위 클래스를 서비스들의 조합으로 나타냄으로서 구현하였다.

하지만 이러한 방법에 유사한 패턴으로서 퍼사드가 있다는 것을 알게 되었고, 이를 학습로그에 작성하게 되었다.

* how
```
// order controller
public class OrderFulfillmentController {
    OrderServiceFacade facade;
    boolean orderFulfilled=false;
    public void orderProduct(int productId) {
        orderFulfilled=facade.placeOrder(productId);
        System.out.println("OrderFulfillmentController: Order fulfillment completed. ");
    }
}

// order facade
public interface OrderServiceFacade {
    boolean placeOrder(int productId);
}

public class OrderServiceFacadeImpl implements OrderServiceFacade{
    public boolean placeOrder(int pId){
        boolean orderFulfilled=false;
        Product product=new Product();
        product.productId=pId;
        if(InventoryService.isAvailable(product))
        {
            System.out.println("Product with ID: "+ product.productId+" is available.");
            boolean paymentConfirmed= PaymentService.makePayment();
            if(paymentConfirmed){
                System.out.println("Payment confirmed...");
                ShippingService.shipProduct(product);
                System.out.println("Product shipped...");
                orderFulfilled=true;
            }
        }
        return orderFulfilled;
    }
}
```

### 참고 자료
* [Facade Pattern - Spring Framework Guru](https://springframework.guru/gang-of-four-design-patterns/facade-pattern/)