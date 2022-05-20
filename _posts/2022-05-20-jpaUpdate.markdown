---
layout: post
title:  "영속성과 MapStruct를 이용한 JPA UPDATE 설정"
date:   2022-05-20 18:58 +0900
categories: JPA
---

### 들어가며
이번에 데이터를 UPDATE 하는 기능을 만들다가 JpaRepository에서 제공해주는 SAVE 함수를 사용해봤다. <br>
그런데 문제는 변경된 데이터는 잘들어가는데 나머지가 null 처리가 되는 것이다.<br>
그래서 찾아보니 보통 getById함수를 사용해서 Entity 객체에 영속성을 이용해서 해당 값을 set 해주는 것이다.<br>
그런데 나는 이게 불편하다고 느꼈던게 update 문이 기능 별로 새로 만들어야한다는게 생산성에서 저하된다고 느껴 새로운 방법을 찾아봤다.<br>
그래서 그 방법을 소개한다.
MapStruct 설정은 여길 <a href="https://kimsiyong95.github.io/jpa/2022/05/16/jpaMapStruct.html">참고</a>하면된다. 

### MapStruct Interface
BeanMapping에 nullValuePropertyMappingStrategy = NullValuePropertyMappingStrategy.IGNORE 설정으로 인해<br>
Dto가 Entity로 변환할때 null이 아닌 값 들만 변환 시킨다.
```
@BeanMapping(nullValuePropertyMappingStrategy = NullValuePropertyMappingStrategy.IGNORE)
RegistEntity updateRegistFromDto(RegistDTO dto, @MappingTarget RegistEntity entity);
```

###  build 시
위에 interface에 updateRegistFromDto를 선언해놓고 build 를 하면 <br>
아래와 같이 자동으로 구현체가 만들어진다. 구현체를 확인하고싶으면 build 하위 폴더에서 확인이 가능하다.

```
public RegistEntity updateRegistFromDto(RegistDTO dto, RegistEntity entity) {
    if (dto == null) {
        return null;
    } else {
        if (dto.getRegistId() != null) {
            entity.setRegistId(dto.getRegistId());
        }

        if (dto.getStoreCode() != null) {
            entity.setStoreCode(dto.getStoreCode());
        }

        if (dto.getDetailCode() != null) {
            entity.setDetailCode(dto.getDetailCode());
        }

        if (dto.getStrtDt() != null) {
            entity.setStrtDt(dto.getStrtDt());
        }

        ...
        ...
        ...

        return entity;
    }
}
```


### Service 단에서 사용하는 곳
먼저 @Transactional를 선언해주는 이유는 getById로 Entity 영속성을 연결해놓은 상태에서 <br>
updateRegistFromDto 함수를 통해 registEntity 값이 바뀌고 메소드가 종료될때 자동으로 db에 저장하기위해 선언해놓는다.<br>
앞으로는 하나하나 set,get를 사용해서 기능 별로 update문을 만들지 않아도 되고 저 문법 하나로 다른 update 문을 통일시킬수있다. <br>

```
@Transactional
public void registSave(RegistDTO registDTO){
    RegistEntity registEntity = registRepository.getById(registDTO.getRegistId());
    registMapStructMapper.updateRegistFromDto(registDTO, registEntity);

}
```
### 마무리하며
저 문법만으로도 update를 통일 시킬 수 있다는게 뿌듯했고, 앞으로도 생산성을 높이는 코드를 <br>
짜려고 노력할 것 이다. 계속해서 성장하는 개발자가 되고싶다.
