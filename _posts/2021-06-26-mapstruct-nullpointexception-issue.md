---
title: MapStruct nullPointException issue
category: dependency
tags:
  - Lombok
  - MapStruct
---

# MapStruct와 Lombok의 우선순위 문제 및 바인딩 문제

##### 해결방법은 [여기로](#해결)

##### 해결방법을 바로 보는 것도 좋으나 어떻게 발생하게 되었는지 읽어보는 것도 나쁘지 않을 것..이라 생각한다.

## 시작

객체매핑을 위해서 set() 해주는 수고를 덜기 위해 MapStruct를 프로젝트에 적용해보았다.

그러나 역시 스무스하게 적용되지 않았다. 딱히 의존성을 받고있는 것이 많은 것도 아닌데 말이다.

MapStruct의 공식문서를 보며 기본적인 사용방법대로 아주 기본적인 객체매핑을 시도했다.

하지만, MapStruct의 Mappers클래스에서 getMapper(someClass.class) 를 사용하여 매핑할 Mapper 오브젝트를 가져오는데 실패하면서 Mapper 가 NullPointException을 내뱉었다.

요상한 점은 빌드를 다시하면 NullPointException이 발생안하고 Mapper 오브젝트를 잘 가져올 때도 있다는 것.
처음엔 MapStruct가 불안정한 라이브러리인가? 라고 의심을 해봤으나.

수많은 사람들이 사용하는 것으로 미루어볼 때 라이브러리가 불안정해서 내가 기본적인 부분에서 문제가 생기는 가능성은 희박하다고 생각했다.

그렇게 문제를 해결하기 위한 삽질이 시작되었다.

공식문서에 이에 대한 해결방법은 제공되고있지 않았고 어느 오래된 블로그의 좋은 게시글들을 찾아다녔다.

## 의심

#### 첫번째 의심) Mapper Bean이 등록되지 않았는가?

이를 확인하기 위해 Mapper Bean이 안들어왔는지 Bean을 전부 출력하여 눈으로 확인했다. 그러나, 보란듯이 Bean은 정상적으로 생성되어 Spring Container에 들어가 있었다.
첫번째 의심 실패.

#### 두번째 의심) Mapper의 사용법이 잘못되었는가?

이를 확인하기 위해 공식문서를 다시 한번 살펴봤지만, 단하나도 다르지 않은 코드를 짜도 되었다 안되었다 하는 것은 여전했다.
두번째 의심 실패.

#### 세번째 의심) Mapper와 충돌하는 의존성이 있는가?

이를 확인하기 위해 우리의 친구 스택오버플로를 쥐잡듯 뒤져보았다. 결국, 찾아내었다. MapStruct는 Lombok과 충돌 가능성이 존재했다.
사실 충돌이라기 보다 어노테이션 프로세싱의 우선 순위(순서) 때문에 일어나는 것이었다.

많은 게시글에서 build.gradle안의 Lombok의 annotation processor 정의보다 윗줄에 mapstruct의 annotation processor를 정의하라고 되어있었다.
이를 실천했고, 문제는 해결되지않았다(?)

나도 저기까지 하면 문제가 해결될 줄 알았으나 더 뒤져보다보니 몇가지 의존성이 더 필요했다.
공식문서나 블로그를 찾아보던 개발 동료들은 여기까지 작성했을 것이다. (아마도)

```gradle
implementation 'org.mapstruct:mapstruct:1.4.2.Final'
annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.2.Final'

compileOnly 'org.projectlombok:lombok'
annotationProcessor 'org.projectlombok:lombok'
```

<br>

# 해결

하지만 이렇게 수순에 맞게 배치해준 것만으론 해결되지 않았다. 추가로 다음 의존성을 받아줘야했다.

```gradle
implementation 'org.projectlombok:lombok-mapstruct-binding:0.2.0'
annotationProcessor "org.projectlombok:lombok-mapstruct-binding:0.2.0"
```

즉 build.gradle의 결과는 다음과 같이 되었다.

```gradle
implementation 'org.mapstruct:mapstruct:1.4.2.Final'
implementation 'org.projectlombok:lombok-mapstruct-binding:0.2.0'

annotationProcessor "org.projectlombok:lombok-mapstruct-binding:0.2.0"
annotationProcessor 'org.mapstruct:mapstruct-processor:1.4.2.Final'

compileOnly 'org.projectlombok:lombok'
annotationProcessor 'org.projectlombok:lombok'
```

이제 Mapper 오브젝트를 가져올 때 어쩔땐 Null로 가져오고 어쩔땐 정상적으로 가져오던 문제가 해결되었다.
이 문제가 생기던 이유는 MapStruct가 Lombok이 Getter 와 Setter 혹은 Builder 어노테이션을 사용하여 컴파일 시간에 코드를 만들어내기 전에 매핑을 위한 Impl클래스를 만들어내면서 문제가 생기는 것 같다.

이를 lombok-mapstruct-binding:0.2.0 의존성을 받음으로써 매핑이 잘 이루어질 수 있도록 한 것.
