Paper Implementation 1 (PI_1)

제목 : Tacotron2모델을 이용한 음성합성 프로젝트 입니다 (PyTorch)\

기간 : 02/08 ~ 02/20

dataset : LJ Speech 데이터셋을 사용하였습니다.한국어 데이터셋은 학습하는데 너무나 오래걸려 GPU문제등 여러가지 문제 때문에 사용하지 못하였습니다.

내용 : torch_hub_LJ_Speech 폴더에 들어가면 torch_hub를 이용하여 타코트론2 모델과 waveglow 모델을 불러와 음성합성을 진행했습니다.\
      (tacotron2 + 보코더 모두 구현되어 있다.)\
      또한 tacotron2_pynb.ipynb 파일 에서 논문을 읽고 레퍼런스 코드를 참조하여\
      타코트론2 부분을 구현하였습니다.보코더 부분은 제외하였습니다.\
      논문에 대한 번역본도 paper 폴더에 올려 놓았습니다.

      합성한 음성입니다: https://blog.kakaocdn.net/dn/ODJIF/btrZ6Ie8yCV/KcBNw6mvHRhC6hYrTcpZkK/tfile.wav


모델설명:

타코트론2 모델은 2단계로 나뉘어서 음성을 생성합니다.
task1 : 텍스트로부터 Mel-Spectrogram을 생성하는 단계
task2 : Mel-Spectrogram으로부터 음성을 합성하는 단계

task1에서 text로 input 을 받아 Encoder를 지나 Attention과 Decoder가 서로 작동합니다. Mel-Spectrogram 출력됩니다.타코트론2 모델을 사용하였습니다
task2에서 Mel-spectrogram을 이용하여 음성합성을 합니다.

Encoder 역할 = Character로부터 특징을 추출하는 역할입니다
Encoder 과정 = Embedding Matrix를 통과해 전처리된 정수열은 Matrix형태로 변환되며 1D Conv(5x1) + Batch Norm + ReLU 를 지나 bi-LSTM 까지 도달 그 뒤 Attention 으로 이동합니다.

Attention 역할 = Decoder에서 사용할 정보를 Encoder에서 추출하고 할당합니다.
+TTS Attention 역할 = 매 시점 Decoder에서 사용할 정보를 Encoder에서 추출하고 할당하는 역할
+TTS 의 특징 = Decoder의 매 시점 에서 사용하는 정보는 이전시점 의 다음 정보를 사용

Decoder 역할 = Attention에서 얻은 정보와 이전 시점에서 생성된 Mel-spectrogram을 이용하여 현재시점의 Mel-spectrogram을 생성, 종료확률을 계산, 품질을 향상합니다
               이전 시점의 Mel-spectrogram은 Pre-Net을 통과하여 축약된 벡터가 됩니다. 이 벡터는 두 개의 층으로 구성된 Decoder LSTM에 활용됩니다.
               그 뒤 FC Layer 를 지나 현재시점의 Mel-Spectrogram이 생성됩니다.
    
Vocoer 역할 = mel-spectrogram을 이용하여 WaveNet은 MOL에 사용할 paramter를 생성합니다. 생성된 paramter를 이용하여 
             -2^15 ~ 2^15 + 1 사이의 숫자가 나올 확률인 mixture of logistic distribution를 생성하고 가장 큰 확률을 갖고 있는 값을 이용하여 waveform을 생성합니다.


+ 프로젝트 코드에 대한 자세한 공부+설명은 블로그에 올려놓았습니다 https://lim123.tistory.com/88 \

레퍼런스 = t아카데미 음성합성, https://tts.readthedocs.io/

ppt:
![image](https://user-images.githubusercontent.com/93918673/232313241-049b780a-8ad0-4fd0-bd00-2d38945881e4.png)
![image](https://user-images.githubusercontent.com/93918673/232313246-ee383bea-39b2-426e-8f9a-39ba25e3aee5.png)
![image](https://user-images.githubusercontent.com/93918673/232313252-6df1de40-8b98-43c6-b6c8-6c8a9c180493.png)
![image](https://user-images.githubusercontent.com/93918673/232313258-47875f28-3ddb-415f-be3f-791fc20c81af.png)
![image](https://user-images.githubusercontent.com/93918673/232313263-197dd66b-87fb-4cca-bc81-fe117dab6264.png)
![image](https://user-images.githubusercontent.com/93918673/232313268-8827e5a2-bb39-4808-8116-0371c1632c88.png)
![image](https://user-images.githubusercontent.com/93918673/232313273-f713ae57-9652-4868-a6fd-8f8e1a793721.png)
![image](https://user-images.githubusercontent.com/93918673/232313280-efd902f7-8f64-471e-8171-f66ddf7ce69b.png)
![image](https://user-images.githubusercontent.com/93918673/232313283-54cc5041-6fc8-40e8-aa08-a859f17e2014.png)

