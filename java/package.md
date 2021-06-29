## 패키지 구조는 어떻게 구성하는 것이 좋을까?

* what

    layered architecture 기반 패키지 구조를 구성하는 데에는 크게 3가지 안이 존재한다.
    1. layer 우선  
       net.slipp.domain.modulename  
       net.slipp.service.modulename  
       net.slipp.dao.modulename  
       net.slipp.web.modulename  
    2. 모듈 우선  
       net.slipp.modulename.domain  
       net.slipp.modulename.service  
       net.slipp.modulename.dao  
       net.slipp.modulename.web  
    3. feature 우선  
       com.app.doctor  
       com.app.drug  
       com.app.patient  
       com.app.presription  
       com.app.report  
       com.app.security  
       com.app.webmaster  
       com.app.util  
       
* opinion  
    layer 우선으로 패키지를 구성하면 도메인 위주 개발에 더 알맞은 패키지 구조를 구성할 수 있으며, 중복된 부분을 제거하는 것이 용이하다.
    module 우선으로 패키지를 구성하면 모듈 단위로 분리할 때 유리하지만 패키지 간 순환 참조가 발생할 수 있으며 각 모듈에 중복 코드가 발생할 가능성이 있다.
    feature 우선으로 패키지를 구성하면 기능 단위 개발 시 유리하고, 기능에 따른 dao, domain 등이 하나의 패키지에 속해있기 때문에 구조 파악이 용이하다.
    
    모듈 우선으로 개발하는 것이 현재는 편리하지만, 기능 단위로 패키지를 구성하는 것 또한 여럿이서 개발할 때에 용이해보인다.

### 참고 자료
- [패키지 구조는 어떻게 가져가는 것이 좋을까?](https://www.slipp.net/questions/36)
- [Package by feature, not layer](http://www.javapractices.com/topic/TopicAction.do?Id=205)
