## 1.강화 학습 이란

### 1.1 지도 학습과 강화학습
￼
지도 학습 : 지도자(정답)의 도움을 통해 학습
강화 학습 : 스스로 시행착오를 통해 학습
비지도 학습 : 정답을 학습하는 것이 아닌 분포등을 학습해서 생성

#### 지도 학습
* 학습데이터(인풋과 정답)- 학습 - 테스트 데이터(정답을 맞히고자 하는 데이터)

#### 강화 학습
* 시행착오를 통해 발전해 나가는 과정
* 순차적 의사 결정 문제에서 누적 보상을 최대화 하기 위해 시행착오를 통해 행동을 교정하는 학습 과정

### 1.2 순차적 의사 결정 문제(sequential decision making)
* 강화 학습이 풀고자 하는 문제
* 한 과정을 성공적으로 마치기 위해서 몇가지 의사 결정들을 순차적으로 해주어야 함
    * 어떤 행동을 하고, 그로 인해 상황이 바뀌고, 다시 어떤 행동을 하고.. 연이은 행동을 잘 선택해야 하는 문제

#### 순차적 의사 결정 문제의 예시
* 주식 투자에서의 포트폴리오 관리 - 순차적 의사 결정 문제
    * 주식을 사고 팔고 다른 주식을 사고, 시장 상황에 따라 리스크를 줄이고 기대 수익률을 올리는 방향으로 포트폴리오를 재구성 해야함
* 운전
    * 어느 도로를 이용할지, 어느 차선을 이용할지, 차의 속도를 변경할지, 정지를 할지.. 순차적 의사 결정
* 게임
    * 순차적 의사 결정의 정수
    * 캐릭터 선택, 위치, 플레이 스타일에 따라 상황이 계속해서 바뀜
    * 롤, 스타, 바둑, 체스, RPG …
    * 잘게 쪼개진 선택의 연속. 주어진 상황 안에서 최선의 선택을 내려야 함

강화 학습은 순차적 의사 결정 문제들을 풀기 위해 고안된 방법.

### 1.3 보상(reward)

보상(reward) : 의사결정을 얼마나 잘하고 있는지 알려주는 신호
누적 보상(cumulative reward) : 과정에서 받는 보상의 총합
* 강화학습의 목적 = 누적보상의 최대화.

#### 보상의 3가지 특징
1. 어떻게 x 얼마나 o
    * 보상을 높이기 위해 어떻게 해야되는지 알려주지 않고 얼마나 잘하고 있는지만 알려줌
    * 수많은 시행 착오를 통해서 보상을 더 많이 방법을 계속해서 스스로 찾음
    * 보상이 낮았던 행동은 덜하고, 보상이 높았던 행동을 더하면서 보상을 최대화해 나감
2. 스칼라
    * 스칼라 : 크기를 나타냄
    * 강화 학습은 스칼라 형태의 보상이 있는 경우에만 적용할 수 있다.
    * 도저히 하나만의 목표를 설정하기 어렵다면 그 문제에 강화 학습을 적용하는 것은 적절하지 못할 수 있다
        * 그 경우 최대한 문제를 단순화 하여 하나의 목표를 설정해야만 한다.
        * 잘 정해진 하나의 숫자로 된 목표를 잡는게 best
    * 스칼라 보상의 예시
        * 자산 포트폴리오 배분에서의 이득
        * 자전거 타기에서 넘어지지 않고 나아간 거리
        * 게임에서의 승리
3. 희소하고 지연된 보상
    * 보상이 희소(sparse)할 수 있고 지연(delay)될 수 있다.
    * 행동과 보상이 일대일로 대응할 경우 강화학습은 한결 쉬워짐
        * 행동에 대한 평가가 즉각적으로 이루어 지면 좋은 행동, 나쁜 행동을 가려내기 쉽기 때문
        * 보상이 지연되거나 행동의 빈도에 비해 훨씬 적게 주어지면, 행동과 보상의 연결이 어려워짐
            * 최근 이 문제를 해결하기 위해 강화 학습 연구에서도 밸류 네트워크 등의 다양한 아이디어가 등장
    * 지도 학습에서는 정답이 뒤늦게 주어지는 경우가 없지만 순차적 의사결정 문제에서는 시간의 흐름이 중요하고 그것으로 인해 보상의 지연이 가능하다.

### 1.4 에이전트(agent)와 환경(environment)
￼
* 순차적 의사결정 문제의 도식화

루프(loop) : Agent가 action을 취함 -> environment가 변함
순차적 의사결정 문제 : 루프의 반복

Agent : 강화학습의 주인공, 주체
Agent 입장에서의 루프 구성 3단계
1. 현재 상황 st에서 어떤 액션을 해야할지 at가 결정
2. 결정된 행동 at를 환경으로 보냄
3. 환경으로부터 그에 따른 보상과 다음 상태의 정보를 받음

상태(state) : 현재 상태에 대한 모든 정보를 숫자로 표현하여 기록해 놓은것
상태 변화(state transition) : 환경에 의해 상태가 변화. 행동의 결과를 알려주는 것

#### 환경이 하는 일
1. 에이전트로 부터 받은 액션 at를 통해서 상태 변화를 일으킴
2. 그 결과 상태는 st -> st+1
3. 에이전트에게 줄 보상 rt+1도 함께 계산
4. st+1과 r+1을 에이전트에게 전달  

하나의 루프가 끝났을때 한 틱(tick)이 지났다고 표현.
실제 세계는 앞의 그림과는 다르게 시간의 흐름이 연속적이지만, 순차적 의사결정 문제에서는 시간의 흐름을 이산적(discrete)으로 생각함.
그리고 그 시간의 단위를 틱, 타임 스텝(time step)이라고 부름

### 1.5 강화 학습의 위력
#### 강화학습의 매력 2가지

#### 병렬성의 힘
* 강화 학습은 경험을 쌓는 부분의 병렬성을 쉽게 증가시킬 수 있음
* 시뮬레이터 : 경험을 쌓는 부분
* 시뮬레이터를 추가하면 경험을 병렬적으로 쌓을 수 있다.
    * 혼자서 시행착오를 진행할 때보다 더 빠르게 지식을 습득할 수 있다.
* OpenAI에서는 Dota2에 강화학습을 적용할 때 시뮬레이션을 위해 256개의 GPU와 12만 8천 개의 CPU 코어를 사용하였다고 함.
    * 병렬적으로 경험을 쌓는 에이전트의 실제 경험 시간을 모두 합하면 180년에 해당
    * 사람이라면 불가능하지만 기계이기에 가능한 방법론
* 알파고도 마찬가지의 방법으로 학습됨
    * 심지어 대국을 할 때 실시간으로 주어지는 초읽기 시간에도 마찬가지로 적용됨

#### 자가 학습(self-learning)의 매력
* 사람을 뛰어넘는 것이 가능했던이유 : 자가학습에 기반을 둔 강화학습
* 사람이 알려준 지식을 잘 따라하는데 그친다면 지능이라고 부를 수 없다. 진정한 인공지능에 가까이 있다고 생각함