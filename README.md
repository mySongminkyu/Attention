# Attention
Attention is all you need

- Attention model 구조
  모델 설명에 앞서 대부분의 자연어 모델은 Encoder와 decoder로 구성이 되어있음.
  Encoder는 input으로 data를 받아 context vector로 변환 및 출력해주는 역할을 함.
  
  Decoder는 반대로 context vector를 input으로 입력 받아 output data를 출력
  이렇게 하는 이유는 정보를 압축함으로써 연산량 최소화 하는 것임.
  
  연산량은 최소화 할 수 있지만 정보 손실이 큼. (맨 마지막 RNN 셀의 반영이 젤 크기 때문)

  그래서 사용하는 것이 attention

  ![image](https://github.com/mySongminkyu/Attention/assets/132251519/90b0fd7b-ce8f-462e-bfa6-a470227050f2)


  ![image](https://github.com/mySongminkyu/Attention/assets/132251519/b35b51eb-ad2d-4ca5-9774-875f868bcc51)


  위는 RNN 구조 도식화 
  output으로 hidden state가 나오는 것을 볼 수 있는데 이 때문에 위의 Encoder Decoder 구조에서 각각의 out이 hidden state의 형태로 출력됨.
  
  유의할 점은 Encoder의 경우 모든 RNN셀의 hidden states들을 사용하는 반면, decoder의 경우 현재 RNN셀의 hidden state만을 사용.
  
  이유는 번역 attention을 예로 들면 Target sequence의 한 단어와 source sequence의 모든 단어의 attention 상관관계를 비교하기 때문임.
  (여기서 hidden state는 압축된 문맥으로 해석 가능)

  - Decoder hidden state : target sequence의 문맥
  - Encoder hidden state : source sequence의 문맥(모든 문맥을 활용하겠다.)

  Attention score -> Attention Value

  ![image](https://github.com/mySongminkyu/Attention/assets/132251519/1decd81a-0535-4424-9ff0-ae6e4f7ae450)

  위에서 구한 encoder hidden states와 decoder hidden state를 이용하여 attention score를 구함.
  
  Hidden state는 행렬이기 때문에 위의 그림과 같이 dot product를 해주면 상수 값이 나오게 되고 이 상수 값은 encoder의 RNN셀 수 만큼 나오게 됨.
  
  위의 경우는 score1부터 4까지 4개가 나오는데 이를  attention score라고 함.

  ![image](https://github.com/mySongminkyu/Attention/assets/132251519/5d84c10a-acb4-4c01-a97e-a5ee268f840d)

  앞에서 구한 attention score들을 softmax activation function에 대입하여 attention distribution을 만들어 줌. 이렇게 하는 이유는 각 score들의 중요도를 상대적으로 보기 쉽게 하기 위해서임.
  
  sofgmax함수는 어떤 변수를 0-1 사이의 값으로 만들어 주는데 이는 쉽게 말해서 확률화 한다고 생각하면 됨.
  
  즉 attention score들을 확률분포로 변환한다고 이해하면 되는데 이 다음 encoder hidden states들을 방금 구한 attention distribution에 곱하고 합해주어 attention value 행렬을 만들어 줌.
  
  즉 각 문맥들(hidden states)의 중요도(attention score)를 반영하여 최종 문맥(attention value)를 구한다고 생각하면 됨.

  ![image](https://github.com/mySongminkyu/Attention/assets/132251519/838038fe-2260-40c1-853d-f5139610f634)

  마지막으로 decoder의 문맥을 추가해주기 위해 decoder hidden state를 attention value 아래에 쌓아 줌. 이 과정을 concatenate라고 하는데 추가적으로 성능을 향상하기위해 tanh,softmax 등 활성함수를 이용하면 최종 output y가 나옴

  - 전체 메커니즘
 
    ![image](https://github.com/mySongminkyu/Attention/assets/132251519/36ac65eb-3ae4-49b7-9245-e0cdb2d53676)
