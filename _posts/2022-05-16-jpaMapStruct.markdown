---
layout: post
title:  "JPA MapStruct (DTO <-> Entity) 설정"
date:   2022-05-16 19:56 +0900
categories: JPA
---

### 들어가며
이번에 Spring Cloud FrameWork 이용해서 msa-reservation (예약시스템 서비스)를 구축하게 되었습니다. <br>
평소에 mybatis를 이용해서 개발을 했다면 이번에는 JPA를 이용해서 개발을 진행을 했고<br>
그러는 도중 Entity를 이용하여 사용자에게 데이터를 response 하는게 아니라 dto로 변환해서 전달해야한다는 사실을 알아버렸습니다.<br>
Entity 같은 경우는 DB <-> Service을 중심으로 이용하고,  Service -> Controller는 DTO를 사용해야합니다.<br>
그래서 MapStruct에 대해 알아보겠습니다.<br>

### MapStruct란?
MapStruct는 Entity <-> DTO 간에 변환할 때 자동으로 매핑시켜 변환되도록 도와주는 라이브러리입니다. <br>
매핑해줄 클래스에는 setter가 있어야 하고 매핑이 되는 클래스에는 getter가 있어야 사용 가능합니다. <br>
MapStruct를 적용하기 전 ModelMapper 라는 걸 주로 사용했고, 역시 자동 매핑 해주는 라이브러입니다. <br>
하지만 MapStruct를 사용한 이유는 매핑속도가 빠르며, 컴파일 단계에서 에러확인이 가능하다는 장점이 있어서입니다. <br>

### 설정방법
msa-reservation은 gradle 환경입니다. gradle 기준으로 설명드리겠습니다. <br>

#### gradle dependencies 추가
```
compileOnly 'org.projectlombok:lombok'
annotationProcessor 'org.projectlombok:lombok'
testCompileOnly 'org.projectlombok:lombok'
testAnnotationProcessor 'org.projectlombok:lombok'
implementation 'org.mapstruct:mapstruct:1.4.2.Final'
annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.2.Final'
annotationProcessor 'org.projectlombok:lombok-mapstruct-binding:0.2.0'
```
#### EnterEntity 생성 
```
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import javax.persistence.*;
import java.sql.Timestamp;

@Getter
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table(name = "T_ENTER_H01")
public class EnterEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "ENTER_HIS_ID")
    private String enterHisId;

    @Column(name = "REGIST_ID")
    private String registId;

    @Column(name = "STORE_CODE")
    private String storeCode;

    @Column(name = "ENTER_ID")
    private String enterId;

    @Column(name = "ENTER_NIC")
    private String enterNic;

    @Column(name = "ENTER_EMAIL")
    private String enterEmail;

    @Column(name = "ENTER_DT")
    private Timestamp enterDt;

    @Column(name = "EXIT_DT")
    private Timestamp exitDt;

    @Column(name = "REGIST_DT")
    private Timestamp registDt;

    @Column(name = "UPDT_DT")
    private Timestamp updtDt;
}
```
#### EnterDTO 생성 
```
import lombok.*;

import java.sql.Timestamp;

@AllArgsConstructor
@Builder
@Data
public class EnterDTO {
    private String enterHisId;
    private String registId;
    private String storeCode;
    private String enterId;
    private String enterNic;
    private String enterEmail;
    private Timestamp enterDt;
    private Timestamp exitDt;
    private Timestamp registDt;
    private Timestamp updtDt;
}
```

#### GenericMapper 생성
모든 Entity <-> DTO 변환에 적용시킬 GenericMapper 인터페이스를 만들었고, <br>
지금 만들 ReservationMapStructMapper에서 상속받을 예정이다.
```
import java.util.List;

public interface GenericMapper <D, E>{
    D toDto(E e);
    E toEntity(D d);
    List<D> toDtoList(List<E> entityList);
    List<E> toEntityList(List<D> dtoList);
}
```
#### ReservationMapStructMapper 생성
이런식으로 extends GenericMapperd에 EnterDTO와 EnterEntity를 넣으면 따로 메소드를 작성 해주지않아도 된다.<br>
여기서 @Mapper을 선언해야 Mapstruct로 인식한다.
```
import org.mapstruct.Mapper;
import org.mapstruct.factory.Mappers;

@Mapper
public interface ReservationMapStructMapper extends GenericMapper<EnterDTO, EnterEntity> {
    ReservationMapStructMapper INSTANCE = Mappers.getMapper(ReservationMapStructMapper.class);
}
```

#### 실제 Service 단에서 사용
이렇게 repository에서 가져온 enterEntity 객체를 reservationMapStructMapper.toDto로 변환해서 return 해준다
```
public EnterDTO findByEnterHisId(String id){
    EnterEntity enter = enterRepository.findByEnterHisId(id);
    EnterDTO dto = reservationMapStructMapper.toDto(enter);
    return dto;
}
```

### 마치며
JPA는 알면 알수록 엄청 심오한것같습니다. 단순히 블로그나 강의만 듣고 가볍게 구현할 수 있는게 아니라고 생각이 되어
<a href="https://book.interpark.com/product/BookDisplay.do?_method=detail&sc.prdNo=240925953&gclid=CjwKCAjw7IeUBhBbEiwADhiEMXFmH8Y8NnzqZQKMAZ_PRFP2R3IGDM1kn1wAlw0KOut8SrVb7l9C-RoCduAQAvD_BwE">자바 ORM 표준 JPA 프로그래밍</a>
이라는 유명한 책을 주문했습니다. <br>
다음에는 자바 ORM 표준 JPA 프로그래밍에 대해 리뷰해보겠습니다. 항상 성장하는 개발자가 되겠습니다.


