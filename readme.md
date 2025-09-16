# 프로그래밍에서의 디자인패턴
### 개요 - 인디게임 개발자 입장에서의 디자인 패턴에 대한 고찰
<b>디자인패턴</b>은 프로그램의 코드를 작성할때의 통용되는 개념이다.  
코드의 유지/보수/기능추가에 있어서 자주 발생하는 문제들을 사전에 방지하기 위한  
검증된 코드 구조이며 개발자들 끼리의 효율적인 소통에 있어서 자주 등장하는 코드구조에 대한 정의된 용어이기도 하다. (이게 핵심인 것 같다)  

문맥(Context) –> 문제(Problem) –> 해결(Solution) –> 결과(Consequences)의 과정을 갖는다.
각각을 서술하자면...  
문맥은 해당 패턴이 등장하는 상황이나 환경이다.  
문제는 그 문맥 안에서 반복적으로 발생하는 문제점이다.  
해결은 문제상황 안에서의 사용될 디자인패턴에 대한  고민과 적용이다.  
결과는 해법을 사용하고 난 뒤에 장점과 부작용이다.  

#### 인디 개발자의 딜레마?  
디자인패턴은 그 자체로 인디 개발의 딜레마 중 하나 이다.  

기획/개발/기능 추가 전부 스스로 하게 되며 유지보수 보다는 빠른 출시가 중요한 인디 개발자는 어쩌면 디자인패턴과는 거리가 멀다고 볼 수 있다.  

실제로 수 많은 명작 인디 게임들은 각종 안티패턴을 마다하지 않는다.  

과거에는 직접 렌더링 프레임워크부터 구축하고 게임루프를 직접 작성해야했기에 그 무엇보다 디자인패턴이 중요하였지만 스크립팅에 가까운 현대의 게임 개발에서는 사실 '패턴'이 들어갈 여지가 그리 크지 않다.  

이미 상용 게임엔진들이 즐비한 지금은 루프, 렌더링, 리소스 관리 같은 '전통적 패턴'을 내장했다. 스크립트 단위로 코드를 작성하게 되는 현재는 직접 설계 패턴을 적용할 지점이 크게 줄었다고 볼 수 있다.  


#### 그럼에도 배워야 하는 이유
안티패턴을 알고도 빠른 프로토타이핑/개발을 위해 사용하는것과 그냥 코딩에 미숙하여 안티패턴을 사용하는것은 그 결 부터 다르다고 볼 수 있다.  
그리고 정결된 디자인 패턴 준수가 꼭 개발속도와 상반관계인것도 아니다. 오히려 개발 속도를 올려주고 안티패턴이 불러오는 피로도를 줄일 수 있다.  
그리고 가장 중요한 이유 **남부끄럽지 않은 코드를 작성 할 수 있다**  

#### 실제 필자의 사례
나는 Dansu 프로젝트를 진행하며 Game이라는 거대한 싱글톤을 만든 **God Object** 라는 안티패턴으로 추가 기능 구현에 애를 먹었다.  
처음에는 전역에서 참조해야할 변수 몇 개만 가지고 있던 녀석이 설정/스코어/기타 전역 게임 오브젝트들을 전부 담았더니 코드가 몇 백줄이 넘어가게 되었고 함수를 수정하기 위해 전체 문서의 줄을 읽고있는 상황이 발생하였다.  
결국 코드를 기능 단위로 분리하는 리펙토링을 진행하게 되었다.  

## 내가 배운 디자인 패턴
###  [ 전략패턴 ]
알고리즘, 특정 함수들을 교체 가능한 객체로 >캡슐화<하고 클라이언트는 공통 인터페이스만 의존하도록 만드는 설계... 라고 하면 솔직히 이렇게 들으면 잘 모르겠다.  
이해는 되지만 그래서 어떻게 쓰라는거지? 이해되지 않는다... 솔직히 책의 예시도 별로 와닿지가 않았다.  
오리 시뮬레이터 게임의 구현을 예시로 들었는데 솔직하게 다음과 같은 이유로 좋은 예시라고는 생각이 들지 않았다.  

- **문맥이 실전성이 없다.**
실제로 오리 시뮬레이터를 구현한다면 오리는 매 틱마다 특정 확률로 행동을 하게 될 것이고 그 행동들을 배열 형태로 가지고 있으며 틱마다 확률과 발동조건 쿨타임을 검증하는 공통된 알고리즘을 필요로 하며 시전시 할 행동에 대한 스크립트가 들어가 있을것이다.  
전략패턴을 설명하기엔 뭔가 그냥 객체지향 프로그래밍 설명에 더 가까워 보이는 느낌.  
- **문제가 너무 작다.**
그냥 코드 몇줄 고치면 되는 문제이고 사실 예시 상황 정도의 문맥이라면 그냥 코드 몇 줄 고치면 되는 문제이지 복잡한 상속도 필요가 없다고 느껴졌다.
뭔가 전략 패턴을 설명하기 위한 연출해낸 상황이라는 인상을 받았고 그래서 전략패턴이 유효한가? 에 대한 질문을 계속 하게 되었다.  
날고 소리내는 Behavior 말고 행동의 종류가 수백가지 된다면 어쩔것인가?

... 등등의 이유로 나는 **TCG 게임인 하스스톤을 예로 들어 전략 패턴의 우월성**을 공부하였다  
하스스톤이 떠오른 이유는 지난 강의에서 게임에서 자주 등장하는 패턴이라는 설명을 들었기 때문이다.  
게임에서 자주 쓰인다는 설명을 듣고 조금은 아리송 하고 있었는데 (개발해본 게임에서는 필요 없는 패턴이었다)  
그때 무기마다의 효과... 까지 듣고는 이 예시를 떠올리지 않을 수 없었다.  

**문맥**
cast(card)를 이렇게 구현하였다고 치자.  
```
if card_id == "Fireball":
	대상에게 3 데미지
elif card_id == "Heal":
    대상 체력 +5
elif card_id == "Draw2":
    카드 2장 드로우
```

**문제**
하스스톤은 카드의 종류가 몇천가지는 가뿐히 넘는다. 이를 하나하나 elif로 구현한다면 어떻게 될까?  
수도 없이 많은 elif를 거쳐야 할 것이다.  
코드 가독성, 생산성 측면에서 이는 불편한 문제이다.  


**해결**
카드 사용(Cast)을 인터페이스(Effect)로 추상화 한다.  

-   **CastEffect 인터페이스**: `cast(caster, target, game_state)` 같은 함수를 정의.
-   **구체적 전략**:
    -   FireballCast: `target.damage(5)`
    -   HealCast: `target.heal(5)`
    -   DrawCast: `caster.draw(2)`
-   **Card 객체**: 비용, 이름, 그리고 어떤 `Effect`를 쓰는지만 보관.
    
실제 게임 로직은 카드를 이렇게 호출하게 된다. `cast_strategy.cast(player1, player2, game)`  

#### 알고보니 이미 사용하고 있었다 ?
하스스톤의 예시로 개념을 학습하고 난 뒤 바로 생각난 예시가 있었다.  
C#을 처음 배웠을때도 가장 먼저 구현해봤던것이 text-rpg였었기에  
python을 배운 최근에 똑같이 구현해 본 것. `i_was_bored.py` 의 일부이다.  
```
    def _initialize_skills(self):
        # 기본 데미지 스킬
        def damage(caster, target, skill):
            print(f"'{skill.name}' 발동... {target.name}의 살점을 도려낸다.")
            caster.deal_damage(target, caster.attack * (1 + skill.level/3) * skill.power, is_skill=True)

        # 낮은 레벨 성장력 (power를 높혀주세요)
        def pulverize(caster, target, skill):
            print(f"'{skill.name}' 발동... {target.name}의 뼈를 으깬다.")
            target.deal_damage(target, caster.attack * (1 + skill.level/5) * skill.power, is_skill=True)

        # 높은 스킬 성장력 attack * level * power
        def reaping(caster, target, skill):
            print(f"'{skill.name}' 발동... {target.name}의 영혼을 부순다.")
            target.deal_damage(target, caster.attack * skill.level * skill.power, is_skill=True)

        # 고정 데미지 level * power
        def fixed_damage(caster, target, skill):
            print(f"'{skill.name}' 발동... {target.name}을(를) 강하게 내려친다.")
            caster.deal_damage(target, (1 + skill.level / 3) * skill.power, is_skill=True)

        # 낮은체력 대상 2배 데미지
        def execute(caster, target, skill):
            print(f"'{skill.name}' 발동... {target.name}의 마지막 숨통을 끊는다.")
            damage = caster.attack * (1 + skill.level / 5) * skill.power
            if target.current_health < target.max_health * 0.5:
                time.sleep(0.5)
                print(f"{target.name}의 낮은 생명은 더욱 큰 피해를 받는다.")
                damage *= 2
            caster.deal_damage(target, damage, is_skill=True)

        # 고급 스킬 (power값 크게)
        def advance_damage(caster, target, skill):
            print(f"'{skill.name}' 발동... {target.name}을(를) 향해 일격을 날린다.")
            caster.deal_damage(target, caster.attack * (1 + skill.level/2) * skill.power, is_skill=True)

```

이렇게 스킬 효과를 따로 함수로 구현하고 스킬마다 갈아 끼우는 캡슐화를 하여 구현을 하였다.  
이미 사용해본 개념이라 왜 필요 한 것인지 바로 알 수 있었다.  

#### 요약  
- 특정 행위(함수, 메소드,알고리즘 아무튼)를 갈아 끼울 수 있는 형태로 캡슐화 한다. (예: 카드의 발동)  
- 행위의 교체 가능성을 염두할때 사용한다. (카드마다 다른 효과를 구현해야함)  
