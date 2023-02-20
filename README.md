# SideProject-4
Tacotron2를 이용한 음성합성 프로젝트 입니다 (PyTorch)
torch_hub_LJ_Speech 폴더에 들어가면 torch_hub를 이용하여 타코트론2 모델과 waveglow 모델을 불러와 음성합성을 진행했습니다.
또한 tacotron2_pynb.ipynb 에서 논문을 읽고 레퍼런스 코드를 참조하여
타코트론2 부분을 구현하였습니다.보코더 부분은 제외하였습니다.
논문에 대한 번역본도 깃허브에 올려 놓았습니다.

dataset : LJ Speech 데이터셋을 사용하였습니다.한국어 데이터셋은 학습하는데 너무나 오래걸려 GPU문제등 여러가지 문제 때문에 사용하지 못하였습니다.

합성한 음성입니다.: 

기간 : 02/08 ~ 02/20

모델설명:

타코트론2 모델은 2단계로 나뉘어서 음성을 생성합니다.
task1 : 텍스트로부터 Mel-Spectrogram을 생성하는 단계
task2 : Mel-Spectrogram으로부터 음성을 합성하는 단계

task1에서 text로 input 을 받아 Encoder를 지나 Attention과 Decoder가 서로 작동합니다. Mel-Spectrogram 출력됩니다.타코트론2 모델을 사용하였습니다
task2에서 vocoder 부

Encoder 역할 = Character로부터 특징을 추출하는 역할입니다
Encoder 과정 = Embedding Matrix를 통과해 전처리된 정수열은 Matrix형태로 변환되며 1D Conv(5x1) + Batch Norm + ReLU 를 지나 bi-LSTM 까지 도달 그 뒤 Attention 으로 이동합니다.

Attention 역할 = Decoder에서 사용할 정보를 Encoder에서 추출하고 할당합니다.
+TTS Attention 역할 = 매 시점 Decoder에서 사용할 정보를 Encoder에서 추출하고 할당하는 역할
+TTS 의 특징 = Decoder의 매 시점 에서 사용하는 정보는 이전시점 의 다음 정보를 사용

Decoder 역할 = Attention에서 얻은 정보와 이전 시점에서 생성된 Mel-spectrogram을 이용하여 현재시점의 Mel-spectrogram을 생성, 종료확률을 계산, 품질을 향상합니다
               이전 시점의 Mel-spectrogram은 Pre-Net을 통과하여 축약된 벡터가 됩니다. 이 벡터는 두 개의 층으로 구성된 Decoder LSTM에 활용됩니다.
               그 뒤 FC Layer 를 지나 현재시점의 Mel-Spectrogram이 생성됩니다.
    
Vocoer 역할 =  보코더 부분은 구현하지 않았습니다.

레퍼런스 = t아카데미 음성합성, https://tts.readthedocs.io/

