#  [ 전략패턴 ]
알고리즘, 특정 함수들을 교체 가능한 객체로 >캡슐화<하고 클라이언트는 공통 인터페이스만 의존하도록 만드는 설계... 라고 하면 솔직히 이렇게 들으면 잘 모르겠다. 이해는 되지만 그래서 어떻게 쓰라는거지? 이해되지 않는다... 솔직히 책의 예시도 별로 와닿지가 않았다. 오리 시뮬레이터 게임의 구현을 예시로 들었는데 솔직하게 다음과 같은 이유로 좋은 예시라고는 생각이 들지 않았다.

- **문맥이 실전성이 없다.**
실제로 오리 시뮬레이터를 구현한다면 오리는 매 틱마다 특정 확률로 행동을 하게 될 것이고 그 행동들을 배열 형태로 가지고 있으며 틱마다 확률과 발동조건 쿨타임을 검증하는 공통된 알고리즘을 필요로 하며 시전시 할 행동에 대한 스크립트가 들어가 있을것이다. 전략패턴을 설명하기엔 뭔가 그냥 객체지향 프로그래밍 설명에 더 가까워 보이는 느낌.
- **문제가 너무 작다.**
그냥 코드 몇줄 고치면 되는 문제이고 사실 예시 상황의 스케일의 프로그램에 예시의 문맥이라면 그냥 코드 몇 줄 고치면 되는 문제이지 복잡한 상속도 필요가 없다.

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
하스스톤은 카드가 몇천가지가 넘는다. 이를 하나하나 elif로 구현한다면 어떻게 될까?
수도 없이 많은 elif를 거쳐야 할 것이다.


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



## 전략 패턴의 효율적인 부분 질문 관련

이 질문에 당황하여 답을 명확하게 하지 못 한 이유는 크게 두가지가 있다.

1. 이 패턴을 사용하지 않아서 하드코딩을 해 본 경험이 없다.  

사실 이건 내가 큰 프로젝트를 진행해본 경험이 없었기에 (올해들어 시작한 Dansu 이외엔 없다)  

항상 최소한의 구현만을 필요로 하기도 했고 항상 구현에만 초점을 두었지 유지보수와 클린코드에는 관심이 없었다.  

필자의 3년전 이야기를 해보자면 코딩을 본격적으로 독학하며 만든 첫 프로젝트였던 [이 게임](https://www.youtube.com/watch?v=YJCM6CXq954)의 [코드](https://github.com/4vpr/IVKey)는 노트를 제외한 모든 UI 요소들이 하드코딩 되어있다. (지금보면 매우 가관이다.)  

그래도 생성형 AI 없이(시기상 없었다.) 서칭만으로 구현한 첫 프로젝트라는것엔 자부심을 가지고 있다.  

2. if / elif 엄청 남발하게 될 정도로 복잡한 구현을 해본 적이 없다.  

나는 전체 프로그램을 구현한다기보단 스크립팅 위주의 게임개발을 해왔고 그 경험마저 적다.  

최근와서 OpenGL을 깡으로 사용하는 프로젝트를 시작했지만 이마저도 시작단계에 그친 상황.  

각설하고 효율성이라고 한다면 '조건 분기 난립(if else 남발)' 문제를 해결 할 수 있다고 쉽게 설명 가능하다.