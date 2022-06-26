# assignment
2022/강화학습/아타리

dqn을 이용해 pong만을 돌려보았다.

먼저 코드 흐름 설명에 앞서 DQN에 사용될 신경망은 convolution을 3번 사용하고 fully connect를 2번 사용하여 사용될 action을 정한다.
코드는 https://github.com/jmichaux/dqn-pytorch 이곳의 코드를 참고하였다.

코드 흐름
주요 설정은
lr = 1e-4
optimizer = Adam
scheduler = ReduceLROnPlateau
epoch = 500
을 사용함.

게임은 pong-v5를 사용했으며 dqn에 사용한 네트워크는 위에 설명한대로 진행하였다.
policy_net과 target_net 두개로 만들어지며 train시에는 policy_net만이 열심히 돌고
target_net 자체는 target을 업데이트 할 시에만 사용이 

기존의 코드에서는 wrappers.py의 make_env를 이용해 84 84의 크기로 atari 화면을 조정해서 사용했지만
과제에서는 크기 조정 없이 원래의 크기 그대로 바로 입력을 하였다.

train으로 들어가면 epoch에 해당하는 episode의 크기만큼 도는데 그 안에서도 t만큼의 step을 또 따로 반복하여 진행한다.
계속 반복하며 reward를 기록하고 wrappers.py의 EpisodicLifeEnv class에 있는 step함수를 이용하여 done의 여부를 알아낸다.
대부분의 경우 done이 아니기에 현재의 get_state로 가져온 state를 next_state로 넘겨 계속 반복을 취한다.

done이라면 next_state를 none으로 만들고 break를 걸어 episode 하나를 끝낸다.
그렇게 학습을 500번 진행하고 나면 바로 test를 진행하도록 하였다.

코드를 조금 코드를 조금식 수정하며 결과를 보았는데 500번은 너무 짧았는지 학습이 되는지 의문이 들 정도로 성능이 안 나왔다.
원본 코드의 wrappers.py의 EpisodicLifeEnv class가 qbert에 사용되던 함수처럼 보이는데 그걸 그대로 들고와서 사용해서 그런건지 아니면 최적의 하이퍼 파라미터를 못 찾은 건지 잘 모르겠다.


wandb reward 추이 : https://wandb.ai/paztec/RL/workspace
