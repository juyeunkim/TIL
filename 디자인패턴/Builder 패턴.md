# Builder 패턴

> 복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게하는 패턴
>
> 생성자만을 통해서 인스턴스를 생성하는 어려움을 해결하기위해 고안



## 장점

- 객체들마다 들어가야할 인자가 각각 다를 때 유연하게 사용

  - 유연성 확보

  - 인자가 다른 생성자를 여러개 만들 필요가 없다

  - ~~~java
    new User("name", 28, 180, 150);
    new User("name", 28, 180, 150, 75);
    ~~~

- 무조건 setter 생성을 방지하고 불변객체로 만들 수 있다

  - 수정자 패턴(setter) 을 구현한다는 것은 불필요하게 확장 가능성을 열어놓는것
  - 클래스 변수를 `final` 로 선언하고 객체의 생성을 빌더에게 맡긴다

- 필수 argument를 지정할 수 있다

  - 필요한 데이터만 설정 가능

  - ~~~java
    User user = User.builder()
        		.name("name")
        		.height(180)
        		.iq(150).build();
    ~~~

    

- 가독성 높임



## 생성 방법

1. Builder 클래스 생성

   - ~~~java
     public final class Hero {
       private final Profession profession;
       private final String name;
       private final HairType hairType;
       private final HairColor hairColor;
       private final Armor armor;
       private final Weapon weapon;
     
       private Hero(Builder builder) {
         this.profession = builder.profession;
         this.name = builder.name;
         this.hairColor = builder.hairColor;
         this.hairType = builder.hairType;
         this.weapon = builder.weapon;
         this.armor = builder.armor;
       }
     }
     ~~~

   - ~~~java
      public static class Builder {
         private final Profession profession;
         private final String name;
         private HairType hairType;
         private HairColor hairColor;
         private Armor armor;
         private Weapon weapon;
     
         public Builder(Profession profession, String name) {
           if (profession == null || name == null) {
             throw new IllegalArgumentException("profession and name can not be null");
           }
           this.profession = profession;
           this.name = name;
         }
     
         public Builder withHairType(HairType hairType) {
           this.hairType = hairType;
           return this;
         }
     
         public Builder withHairColor(HairColor hairColor) {
           this.hairColor = hairColor;
           return this;
         }
     
         public Builder withArmor(Armor armor) {
           this.armor = armor;
           return this;
         }
     
         public Builder withWeapon(Weapon weapon) {
           this.weapon = weapon;
           return this;
         }
     
         public Hero build() {
           return new Hero(this);
         }
       }
     ~~~

   - ~~~java
     Hero mage = new Hero.Builder(Profession.MAGE, "Riobard").withHairColor(HairColor.BLACK).withWeapon(Weapon.DAGGER).build();
     ~~~

     

2. Lombok이용

   - `@Builder` 어노테이션



[[Spring] Lombok을 이용해 Builder 패턴을 만들어보자](https://zorba91.tistory.com/298)

[[Java] 빌더패턴을 사용해야 하는 이유](https://mangkyu.tistory.com/163)