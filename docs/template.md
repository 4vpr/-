
#  [ 템플릿 메소드 ]

## 키워드 정리
추상클래스 : 상속을 통해 구현이 필요한 클래스  
추상 메서드 : 반드시 자식 클래스가 구현해야 하는 메서드  
훅 메서드 : 선택적으로 자식 클래스가 재정의할 수 있는 메서드  

## 탬플릿 메서드의 정의
탬플릿 메서드 패턴은 맥락을 공유하지만 구현 내용은 다른 메소드를 부모 클래스에서  
구현 없이 정의만 하고 상속받는 클래스를 만들어 해당 내용을 다르게 구현하는 것이다.  

## 예시
강의를 직접 들었을때는 이걸 어디다 쓰는거지... 했었는데 조금만 더 생각해보니 이것은 나에게 엄청 익숙한 패턴이었다.  
왜 이걸 바로 떠올리지 못 했나 싶을정도로 익숙했다.  
그것은 바로 모든 게임엔진에서 사용하는 오브젝트와 씬의 개념과 직접적으로 연관이 있기 때문  

### 게임엔진에서 쓰인다고?

Godot을 예로들면 모~든 오브젝트들은 전부 Node라는 것을 상속한다.  

Node가 가지고있는 훅 메서드는 다음과 같다  

**_enter_tree**: 노드가 씬 트리에 들어올 때 1회 호출  
**_ready**: 씬에 들어오고 모든 자식 노드가 준비된 뒤 1회 호출  
**_exit_tree**: 노드가 사라질때 호출됨  
**_process(delta: float)**: 매 랜더프레임마다 호출됨  
**_physics_process**: 매 물리계산마다 호출됨  
**_input(event: InputEvent)**: 모든 입력 이벤트를 받음 (키보드, 마우스 등)  

보면 맥락에 따른 아주 직관적인 훅 메서드들을 가지고 있는것을 알 수 있다.  
물론 추상메소드가 없는 점은 학습에 있어서 아쉽지만 이미 충분히 훌륭한 예시이다.  

### 됐고 파이썬 예제는?

최근에 zep 봇을 만들면서 실제로 템플릿 메소드를 사용했다.  
```
class Bot(threading.Thread):
    def __init__(self,bot_name,target_url,update_time = 0.3 , login = False,):

    ... 중략 ...

    def update(self): # 봇 마다 구현할 update 로직을 추상화
        pass

    def run(self):
        self.driver.get(self.target_url)
        time.sleep(2)
        for i in range(50):
            self.driver.find_element(By.CSS_SELECTOR, ".Input_input__NwYso input").send_keys(Keys.BACK_SPACE)
        
        ... 중략 ...
    
        while self.running :
            self.get_chats()
            self.update() # 여기서 호출 !
            time.sleep(self.update_time)

# 아래는 실 구현한 HelloBot의 예시이다

class HelloBot(Bot):
    def __init__(self,number):
        super().__init__(f"인사봇 {number}","https://zep.us/----")
    def update(self):
        for chat in self.new_chat:
            if chat.text == "안녕": # 채팅내용이 '안녕' 이라면
                self.public_chat(f"안녕하세요 {chat.name}님") # 안녕하세요 (채팅친 사람)님 이라고 출력
                # bot.whisper(chat.name,"우리 비밀친구할래?") # 사용자에게 귓속말 (로그인 필요)
                self.press_key("z") # z 키 입력 (찌르기)
```


이렇게 봇마다 다른 업데이트 로직을 구현 할 수 있었다.  

정리 부문은 이미 정의에서의 설명이 충분하기에 생략.  

