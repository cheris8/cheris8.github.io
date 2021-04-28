DNN은 기본적으로 layer라는 모듈을 조립하듯이 이어붙이는 구조를 가집니다.
여기서 layer의 종류는 다양하게 존재하는데 CONV layer, FC layer(Affine layer), BN layer, Pooling layer, activation 등이 존재합니다.
 
이번에 pooling에 대해서 이야기하고자 합니다.
도대체, Pooling은 왜 사용할까?
 
Pooling은 sub sampling이라고 합니다.
sub sampling은 해당하는 image data를 작은 size의 image로 줄이는 과정입니다.
 
pooling은 CNN기준으로 이야기하자면 CONV layer와 Activation을 거쳐 나온 output인 activation feature map에 대하여 technique을 적용합니다.
 
pooling의 종류로는 
- Max pooling (CNN에서 주로 사용, 해당 receptive field에서 가장 큰 값만을 고름)
- Average pooling (receptive field안에 존재하는 parameter 평균값만)
- Stochastic pooling
- Cross channel pooling 
- etc ...

Pooling을 사용하는 이유는 simple합니다.
앞선 layer들(CONV layer, Activation, etc... )을 거치고 나서 나온 output feature map의 모든 data가 필요하지 않기 때문입니다.
다시말해, inference(추론, ex. 사과를 보고 사과라고 맞추는 것)를 하는데 있어 적당량의 data만 있어도 되기 때문이다.
 
 
*Pooling의 효과
1. parameter를 줄이기 때문에, 해당 network의 표현력이 줄어들어 Overfitting을 억제
2. Parameter를 줄이므로, 그만큼 비례하여 computation이 줄어들어 hardware resource(energy)를 절약하고 speedup
 
 
*Pooling의 특징
1. training을 통해 train되어야 할 parameter가 없다.
2. Pooling의 결과는 channel 수에는 영향이 없으므로 channel 수는 유지된다. (independent)
3. input feature map에 변화(shift)가 있어도 pooling의 결과는 변화가 적다. (robustness)